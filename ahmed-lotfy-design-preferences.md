# Ahmed Lotfy — Design & Coding Preferences

> **Single source of truth** for all agents working on my projects.  
> Derived from: `selfTracker`, `egx-app`, `FoodOrdering`, `gym-saas`, `erp-system`, `ecommerce-backend`, `mission-control`, and all chat history.

---

## 1. CORE PHILOSOPHY

### 1.1 Product over Pretty
- **Premium = restraint, not decoration.** Strip everything non-essential.
- Every element must earn its place. If someone says "AI made that", it's failed.
- **Keyboard-aware first.** Inputs never hidden by soft keyboard (Android especially).
- **Fit the flow.** Mid-workout / on-the-go usage → fast loads, minimal taps.

### 1.2 Single Responsibility / File Size
- **No file > 300 lines.** Split before it grows.
- One component = one file. One hook = one file. One utility = one file.
- **Vite/React/Next.js:** Feature-based folder structure, not type-based.
  ```
  src/
    features/
      auth/
        components/
        hooks/
        api.ts
      workouts/
        components/
        hooks/
        api.ts
    shared/
      ui/
      hooks/
      utils/
  ```

### 1.3 Tech Stack Consistency
| Layer | Choice | Reason |
|-------|--------|--------|
| Language | TypeScript strict | Catch errors at compile time |
| Runtime | Bun (backend), React 19 + Expo 58 (mobile), Vite (web) | Fast, modern |
| DB | PostgreSQL + Drizzle ORM | Type-safe, migration-first |
| Auth | better-auth (email OTP + Google) | Unified across web/mobile |
| API | Hono + OpenAPI + openapi-typescript | End-to-end types |
| Styling | Tailwind v4 + CSS variables | Dark mode, theming, performance |
| State | Zustand (global), TanStack Query (server) | Simple, typed |
| Animation | Motion (Framer Motion) + Reanimated (RN) | Spring physics, reduced-motion |
| Icons | `@phosphor-icons/react` (web), `@expo/vector-icons` (RN) | One family per project |

---

## 2. DESIGN SYSTEM (from selfTracker DESIGN.md)

