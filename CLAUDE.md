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
| **Journal** | `journal.html` | Personal, narrative, Bourdain-style | Existing travel stories, cultural depth, photography |
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
│   └── styles.css              ← Single stylesheet, do not split
├── images/                     ← All images (downloaded from Bluehost, local)
├── netlify/
│   └── functions/
│       └── claude-proxy.js     ← Server-side Anthropic API proxy
├── posts/                      ← Journal posts (Cambodia content)
│   ├── banteay-kdei.html
│   ├── buddhist-temple-angkor-park.html
│   ├── chinese-house-phnom-penh.html
│   ├── gates-of-angkor-thom.html
│   ├── khmer-smile-bayon-temple.html
│   ├── koh-rong.html
│   └── walking-in-phnom-penh.html
├── guides/                     ← New practical guide posts go here (empty)
├── .gitignore
├── about.html
├── guides.html                 ← Practical guides listing page
├── index.html                  ← Homepage (updated with Tools section)
├── journal.html                ← Journal listing page (renamed from blog.html)
├── michelin.html               ← Budget Michelin Finder AI tool
├── netlify.toml                ← Netlify config (blog→journal redirect + API proxy)
├── plan.html                   ← Itinerary Engine AI tool
└── CLAUDE.md                   ← This file
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

This is the definitive color system for the site. Apply it via CSS custom properties in `:root` at the top of `css/styles.css`. Do not hardcode hex values anywhere else — always reference the variable.

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

### ✅ Step 1 — Update Navigation + Apply Color System
- Dark editorial color system applied to `css/styles.css` (Falu Red accents, Dark Void base, Iridium surfaces)
- `blog.html` renamed to `journal.html`; 6-link nav added to all pages
- Footer nav expanded across all files; `netlify.toml` redirect added

### ✅ Step 2 — Build `michelin.html` (Budget Michelin Finder)
City/budget/meal-type form, calls `/api/claude` proxy, renders Michelin badge result cards, affiliate hooks.

### ✅ Step 3 — Build `guides.html` (Practical Guides Landing Page)
Coming-soon landing page with 4 placeholder guide cards.

### ✅ Step 4 — Build `plan.html` (Itinerary Engine)
7-parameter form, 5 persona radio cards, Day 1 teaser + email gate, affiliate hooks.

### ✅ Step 5 — Update Homepage (`index.html`)
Tools section added with Budget Michelin and Plan My Trip feature cards. Hero updated to "Asia Travel & Dining Platform". Copyright updated to 2025.

### 🔲 Step 6 — Set Up Google Search Console + Analytics
Add GSC verification meta tag and Google Analytics 4 script to all pages.

### 🔲 Step 7 — Affiliate Infrastructure
Before publishing new guide content, add affiliate links to a reusable include pattern (or just consistent HTML snippets) for: Booking.com, GetYourGuide, SafetyWing, Airalo.

### 🔲 Step 8 — Email Capture
Add a ConvertKit or Beehiiv embed to the Plan My Trip output flow. The itinerary engine gates its output behind an email capture.

---

## Feature Specs

### Budget Michelin Finder (`michelin.html`)

**Concept:** User types a city → page calls Claude API → returns a list of Michelin-recognized restaurants at budget price points, with dining category, approximate cost, and any practical tips.

**Origin story (use in the page copy):** Joseph had two Michelin-star meals in Osaka for $10–15 each. This tool exists to help travelers find those experiences anywhere.

**UI elements:**
- City input field (text)
- Optional: budget tier selector (Under $20 / Under $50 / Any budget)
- Optional: meal type (Lunch / Dinner / Either)
- Submit button → loading state → results list
- Each result card: restaurant name, Michelin distinction (star / Bib Gourmand / Selected), cuisine type, estimated price range, brief description, practical tip

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

**Important:** The API key is injected by Netlify environment variable or the claude.ai artifact system. Never hardcode the key. In production on Netlify, use a Netlify Function as a proxy to keep the key server-side.

**Monetization hooks:**
- After results: "Book your table with ease" → affiliate link to TheFork or OpenTable
- "Get travel insurance before you go" → SafetyWing affiliate link
- "Need an eSIM for this trip?" → Airalo affiliate link

### Itinerary Engine (`plan.html`)

**Concept:** User fills out trip parameters → Claude API generates a fully styled, persona-driven custom itinerary.

**Parameters:**
| Field | Options |
|---|---|
| Destination | Free text |
| Party Type | Solo / Couple / Family / Group |
| Budget Tier | Shoestring / Value / Mid-Range / Luxury |
| Style Persona | Bourdain / Zimmern / Phil Rosenthal / Rick Steves / Condé Nast |
| Travel Pace | Packed / Balanced / Slow |
| Interests | Food / History / Adventure / Nightlife / Nature / Art (multi-select) |
| Duration | Weekend / 1 Week / 2 Weeks+ |

**Persona descriptions for the system prompt:**
- **Bourdain:** Raw, local, off the tourist trail. Street food over white tablecloths. Real neighborhoods, real people, honest writing.
- **Zimmern:** Adventurous, fearless. Seeks unusual ingredients and forgotten culinary traditions. Respects local food culture deeply.
- **Phil Rosenthal:** Joyful, accessible, enthusiastic. Finds delight in everything. Family-friendly wonder without being bland.
- **Rick Steves:** Historical context, careful planning, cultural respect. Budget-conscious, educational, European-focused but adaptable.
- **Condé Nast:** Aspirational, polished. Best hotels, best tables, beautiful experiences. Luxury without being crass.

**Output format:** Day-by-day itinerary with morning/afternoon/evening structure, written in the voice of the chosen persona, with affiliate-linkable recommendations woven in naturally.

**Email gate:** Show a teaser of Day 1, then gate the full itinerary behind an email capture (ConvertKit embed).

---

## Monetization Notes

Every page that produces recommendations should include affiliate links. Priority programs to join:
- Booking.com Partner Hub (hotels)
- GetYourGuide Affiliate (activities)
- Viator Affiliate (tours)
- SafetyWing (travel insurance) — pays well, easy approval
- Airalo (eSIM) — highly relevant for Asia travel content
- TheFork / OpenTable (restaurant bookings — relevant for Michelin tool)

Display ads (Mediavine or Raptive): apply once the site reaches 50,000 monthly sessions. Do not add Google AdSense in the meantime — it pays poorly and clutters the design.

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

Current `netlify.toml` — add to this, don't replace:
```toml
# Add redirect when blog.html is renamed to journal.html
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
2. Check which Step in the roadmap is next
3. Review the relevant existing file(s) before writing any new code
4. Match the existing code style exactly before adding anything new
5. Test at mobile width (375px) before considering anything done
