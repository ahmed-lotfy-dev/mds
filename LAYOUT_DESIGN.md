# Portfolio Layout Design - Handmade Artist (Masks & Props)

## Core Problem
An artist's work IS the main selling point. The layout must:
- ✅ Make photos the HERO of the page
- ✅ Minimal text distraction
- ✅ Professional, gallery-like feel
- ✅ Mobile-first (most traffic from phones)
- ✅ Fast loading (image-heavy site)

---

## RECOMMENDED LAYOUT: "Gallery-First" Modern Portfolio

### Page Structure (Single-Page vs Multi-Page?)
**BEST: Multi-Page with Clear Navigation**
```
Landing Page (Hero)
├── Portfolio Gallery
├── About the Artist
├── Services/Commissions
├── Testimonials
├── Contact/CTA
└── Footer with Social Links
```

---

## Detailed Layout Breakdown

### 1️⃣ HERO SECTION (Landing)
```
┌─────────────────────────────────────┐
│                                     │
│   Large Hero Image (Full Width)    │
│   or Video of her working          │
│                                     │
│      "Handmade Masks & Props"       │
│      "Custom Commissions Welcome"   │
│         [Browse Work CTA]           │
│                                     │
└─────────────────────────────────────┘

Design Tips:
- Full viewport height (100vh)
- High-quality image (her best mask/prop)
- Text overlay: white/gold text, centered
- Scroll indicator → "See More" arrow
- Mobile: Reduce height to 60-70vh
```

**Why:** Immediate visual impact. Artist's work speaks louder than words.

---

### 2️⃣ PORTFOLIO GALLERY (Main Section)
```
┌──────────────────────────────────────┐
│  "Featured Works"                   │
├──────────────────────────────────────┤
│                                      │
│  [All] [Masks] [Props] [Costumes]   │  ← Filter tabs
│  [Custom] [Collaborations]          │
│                                      │
├──────────────────────────────────────┤
│                                      │
│  ┌──────┐  ┌──────┐  ┌──────┐       │
│  │ Img1 │  │ Img2 │  │ Img3 │       │
│  │      │  │      │  │      │       │
│  └──────┘  └──────┘  └──────┘       │
│                                      │
│  ┌──────┐  ┌──────┐  ┌──────┐       │
│  │ Img4 │  │ Img5 │  │ Img6 │       │
│  │      │  │      │  │      │       │
│  └──────┘  └──────┘  └──────┘       │
│                                      │
│  [Load More] or Infinite Scroll      │
│                                      │
└──────────────────────────────────────┘

Grid Layout Options:
- Desktop: 3 columns (best for masks - square/portrait)
- Tablet: 2 columns
- Mobile: 1-2 columns
- Aspect Ratio: 1:1 or 4:5 (portrait)
```

**Best Grid Layouts for This:**

#### Option A: **Masonry Grid** (Pinterest-style)
```
✅ Pros:
- Shows variety of sizes
- Natural, artistic feel
- Works with different image ratios
- Very modern/trendy

❌ Cons:
- Can feel chaotic
- Harder to navigate on mobile
```

#### Option B: **Uniform Grid** (3-column) ⭐ BEST
```
✅ Pros:
- Clean, professional
- Easy to scan
- Consistent, gallery-like
- Scales beautifully to mobile (2→1 column)
- Best for portfolio showcasing

❌ Cons:
- Might waste space with varied image sizes
- Less organic feel
```

#### Option C: **Alternating Layout**
```
Large image left + 2 small right
Then reverse...
Very eye-catching but complex on mobile
```

**My Recommendation: Option B - Uniform Grid**

---

### 3️⃣ LIGHTBOX/MODAL IMAGE VIEWER
```
When user clicks image:

┌─────────────────────────────────────┐
│  ✕ [Close button]                   │
├─────────────────────────────────────┤
│                                     │
│                                     │
│        [Large Image]                │
│        (Can scroll/arrow navigate)  │
│                                     │
│                                     │
├─────────────────────────────────────┤
│  "Venetian Plague Mask"            │
│  Materials: Leather, Feathers      │
│  Size: Custom                       │
│  [View Details] [Commission]        │
│                                     │
│  ← Previous      Next →             │
│                                     │
└─────────────────────────────────────┘

Features:
- Smooth zoom in animation
- Keyboard nav (arrow keys)
- Touch swipe on mobile
- High-res image display
- Related project info
- CTA buttons (Commission/Contact)
```

