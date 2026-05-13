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

### Colors
Exact values are in `css/styles.css`. When in doubt, inspect that file first. Do not hardcode colors inline — use the existing CSS classes.

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

### 🔲 Step 1 — Update Navigation (DO THIS FIRST)
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
Full spec below. This is the highest-impact quick win — build it early.

### 🔲 Step 3 — Build `guides.html` (Practical Guides Landing Page)
Listing page for practical travel content. Same layout as `journal.html` but with "Guides" branding. Starts empty with a "coming soon" message or placeholder cards.

### 🔲 Step 4 — Build `plan.html` (Itinerary Engine)
Full spec below. More complex than Michelin — build after michelin.html is working.

### 🔲 Step 5 — Update Homepage (`index.html`)
- Add a "Tools" section between the intro and blog posts sections
- Feature cards for Budget Michelin Finder and Plan My Trip
- Add a "Guides" section or at minimum a link to guides.html
- Update tagline/hero text to reflect expanded mission

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
