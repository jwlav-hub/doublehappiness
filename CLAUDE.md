# CLAUDE.md — doublehappiness.life Project Context

> This file is the source of truth for the Double Happiness website project.
> Read it fully at the start of every session before writing any code.

---

## Project Overview

**Site:** [doublehappiness.life](https://doublehappiness.life)
**Repo:** [github.com/jwlav-hub/doublehappiness](https://github.com/jwlav-hub/doublehappiness)
**Deployment:** Netlify (auto-deploys from GitHub `main` branch)
**Stack:** Pure HTML + CSS + vanilla JS — no framework, no build step, no npm
**Owner:** Joseph Laviolette — CPA/MBA, 20+ years international finance, based in Las Vegas, extensive time in Southeast Asia and Macau

---

## What This Site Is Becoming

Double Happiness is being transformed from a personal travel photo blog into a **monetized travel platform** with two distinct content pillars and two AI-powered tools. The core brand identity (囍 logo, editorial aesthetic, Asia focus) stays intact.

### Two Content Pillars

| Pillar | File | Voice | Purpose |
|---|---|---|---|
| **Journal** | `journal.html` | Personal, narrative, immersive | Existing travel stories, cultural depth, photography |
| **Guides** | `guides.html` | Practical, user-friendly, business-clear | SEO-targeted, monetizable, trip-planning content |

### Two AI-Powered Tools

| Tool | File | Description |
|---|---|---|
| **Budget Michelin Finder** | `michelin.html` | User inputs a city → AI returns Michelin-recognized restaurants under a budget threshold |
| **Itinerary Engine** | `plan.html` | User inputs trip parameters → AI generates a styled, persona-driven custom itinerary |

---

## Current File Structure

```
doublehappiness/
├── css/
│   └── styles.css          ← Single stylesheet, do not split
├── images/                 ← All images live here
├── posts/                  ← Individual post HTML files
│   ├── banteay-kdei.html
│   ├── walking-in-phnom-penh.html
│   ├── koh-rong.html
│   ├── chinese-house-phnom-penh.html
│   ├── gates-of-angkor-thom.html
│   └── khmer-smile-bayon-temple.html
├── .gitignore
├── about.html
├── blog.html               ← RENAME to journal.html (see Step 1 below)
├── index.html              ← Homepage
├── netlify.toml            ← Netlify config
└── CLAUDE.md               ← This file
```

### Target File Structure (after transformation)

```
doublehappiness/
├── css/
│   └── styles.css
├── images/
├── posts/                  ← Journal posts (existing Cambodia content)
├── guides/                 ← New practical guide posts go here
├── .gitignore
├── about.html
├── journal.html            ← Renamed from blog.html
├── guides.html             ← NEW: Practical guides listing page
├── michelin.html           ← NEW: Budget Michelin Finder tool
├── plan.html               ← NEW: Itinerary Engine tool
├── index.html              ← Updated homepage
├── netlify.toml            ← Updated with redirect
└── CLAUDE.md
```

---

## Design System

### Fonts
```css
font-family: 'Playfair Display', Georgia, serif;   /* headings, brand */
font-family: 'Lato', system-ui, sans-serif;        /* body, nav, UI */
```
Both loaded from Google Fonts. Do not add new font families.

### Brand Mark
- Logo character: `囍` (Unicode double happiness / Shuāngxǐ)
- Logo HTML pattern used everywhere:
```html
<a href="index.html" class="logo">
  <span class="logo-char">囍</span>
  <span class="logo-text">
    <span class="logo-name">Double Happiness</span>
    <span class="logo-tagline">Asia Travel Blog & Gallery</span>
  </span>
</a>
```

### Color Palette

This is the definitive color system for the site. Apply via CSS custom properties in `:root` at the top of `css/styles.css`. Do not hardcode hex values anywhere else — always reference the variable.

```css
:root {
  --color-dark-void:   #141616;  /* Primary background, darkest surfaces */
  --color-iridium:     #3D3C38;  /* Card backgrounds, secondary surfaces */
  --color-artillery:   #746D67;  /* Borders, muted text, dividers */
  --color-equilibrium: #A49F9D;  /* Subheadings, captions, placeholder text */
  --color-falu-red:    #7F1D1A;  /* PRIMARY ACCENT — 囍 logo mark, links, buttons, hover states */
  --color-white:       #F5F3F0;  /* Body text on dark backgrounds (warm white, not pure) */
}
```

#### How to Use Each Color

| Variable | Hex | Use |
|---|---|---|
| `--color-dark-void` | `#141616` | Page background, nav background, hero |
| `--color-iridium` | `#3D3C38` | Post card backgrounds, tool panels, footer |
| `--color-artillery` | `#746D67` | Borders, `<hr>` dividers, secondary nav text |
| `--color-equilibrium` | `#A49F9D` | Post meta dates, captions, section labels |
| `--color-falu-red` | `#7F1D1A` | **囍 logo-char, active nav links, buttons, `.read-more`, hover states, accent bars** |
| `--color-white` | `#F5F3F0` | Body copy, headings on dark backgrounds |

#### Critical: The 囍 Mark
The logo character must always render in Falu Red. This is both a brand and a cultural decision — red is the correct color for 囍 in Chinese tradition.

```css
.logo-char {
  color: var(--color-falu-red);
}
```

#### Color Rules
- Never use pure black (`#000000`) or pure white (`#ffffff`) — use Dark Void and the warm white instead
- Falu Red is an accent only — do not use it for large background areas or body text
- Maintain sufficient contrast: warm white text on Dark Void or Iridium backgrounds always passes WCAG AA
- Do not add new colors without updating this file first

### Component Patterns
- **Post cards:** `<article class="post-card">` with `.post-card-img` and `.post-card-body`
- **Section labels:** `<span class="section-label">Label Text</span>` above headings
- **Buttons:** `.btn-outline` for secondary, `.read-more` for post links
- **Page header:** `.page-header` with logo + nav + empty div (3-column grid)
- **Page banner:** `.page-banner` below header for page title sections
- **Sticky nav:** `id="stickyNav"` — appears after scrolling past 65% of hero height
- **Footer:** logo + footer-nav + footer-social + footer-copy

### Rules
- Keep all CSS in `css/styles.css`. Do not use `<style>` tags in HTML files.
- Keep all JS inline at bottom of `<body>` or in a single `js/main.js` file if it grows.
- Every new page must include the sticky nav, page header, and footer in the same pattern as existing pages.
- All images go in `/images/`. Use descriptive alt text always.
- Use `loading="lazy"` on all `<img>` tags except above-the-fold hero images.

---

## Navigation

### Current Nav (3 links — OUTDATED, needs updating)
```html
<a href="index.html">Home</a>
<a href="blog.html">Blog</a>
<a href="about.html">About</a>
```

### Target Nav (6 links — use this in all new/edited pages)
```html
<nav class="hero-nav" aria-label="Primary navigation">
  <a href="index.html">Home</a>
  <a href="journal.html">Journal</a>
  <a href="guides.html">Guides</a>
  <a href="michelin.html">Budget Michelin</a>
  <a href="plan.html">Plan My Trip</a>
  <a href="about.html">About</a>
</nav>
```

This nav appears in **two places** in `index.html` (hero nav + sticky nav) and **once** in all other pages (`.page-header` nav). Update both in index.html.

Also update the footer nav in every file:
```html
<nav class="footer-nav" aria-label="Footer navigation">
  <a href="index.html">Home</a>
  <a href="journal.html">Journal</a>
  <a href="guides.html">Guides</a>
  <a href="michelin.html">Budget Michelin</a>
  <a href="plan.html">Plan My Trip</a>
  <a href="about.html">About</a>
</nav>
```

---

## Transformation Roadmap

Work through these steps in order. Do not skip ahead.

### ✅ Step 0 — Audit Complete
Repo structure and existing files have been reviewed. Stack confirmed as raw HTML/CSS/JS.

### 🔲 Step 1 — Update Navigation + Apply Color System (DO THIS FIRST)

**1a. Apply the color system to `css/styles.css`:**
- Add the `:root` CSS custom properties block at the very top of the file (see Design System → Color Palette above)
- Replace all existing hardcoded hex color values throughout the file with the appropriate variable
- Ensure `.logo-char { color: var(--color-falu-red); }` is set
- Ensure accent elements (`.read-more`, `.btn-outline`, `.section-label`, active nav states) all use `var(--color-falu-red)`
- Make no other visual changes — variable substitution only, the site should look identical after this step

**1b. Rename and update navigation:**
1. Rename `blog.html` → `journal.html`
2. Update nav in `index.html` (both hero nav and sticky nav), `journal.html`, and `about.html`
3. Update footer nav in all files
4. Add redirect in `netlify.toml`:
```toml
[[redirects]]
  from = "/blog.html"
  to = "/journal.html"
  status = 301
```
5. Update all internal `href="blog.html"` links (including post cards on homepage) to `journal.html`

### 🔲 Step 2 — Build `michelin.html` (Budget Michelin Finder)
Full spec in Feature Specs below. Highest-impact quick win — build it early.

### 🔲 Step 3 — Build `guides.html` (Practical Guides Landing Page)
Listing page for practical travel content. Same layout as `journal.html` but with "Guides" branding. Starts with placeholder cards or a coming soon message.

### 🔲 Step 4 — Build `plan.html` (Itinerary Engine)
Full spec in Feature Specs below. More complex — build after michelin.html is working.

### 🔲 Step 5 — Update Homepage (`index.html`)
- Add a "Tools" section between the intro and blog posts sections
- Feature cards for Budget Michelin Finder and Plan My Trip
- Add a "Guides" section or at minimum a link to guides.html
- Update tagline/hero text to reflect expanded mission

### 🔲 Step 6 — Set Up Google Search Console + Analytics
Add GSC verification meta tag and Google Analytics 4 script to all pages.

### 🔲 Step 7 — Affiliate Infrastructure
Add the `/go/` redirect slugs to `_redirects` file (see Monetization Notes). Programs: Booking.com, GetYourGuide, SafetyWing, Airalo, Viator, TheFork. Add FTC disclosure to footer of all pages.

### 🔲 Step 8 — Email Capture
Add a ConvertKit or Beehiiv embed to the Plan My Trip output flow. The itinerary engine gates full output behind an email capture.

---

## Feature Specs

### Budget Michelin Finder (`michelin.html`)

**Concept:** User types a city → page calls Claude API → returns a list of Michelin-recognized restaurants at budget price points, with dining category, approximate cost, and practical tips.

**Origin story (use in the page copy):** Joseph had two Michelin-star meals in Osaka for $10–15 each. This tool exists to help any traveler find those experiences anywhere in the world.

**UI elements:**
- City input field (text)
- Budget tier selector (Under $20 / Under $50 / Any budget)
- Meal type selector (Lunch / Dinner / Either)
- Submit button → loading state → results list
- Each result card: restaurant name, Michelin distinction (Star / Bib Gourmand / Selected), cuisine type, estimated price range, brief description, practical tip

**Claude API call pattern:**
```javascript
const response = await fetch("https://api.anthropic.com/v1/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    model: "claude-sonnet-4-20250514",
    max_tokens: 1000,
    messages: [{
      role: "user",
      content: `List budget-friendly Michelin-recognized restaurants in ${city}.
Focus on Bib Gourmand and affordable Michelin-starred options under ${budget}.
Return JSON only: array of objects with fields:
name, michelin_distinction, cuisine, price_range, description, tip.
No preamble, no markdown, just raw JSON array.`
    }]
  })
});
```

**Important:** Never hardcode the API key. In production on Netlify, use a Netlify Function as a proxy to keep the key server-side. During development, the claude.ai artifact system handles auth automatically.

**Monetization hooks:**
- After results: "Book your table" → `/go/thefork`
- "Get travel insurance before you go" → `/go/safetywing`
- "Need an eSIM for this trip?" → `/go/airalo`

---

### Itinerary Engine (`plan.html`)

**Concept:** User fills out trip parameters → Claude API generates a fully styled, persona-driven custom itinerary written in the voice of the chosen travel persona.

#### Parameters

**Party Type:** Solo / Couple / Family / Group

**Budget Tier:** Shoestring / Value / Mid-Range / Luxury

**Travel Pace:** Packed / Balanced / Slow

**Duration:** Weekend / 1 Week / 2 Weeks+

**Interests** *(multi-select checkboxes — user picks all that apply)*

| # | Interest | Description for AI |
|---|---|---|
| 1 | Local Cuisine | Authentic regional food, market stalls, family-run restaurants, dishes you can't get at home |
| 2 | Health Food | Plant-based, organic, wellness-focused dining, juice bars, farm-to-table |
| 3 | Microbrews | Craft beer, local breweries, taprooms, regional beer culture |
| 4 | Cocktails | Craft cocktail bars, speakeasies, rooftop bars, local spirits and liqueurs |
| 5 | Historic | Ancient sites, ruins, UNESCO heritage, living history, museums, old quarters |
| 6 | Art | Galleries, street art, design districts, cultural institutions, public installations |
| 7 | Adventure | Trekking, climbing, extreme activities, adrenaline experiences |
| 8 | Hiking | Trail walking, national park routes, scenic day hikes, mountain paths |
| 9 | Water Sports | Diving, snorkeling, surfing, kayaking, sailing, island-hopping |
| 10 | Wildlife | National parks, safaris, marine life, birdwatching, conservation experiences |
| 11 | Photography | Golden hour locations, iconic viewpoints, architectural details, landscape composition |
| 12 | Street Scene | Markets, alleys, everyday local life, the unscripted texture of a neighborhood |
| 13 | Nightlife | Clubs, live music venues, night markets, evening culture and social scenes |
| 14 | Nature | Landscapes, forests, mountains, rivers, slow outdoor immersion |
| 15 | Shopping | Malls, designer retail, luxury goods, duty-free, brand-name destinations |
| 16 | Local Markets | Artisan goods, handmade crafts, street markets, neighborhood shops, authentic souvenirs |
| 17 | Relax | Spas, beaches, slow mornings, minimal itinerary, restorative and unhurried travel |
| 18 | Active | Cycling, running routes, fitness culture, outdoor workouts, active sightseeing |
| 19 | Social Scene | Communal dining, rooftop bars, hostel culture, local events and festivals where solo travelers naturally connect |
| 20 | Local Shops | Independent neighborhood stores, bookshops, specialty food stores, non-tourist retail |

#### Travel Personas

Organized from raw/immersive to aspirational/polished. Anthony Bourdain is always listed first, Condé Nast always last.

| # | Persona | Register |
|---|---|---|
| 1 | **Anthony Bourdain** | Raw, local, honest — street level, no filters |
| 2 | **Andrew Zimmern** | Adventurous, fearless — seeks the unfamiliar with deep cultural respect |
| 3 | **Paul Theroux** | Uncompromising, literary — slow overland travel, unsentimental observation |
| 4 | **Rolf Potts** | Philosophical, unhurried — vagabonding as a lifelong practice |
| 5 | **Pico Iyer** | Contemplative, interior — travel as self-examination and cultural meditation |
| 6 | **Michael Yamashita** | Visual, humanist — finds the soul of a place through its people and light |
| 7 | **Phil Rosenthal** | Joyful, warm, accessible — finds genuine delight in everything |
| 8 | **Condé Nast** | Aspirational, polished — best hotels, best tables, beautifully considered |

#### Persona System Prompt Descriptions

Use these exact descriptions when constructing the Claude API system prompt for itinerary generation:

- **Anthony Bourdain:** Raw, local, off the tourist trail. Street food over white tablecloths. Real neighborhoods, real people, no filters. Writes with honesty and a sharp eye for what makes a place authentic versus performed.
- **Andrew Zimmern:** Adventurous and fearless. Seeks unusual ingredients, forgotten culinary traditions, and experiences most travelers walk past. Approaches every culture with genuine curiosity and deep respect.
- **Paul Theroux:** Literary and uncompromising. Prefers trains, slow travel, and the places between destinations. Unsentimental, observational, deeply read. Suspicious of comfort but not ascetic.
- **Rolf Potts:** Philosophical and unhurried. Believes travel is not an escape from life but a deepening of it. Itineraries are loose, time is generous, and the point is absorption not accumulation.
- **Pico Iyer:** Contemplative and interior. Writes about displacement, the in-between, and what movement reveals about stillness. Finds meaning in airports, transit hotels, and the margins of places. The journey is always also inward.
- **Michael Yamashita:** Visual and humanist. Every recommendation is seen before it is experienced — the quality of light, the human face, the moment before and after the obvious shot. Follows ancient routes and finds the thread connecting past to present.
- **Phil Rosenthal:** Joyful, warm, and genuinely enthusiastic. Finds something wonderful in every meal, every encounter, every city. Accessible without being bland. Makes the reader feel invited rather than instructed.
- **Condé Nast:** Aspirational and polished. The best room, the best table, the most considered experience. Luxury without ostentation. Writes as though everything is a privilege worth honoring.

#### Output Format
Day-by-day itinerary with morning / afternoon / evening structure, written in the voice of the chosen persona. Affiliate-linkable hotel and activity recommendations woven in naturally. Do not break persona voice to insert links — integrate them as the persona would naturally reference them.

#### Email Gate
Show a full preview of Day 1 only. Gate Days 2+ behind a ConvertKit email capture: *"Get your full itinerary delivered to your inbox."*

---

## Monetization Notes

### Affiliate Link Redirect Pattern
Manage all affiliate links via Netlify `_redirects` file — never embed raw affiliate URLs in HTML. This allows updating a single line if a program changes your tracking code, and keeps URLs clean and trustworthy.

```
/go/safetywing     https://safetywing.com/?referral=YOURCODE        302
/go/airalo         https://www.airalo.com/?ref=YOURCODE              302
/go/booking        https://www.booking.com/?aid=YOURCODE             302
/go/getyourguide   https://www.getyourguide.com/?partner=YOURCODE   302
/go/viator         https://www.viator.com/?pid=YOURCODE              302
/go/thefork        https://www.thefork.com/?utm_source=YOURCODE      302
```

Replace `YOURCODE` with your actual affiliate tracking code after joining each program.

Use in HTML as:
```html
<a href="/go/safetywing" target="_blank" rel="noopener sponsored">
  Get travel insurance
</a>
```

### Priority Affiliate Programs

| Program | Category | Commission | Traffic Min | Notes |
|---|---|---|---|---|
| SafetyWing | Travel insurance | 10% recurring | None | Instant approval — set up first |
| Airalo | eSIM | 9% | None | Perfect for Asia content |
| Booking.com | Hotels | Varies | None | 3–5 day site review |
| GetYourGuide | Activities | 8% | None | Embed activity widgets in posts |
| Viator | Tours | 8% | None | Better SE Asia inventory than GYG |
| TheFork | Restaurants | Per booking | None | Primary CTA on michelin.html |

### Display Ads
Apply to Mediavine or Raptive once the site reaches 50,000 monthly sessions. Do not add Google AdSense at any point — it pays poorly and degrades the design.

### FTC Disclosure (Required by Law)
Add to the footer of every page containing affiliate links:
```html
<p class="footer-disclosure">
  Some links on this site are affiliate links. If you book or buy
  through them, I may earn a small commission at no extra cost to you.
</p>
```

---

## Brand Story

### Site Philosophy
Double Happiness is built around transformational, immersive travel — not the collection of destinations but the accumulation of perspective. The site is for travelers who come back different.

### About Page — Section 1: The Story Behind the Blog

Use this copy verbatim:

```
That first transformational journey — the trip that took you far enough from everything
familiar that you had to figure out who you were without any of the usual reference
points — the one that quietly rearranges your interior architecture and rewrites your
hard-coded paradigms is what Double Happiness is built around. Not as a one-time rite
of passage, but as a practice. A way of moving through the world that keeps expanding
what you thought you already understood.

Every destination has an interior life that only reveals itself to those willing to look
past the obvious. The meal that requires pointing at something unpronounceable. The
conversation that survives the language barrier. The moment a place hands you back a
slightly different version of yourself — a little less certain, a little more curious,
a little better equipped for the next one.

That feeling is available on every trip. This site is about learning to find it.
```

### About Page — Section 2: Understanding Double Happiness

Write this section in first person, specific to Joseph's experience. Cover:
- First encountering 囍 in Macau — finding it both funny and profound as an American
- The cultural meaning: 囍 (Shuāngxǐ) is two distinct joys arriving simultaneously, not happiness doubled
- The Song Dynasty origin story: a scholar on his way to the imperial exam falls ill, is nursed back to health by an herbalist's daughter, she gives him half a couplet as a challenge, he passes the exam and the Emperor completes the couplet — he writes 喜 twice to mark both joys arriving at once
- Why the name stuck and became the brand
- Do not genericize — this section is personal and specific to Joseph

---

## Content Strategy Notes

**Journal** (existing content): Cambodia-heavy (Angkor, Phnom Penh, Koh Rong). Next posts should expand to: Macau, mainland China, Thailand, Vietnam, Japan.

**Guides** (new content): Target low-competition long-tail keywords. Priority topics:
- Budget Michelin dining by city (Japan, Thailand, Singapore, Hong Kong)
- Visa logistics for Americans in Southeast Asia
- eSIM recommendations by country
- Budget breakdowns for 1–2 week trips in specific destinations
- Expat banking and money management while traveling Asia

**Voice distinction:** Journal = first person, literary, slow. Guides = second person, practical, skimmable with headers and bullet points.

---

## Coding Standards for This Project

- **No frameworks.** No React, Vue, Tailwind, Bootstrap. Raw HTML/CSS/JS only.
- **No build tools.** No webpack, vite, parcel. Files are served as-is by Netlify.
- **CSS in one file.** All styles in `css/styles.css`. Use comments to organize sections.
- **Semantic HTML.** Use `<article>`, `<section>`, `<nav>`, `<main>`, `<header>`, `<footer>` correctly.
- **Mobile-first.** Every new page must be responsive. Test at 375px width.
- **No inline styles.** Use CSS classes, never `style=""` attributes.
- **Consistent indentation.** 2-space indent throughout.
- **Alt text always.** Every `<img>` needs a descriptive `alt` attribute.
- **External links.** All links to third-party sites get `target="_blank" rel="noopener"`.
- **API calls.** Never expose API keys in client-side JS in production. Use Netlify Functions as a proxy. During development/prototyping, the claude.ai artifact system handles auth automatically.

---

## Netlify Configuration

Current `netlify.toml` — add to this, do not replace existing content:
```toml
# Redirect blog.html to journal.html (permanent)
[[redirects]]
  from = "/blog.html"
  to = "/journal.html"
  status = 301

# Future: proxy for Claude API calls (keeps key server-side)
# [[redirects]]
#   from = "/api/claude"
#   to = "/.netlify/functions/claude-proxy"
#   status = 200
```

---

## Session Startup Checklist

At the start of every Claude Code session working on this project:
1. Read this CLAUDE.md fully
2. Check which Step in the roadmap is next (look for 🔲)
3. Review the relevant existing file(s) before writing any new code
4. Match the existing code style exactly before adding anything new
5. Make changes **one file at a time** — confirm before moving to the next
6. Test at mobile width (375px) before considering anything done
7. Never hardcode hex colors — always use CSS custom properties from the `:root` block
8. Never hardcode affiliate URLs — always use `/go/` redirect slugs