---

### 4️⃣ ABOUT SECTION
```
┌─────────────────────────────────────┐
│         "About the Artist"          │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────┐   ┌─────────┐ │
│  │  Portrait Img   │   │  Bio    │ │
│  │  (circular)     │   │  Text   │ │
│  │                 │   │         │ │
│  └─────────────────┘   │ - Years │ │
│                        │   of exp│ │
│  Split: Image | Text   │ - Awards│ │
│  (Mobile: Stack)       │ - Skills│ │
│                        │         │ │
│                        └─────────┘ │
│                                     │
└─────────────────────────────────────┘

Keep it SHORT:
- 2-3 paragraphs max
- Focus on her journey, not resume
- Include why people should hire her
- Personal touch (make it human)
```

---

### 5️⃣ SERVICES/COMMISSIONS SECTION
```
┌─────────────────────────────────────┐
│      "What I Offer"                 │
├─────────────────────────────────────┤
│                                     │
│  ┌──────────┐ ┌──────────┐        │
│  │  Custom  │ │ Costume  │        │
│  │  Masks   │ │ Rentals  │        │
│  └──────────┘ └──────────┘        │
│                                     │
│  ┌──────────┐ ┌──────────┐        │
│  │   Props  │ │Workshop/ │        │
│  │  & Armor │ │ Teaching │        │
│  └──────────┘ └──────────┘        │
│                                     │
│  Grid of service cards with:        │
│  - Icon or small image             │
│  - Title                           │
│  - Short description               │
│  - [Learn More] CTA                │
│                                     │
└─────────────────────────────────────┘
```

---

### 6️⃣ TESTIMONIALS SECTION
```
┌─────────────────────────────────────┐
│    "What Clients Say"               │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────────┐│
│  │ ⭐⭐⭐⭐⭐                         ││
│  │ "Amazing work! So talented..."  ││
│  │ - John Doe, Costume Designer    ││
│  │ Avatar: [small img]             ││
│  └─────────────────────────────────┘│
│                                     │
│  ┌─────────────────────────────────┐│
│  │ ⭐⭐⭐⭐⭐                         ││
│  │ "Exactly what I imagined..."    ││
│  │ - Jane Smith, Event Producer    ││
│  │ Avatar: [small img]             ││
│  └─────────────────────────────────┘│
│                                     │
│  ← [Carousel or scroll] →           │
│                                     │
└─────────────────────────────────────┘

Tip: Show 2-3 per row (desktop)
     Scroll carousel on mobile
     Keep quotes SHORT and punchy
```

---

### 7️⃣ CONTACT/CTA SECTION
```
┌─────────────────────────────────────┐
│    "Ready to Commission?"           │
│    "Let's Create Something Great"   │
├─────────────────────────────────────┤
│                                     │
│        [Get in Touch CTA Button]    │
│                                     │
│    Or inline contact form:          │
│    ┌───────────────────────────┐    │
│    │ Name: [______]            │    │
│    │ Email: [______]           │    │
│    │ Project Desc:             │    │
│    │ [___________]             │    │
│    │ Budget Range: [dropdown]  │    │
│    │      [Send Message]        │    │
│    └───────────────────────────┘    │
│                                     │
└─────────────────────────────────────┘

Design:
- Eye-catching background color/image
- Large, clear CTA button
- Optional: inline form vs modal
```

---

### 8️⃣ FOOTER
```
┌─────────────────────────────────────┐
│                                     │
│  [Social Icons]                     │
│  Instagram | Facebook | TikTok      │
│                                     │
│  Quick Links:                       │
│  Home | Portfolio | About | Contact │
│                                     │
│  © 2024 [Artist Name]               │
│  Proudly Handmade                   │
│                                     │
└─────────────────────────────────────┘
```

---

## COMPLETE SITE MAP

```
HOME PAGE (Single-Page with Sections or Multi-Page?)
│
├── Navigation Bar (Sticky)
│   ├── Logo/Name
│   ├── Home
│   ├── Portfolio
│   ├── About
│   ├── Services
│   └── Contact
│
├── HERO
│   └── Full-width image + text overlay + CTA
│
├── PORTFOLIO SECTION
│   ├── Filter tabs (All, Masks, Props, etc.)
│   ├── 3-column grid gallery
│   └── Lightbox on click
│
├── ABOUT SECTION
│   ├── Portrait image (circular)
│   └── Bio text + stats
│
├── SERVICES SECTION
│   └── 4 service cards
│
├── TESTIMONIALS SECTION
│   └── Carousel/scrollable testimonials
│
├── CTA/CONTACT SECTION
│   └── "Get Started" + inline form
│
└── FOOTER
    ├── Social links
    ├── Quick links
    └── Copyright
```

