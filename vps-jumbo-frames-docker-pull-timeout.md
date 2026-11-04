# VPS: Docker pulls timing out — jumbo frames (MTU 9000)

**Date:** 2026-11-04
**Host:** Oracle Cloud instance `139.185.55.63` (Ubuntu, `ens3`)
**Symptom:** `docker pull` / `docker compose pull` from GHCR and Docker Hub hung for minutes then failed with `net/http: TLS handshake timeout`. Same for large `scp` transfers. Small requests (git, curl to a tiny file) worked fine.

---

## TL;DR

The VM's network interface had **MTU 9000 (jumbo frames)** while the rest of the path to the internet only handles **1500**. Small packets got through, large TLS handshakes and image layers were silently dropped.

```bash
sudo ip link set dev ens3 mtu 1500
```

Persisted in `/etc/netplan/50-cloud-init.yaml`:

```yaml
network:
  version: 2
  ethernets:
    ens3:
      match:
        macaddress: "02:00:17:03:36:06"
      dhcp4: true
      set-name: "ens3"
      mtu: 1500   # was 9000
```

---

## How it was diagnosed

Check the interface MTU and compare it with the container network:

```bash
ip -o link show | head -3
```

Output showed the mismatch:

```
2: ens3:    ... mtu 9000 ...     # host NIC — wrong
3: docker0: ... mtu 1500 ...     # docker default
```

Then confirm that small requests succeed while the registry still fails:

```bash
# works: tiny request, small packets
curl -4 -o /dev/null -s -w '%{http_code} %{time_total}s\n' https://ghcr.io/v2/
# 401 0.46s

# fails: registry pull needs large TLS records / image layers
docker pull ghcr.io/<owner>/<image>:latest
# net/http: TLS handshake timeout
```

That split — small OK, large broken — is the signature of an MTU / PMTUD blackhole, **not** a Docker or registry outage.

Other red herrings that this explains:

- `docker build` failing to fetch `oven/bun` from Docker Hub with a TLS timeout.
- `scp` of a ~200 MB image tar timing out, while `scp` of a 400 KB source tarball succeeded.
- Pulls that "sometimes" worked — path MTU depends on the route taken.

---

## Why MTU 9000 breaks it

Jumbo frames must be supported end-to-end. The Oracle image ships `mtu: 9000` for the VNIC, but the upstream VCN gateway / internet path uses 1500. Path MTU Discovery (PMTUD) is supposed to negotiate this down, but when ICMP "fragmentation needed" messages are filtered, the sender keeps emitting 9000-byte packets that get dropped.

The TCP handshake itself may complete (small segments), then the first large TLS record disappears and the client waits until the handshake timeout.

---

## Fix

### 1. Runtime (immediate)

```bash
sudo ip link set dev ens3 mtu 1500
```

No reboot, no container restart needed. The next `docker pull` completed in seconds.

### 2. Persistence

Edit the netplan file that cloud-init generated:

```bash
sudo sed -i 's/^      mtu: 9000/      mtu: 1500/' /etc/netplan/50-cloud-init.yaml
sudo grep mtu /etc/netplan/50-cloud-init.yaml
```

Optionally apply it now (already applied at runtime):

```bash
sudo netplan apply
```

> If cloud-init ever regenerates the netplan file on re-provisioning, re-check this value.

### 3. Docker daemon (only if containers still see 9000)

`docker0` was already 1500, so nothing was needed here. If you ever see containers with MTU 9000, set it in `/etc/docker/daemon.json`:

```json
{ "mtu": 1500 }
```

then `sudo systemctl restart docker`.

---

## Verification

```bash
# interface is 1500
ip -o link show ens3 | cut -d' ' -f1-5

# registry reachable
curl -4 -o /dev/null -s -w '%{http_code}\n' https://ghcr.io/v2/   # 401 is expected

# image pull succeeds
docker pull ghcr.io/ahmed-lotfy-dev/egx-api:latest

# stack is healthy
docker compose -f /home/ubuntu/apps/egx-api/docker-compose.yml up -d
curl -s https://egx-api.alotfy.site/api/status
```

---

## Related fixes made in the same session (EGX API)

These were separate bugs that surfaced once the network worked.

### A. API image had no dependencies

`Cannot find module 'drizzle-orm/postgres-js'` on every start.

Cause: the image had been built from an **incomplete source tree** — root `package.json` was missing its dependency block and `apps/api/package.json` was absent, so `bun install` installed almost nothing.

Fix: always build from a full checkout. CI now builds from the repository root.

### B. Production replayed old migrations

`PostgresError: column "market" of relation "instruments" already exists`.

Cause: the `Dockerfile` ran `bun run db:generate` during the build. Drizzle hashes migration files, so regenerating changed the hashes and the production migrator treated old migrations as unapplied.

Fix:

- Removed `db:generate` from `apps/api/Dockerfile`. Migrations are committed and applied at container start.
- Production had no `drizzle.__drizzle_migrations` journal at all (the DB was provisioned without it). Baselined migrations `0000`–`0008` by inserting their SHA-256 hashes and journal timestamps, so only `0009`–`0011` ran.

### C. Deploy pipeline

- GitHub Actions builds both images and pushes to GHCR (`ghcr.io/ahmed-lotfy-dev/egx-api` and `egx-worker`).
- A deploy job SSHes to the VPS and runs `docker compose pull && docker compose up -d`.
- Pulls are retried 5× to survive transient registry timeouts.
- Required repo secrets: `VPS_HOST`, `VPS_USER`, `VPS_SSH_KEY`.

---

## Quick checklist for next time

1. `docker pull` timing out? → `ip -o link show` and check MTU **first**.
2. Small `curl` works but large transfers hang? → almost always MTU/PMTUD.
3. Container starts then exits with a missing module? → image built from an incomplete context; check `bun install` actually ran.
4. `column ... already exists` on boot? → migration hash drift; never regenerate migrations in the image build.