### 2.1 Color Strategy — "Two-Voice Rule"
**Primary (Emerald #10B981):** Primary actions, success, active nav, key metrics.  
**Secondary (Indigo #6366F1):** Charts, decorative surfaces, secondary accents.  
**Never both on same element. Neither dominates — they share the stage.**

```css
/* CSS Variables (Tailwind v4 + class-based dark mode) */
:root {
  --color-primary: #10B981;
  --color-secondary: #6366F1;
  --color-background: #F9FAFB;
  --color-surface: #FFFFFF;
  --color-surface-elevated: #FFFFFF;
  --color-card: #FFFFFF;
  --color-text: #111827;
  --color-text-secondary: #4B5563;
  --color-text-muted: #9CA3AF;
  --color-border: #E5E7EB;
  --color-error: #EF4444;
  --color-warning: #F59E0B;
  --color-success: #10B981;
}
.dark {
  --color-background: #06060A;
  --color-surface: #1C1F26;
  --color-surface-elevated: #232733;
  --color-card: #1C1F26;
  --color-text: #F8FAFC;
  --color-text-secondary: #B6BCC8;
  --color-text-muted: #6B7280;
  --color-border: #1A1A2E;
}
```

**Banned as defaults:**
- `#000` / `#fff` — tint every neutral toward brand hue
- Warm beige/cream + brass/oxblood + espresso (premium-consumer cliché)
- Gradient text (`background-clip: text`)
- Neon outer glows

### 2.2 Typography
- **Display:** System font, Black 900, `clamp(1.75rem, 5vw, 2rem)`, -0.03em tracking
- **Headline:** Bold 700, 1.25rem
- **Title:** Semibold 600, 1rem
- **Body:** Regular 400, 1rem, line-height 1.5, max 65-75ch
- **Label:** Black 900, 0.625rem, 0.12em tracking, **uppercase**

**Arabic/RTL:** `letter-spacing: 0 !important` on `[dir="rtl"] *`. Never `tracking-*` on Arabic text.

### 2.3 Elevation — "Shadow-Equals-Action Rule"
| Layer | Dark | Light | Shadow | When |
|-------|------|-------|--------|------|
| Canvas | #06060A | #F9FAFB | none | Base |
| Surface | #1C1F26 | #FFFFFF | micro (interactive) | Cards, inputs |
| Elevated | #232733 | #FFFFFF | medium | Modals, dropdowns |
| Premium | gradient + blur | gradient + blur | hero | Hero metrics, profile (max 1/view) |

**Rule:** Surface at rest = tonal color. Shadow = "interactive" or "elevated above flow".

### 2.4 Components

**Buttons:**
- Rounded-xl (12px), full-width default
- Primary: Emerald bg, white text — ONE per view
- Secondary: Indigo bg, white text — charts, data actions
- Outline: Transparent, Emerald text + border
- Ghost: Transparent, Emerald text
- Danger: Ember bg, white text
- Press: Spring scale 0.97–0.98. Disabled: opacity 70%.

**Cards:**
- Rounded-xl, 1px border
- Default: no shadow at rest
- Interactive: `shadow-sm`
- PremiumCard: BlurView + LinearGradient, **max 1 per view**

**Inputs:**
- Rounded-xl, 1px border
- Focus: border → Emerald, 2px
- Error: border → Ember
- Label: 10px uppercase above field

**Navigation:**
- Drawer: Dark Slate + indigo gradient top. Active = Emerald tint.
- Tabs: Bottom, icons. Active = Emerald, resting = Gravel.

---

## 3. FRONTEND ARCHITECTURE (React / Next.js / Expo)

### 3.1 File Structure
```
src/
  app/              # Expo Router / Next.js pages
  features/         # Domain features (auth, workouts, stocks, etc.)
    <feature>/
      components/   # Feature-specific components
      hooks/        # Feature-specific hooks
      api.ts        # API calls (typed via openapi-typescript)
      types.ts      # Feature types
  shared/
    ui/             # Reusable primitives (Button, Card, Input, etc.)
    hooks/          # Shared hooks (useAuth, useTheme, etc.)
    utils/          # Pure utilities (formatters, validators)
    constants/      # App-wide constants
```

### 3.2 State Management
- **Global:** Zustand stores in `shared/stores/` — one store per domain
- **Server:** TanStack Query — keys as constants, typed via `api.d.ts`
- **Local:** `useState` / `useReducer` only for ephemeral UI
- **Never** `useState` for continuous values (scroll, drag, physics) → use Motion `useMotionValue`

### 3.3 Animation
- **Motion (web) / Reanimated (RN)** — spring physics only
- `prefers-reduced-motion` honored automatically via `useReducedMotion()`
- **Banned:** `window.addEventListener('scroll')`, `requestAnimationFrame` touching React state, animating layout props (`top`, `left`, `width`, `height`)

### 3.4 Arabic/RTL Support
```css
/* Global */
[dir="rtl"] * { letter-spacing: 0 !important; }
/* Only allow tracking on Latin/numeric */
.tracking-latin { direction: ltr; unicode-bidi: embed; }
```

---

## 4. BACKEND ARCHITECTURE (Bun + Hono + Drizzle)

### 4.1 Project Structure
```
apps/api/
  src/
    index.ts              # App entry, middleware, routes
    routes/               # One file per domain
      instruments.ts
      admin-documents.ts
      ai.ts
    db/
      client.ts           # Drizzle client (singleton)
      schema/             # One file per table group
        market.ts
        documents.ts
        auth.ts
      migrations/         # Generated, committed
      migrate.ts          # Runtime migration runner
    lib/
      auth.ts             # better-auth config
      admin.ts            # Admin guard
      cache.ts            # Runtime cache helpers
      documents.ts        # PDF storage/read
      email.ts            # Resend wrapper
      logger.ts           # Pino
    scripts/
      generate-openapi.ts
      check-openapi.ts
      seed-admin.ts
```

### 4.2 API Design
- **Hono + Zod OpenAPI** — every route typed
- `openapi-typescript` generates `packages/contracts/openapi.json` → `apps/mobile/src/types/api.d.ts`
- **CORS:** Single `trustedOrigins` list shared between CORS + better-auth
- **Cookies:** `SameSite=None; Secure` in production for cross-origin dashboard → API
- **Auth:** better-auth with `isAdmin` additional field (`input: false`)

### 4.3 Database
- **Drizzle ORM** — schema in TypeScript, migrations generated + committed
- **Migration runner at container start** — never `db:generate` in Dockerfile
- **Partial unique indexes** for "one live row" patterns:
  ```sql
  CREATE UNIQUE INDEX ON stock_snapshots (instrument_id, period_end)
  WHERE superseded_at IS NULL;
  ```
- **Never delete** — supersede with new row + timestamp

### 4.4 Workers (Background Jobs)
- Separate `apps/worker` — same DB, different entry
- CLI with subcommands: `universe`, `quotes`, `bars`, `fundamentals`, `news`, `extract-document`, `stage-document`
- Scheduler runs daily at 04:30 Cairo time
- Python (pdfplumber) for PDF extraction → JSON → importer

---

## 5. DEVOPS & DEPLOYMENT

### 5.1 Docker
```dockerfile
# apps/api/Dockerfile — NO db:generate at build
RUN bun install
# Migrations applied at container start via entrypoint
```

### 5.2 CI/CD (GitHub Actions)
- Build → push to GHCR (`ghcr.io/ahmed-lotfy-dev/egx-{api,worker}`)
- Deploy job: SSH to VPS → `docker compose pull` (retry 5×) → `docker compose up -d`
- **Sequential:** Pull API first, restart, then worker (avoids timeout on small VPS)
- `docker image prune` runs detached (non-blocking)

### 5.3 VPS (Oracle Cloud, 2 vCPU, 1GB RAM)
- **MTU 1500** (fixed jumbo frames 9000 → TLS timeouts)
- Netplan persisted: `/etc/netplan/50-cloud-init.yaml`
- Caddy reverse proxy → API on 8106
- Postgres + Redis in Docker network
- `DOCUMENTS_DIR=/var/egx` volume for PDFs

### 5.4 Domains
- **API:** `https://egx-api.alotfy.site` (hardcoded everywhere)
- **Dashboard:** `https://egx.alotfy.site` (same parent domain — Safari 3rd-party cookie fix)
- **Vercel alias:** `https://egx-app.vercel.app` (kept for compatibility)
- Cloudflare DNS-only (proxy off)

---

## 6. CODING STANDARDS

### 6.1 TypeScript
- `strict: true`, `noUnusedLocals`, `noUnusedParameters`
- **Never** `any`. Use `unknown` + type guards.
- Zod for runtime validation → `z.infer<typeof schema>`

### 6.2 React
- **Function components only.** No classes.
- **Client Components** (`"use client"`) ONLY for interactivity (Motion, scroll, forms).
- Server Components by default (Next.js) or static (Vite).
- **Props interface** named `Props` or `ComponentNameProps`.

### 6.3 Naming
| Type | Convention |
|------|------------|
| Files | kebab-case (`stock-snapshots.ts`, `useAuth.ts`) |
| Components | PascalCase (`StockCard.tsx`) |
| Hooks | camelCase `use` prefix (`useAuth`, `useStockData`) |
| Constants | SCREAMING_SNAKE_CASE |
| DB columns | snake_case |
| API routes | kebab-case (`/api/instruments/{ticker}/history`) |

### 6.4 Error Handling
```typescript
// API routes: typed error responses
return c.json(
  { error: { code: "INSTRUMENT_NOT_FOUND", message: `No instrument matches ${ticker}` } },
  404
);

// Client: try/catch + user-facing message
try {
  await apiCall();
} catch (e) {
  setError(e instanceof Error ? e.message : "فشل التحميل");
}
```

### 6.5 Git
- **Conventional Commits:** `feat:`, `fix:`, `perf:`, `ci:`, `docs:`, `refactor:`
- **No secrets** — gitleaks pre-commit hook
- **PR description:** What + Why + Testing done

---

## 7. MOBILE (Expo + React Native)

### 7.1 Stack
- Expo Router (file-based routing)
- Uniwind (Tailwind v4 for RN)
- Reanimated 4 + Gesture Handler
- FlashList for performant lists
- MMKV for fast local storage

### 7.2 Patterns
- **Keyboard-aware:** `KeyboardAvoidingView` + `useKeyboardOffset` on every input screen
- **Safe areas:** `react-native-safe-area-context` everywhere
- **Fonts:** `@expo-google-fonts/inter` + `@expo-google-fonts/ibm-plex-sans-arabic` preloaded
- **RTL:** `I18nManager.forceRTL(true)` for Arabic

### 7.3 Data Fetching
- TanStack Query with `openapi-fetch` client (generated types)
- Cache keys as constants: `['instrument', ticker]`, `['instrument', ticker, 'history', range]`
- Intraday (1D): `refetchInterval: 60_000`

---

## 8. SPECIFIC PROJECT PATTERNS

### 8.1 EGX App (Current)
- **Admin Dashboard:** React + Vite + React Router, served from `apps/api/public/admin`
- **Mobile:** Expo Router + Uniwind, same better-auth flow (OTP + Google)
- **Financial Pipeline:** PDF upload → extract (pdfplumber) → stage (per-stock rows) → admin review → publish (supersedes old)
- **Stock Page:** TradingView fundamentals + EGX PDF snapshots (when published)
- **RAG:** Page-level embeddings from approved documents only

### 8.2 SelfTracker
- Dark-first, emerald/indigo dual accent
- PremiumCard for hero metrics (max 1/view)
- Local-first with PowerSync → Postgres
- Workout logging mid-session → minimal taps

---

## 9. QUICK REFERENCE FOR AGENTS

### Before Starting Any Task
1. **Read this file** — it's your context
2. **Check existing patterns** in the project — match them
3. **Single responsibility** — split files > 300 lines
4. **No AI defaults** — no purple gradients, no centered hero over dark mesh, no 3 equal cards

### When Designing UI
1. Declare **Design Read** in one line
2. Set **Three Dials** (Variance/Motion/Density) from brief
3. Pick **Color Strategy** (Restrained/Committed/Full/Drenched)
4. **Audit** before shipping: contrast, reduced-motion, RTL, keyboard, empty/error/loading states

### When Writing Backend
1. Schema first → migration → types flow to frontend
2. Single `trustedOrigins` for CORS + auth
3. Runtime cache with tags for invalidation
4. Never regenerate migrations in Docker build

### When Deploying
1. Build images in CI → GHCR
2. VPS pulls + restarts (sequential API → worker)
3. Verify health endpoints + CORS preflight
4. Check `docker logs` for migration errors

---

## 10. ANTI-PATTERNS (Auto-Reject)

| Pattern | Replacement |
|---------|-------------|
| `border-left-4` colored accent | Full border, background tint, or leading icon |
| Gradient text | Solid color, weight/size for emphasis |
| 3 equal feature cards | Asymmetric grid, bento, or scroll-snap |
| `useState` for scroll/drag | Motion `useMotionValue` / `useTransform` |
| `#000` / `#fff` | Tinted neutrals (zinc-950, gray-50) |
| Modal as first thought | Inline, progressive disclosure, sheet |
| Side-by-side identical cards | Vary size, mix content types |
| PremiumCard everywhere | Max 1 per view, deliberate accent |
| Em dashes in copy | Commas, colons, semicolons, periods |
| Fake-precise numbers (92%, 4.1×) | Real data or labeled "example" |

---

*Generated from analysis of all projects in `/mnt/hdd/projects/` and chat history.  
Update this file when patterns evolve — it's the contract for all agents.*