---

## DESIGN RECOMMENDATIONS

### Color Scheme Suggestions
```
For Masks/Props Portfolio:

Option 1: ELEGANT
- Background: Dark (charcoal/black)
- Text: White/Light gray
- Accent: Gold/Copper
- Why: Makes images POP, gallery-like

Option 2: ARTISTIC
- Background: Off-white/cream
- Text: Dark gray/black
- Accent: Deep jewel tone (emerald/purple)
- Why: Clean, professional, artistic

Option 3: BOLD
- Background: Deep blue/purple
- Text: White
- Accent: Gold/brass
- Why: Modern, sophisticated, high-end

I recommend: Option 1 (Dark with gold accents)
→ Professional + Images are the focus
```

### Typography
```
Headings: Modern serif or geometric sans-serif
Body: Clean sans-serif (Montserrat, Inter, Poppins)
Size: 
- Hero heading: 48-72px
- Section headings: 32-48px
- Body: 14-16px
- Mobile: Scale down by 40%
```

### Navigation
```
Desktop: Horizontal top nav (sticky when scroll)
Mobile: Hamburger menu (sidebar or dropdown)

Always include:
- Logo/brand on left
- Current page indicator
- Smooth scroll to sections
```

---

## IMAGE OPTIMIZATION

**CRITICAL for image-heavy site:**

```
Photography Tips:
✅ Multiple angles (front, back, side)
✅ Detail shots of craftsmanship
✅ Hero shots (full body/environment)
✅ Close-ups of textures
✅ Before/after (if custom commissions)
✅ Artist at work (humanize)

Technical Optimization:
- Use WebP format (smaller, fast)
- Lazy loading (load on scroll)
- Srcset for responsive images
- Compress without quality loss
- CDN delivery (Vercel/Netlify automatic)
```

---

## INTERACTION PATTERNS

### Hover Effects
```
Gallery items:
- Slight scale (1.05x) on hover
- Soft shadow appears
- Overlay with play icon (if video)
- Smooth transition (200-300ms)

Buttons:
- Change color on hover
- Subtle shadow
- Cursor pointer

Links:
- Underline on hover
- Color transition
```

### Animations (Subtle, Professional)
```
✅ DO:
- Fade in on scroll
- Gentle scale up on load
- Smooth page transitions
- Scroll-triggered content reveal

❌ DON'T:
- Auto-playing music/video
- Excessive animations (10+ on page)
- Flash/pop effects
- Anything that distracts from images
```

---

## RECOMMENDED COMPONENT STRUCTURE

```jsx
<App>
  <Navigation />
  
  <Hero />
  
  <PortfolioGallery>
    <FilterTabs />
    <ImageGrid>
      <ImageCard /> × 12
      <Lightbox />
    </ImageGrid>
  </PortfolioGallery>
  
  <About>
    <ArtistImage />
    <BioText />
  </About>
  
  <Services>
    <ServiceCard /> × 4
  </Services>
  
  <Testimonials>
    <CarouselSlider>
      <TestimonialCard /> × N
    </CarouselSlider>
  </Testimonials>
  
  <ContactCTA />
  
  <Footer />
</App>
```

---

## IMPLEMENTATION PRIORITY

### Week 1:
1. Header + Navigation
2. Hero Section
3. Portfolio Grid + Lightbox

### Week 2:
4. About Section
5. Services Section
6. Testimonials (optional Week 1)

### Week 3:
7. Contact Form
8. Footer
9. Mobile responsive polish
10. Performance optimization

---

## FINAL VERDICT

**Best Layout Type:** Gallery-First, Multi-Page Sections  
**Grid Style:** 3-Column Uniform Grid  
**Color Scheme:** Dark background with gold accents  
**Focus:** LET THE IMAGES SPEAK  
**Mobile:** Ensure beautiful on small screens (1-2 columns)  
**Performance:** Lazy load images, use WebP, CDN delivery

This layout puts the artwork FIRST and everything else second—exactly what a portfolio should do! 🎨
