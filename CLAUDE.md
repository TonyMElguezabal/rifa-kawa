# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static single-page raffle site for a **Kawasaki ZX-6R 636 Racing 2006**. No build step — the entire site is `index.html` with inline CSS and JS.

## Stack

- Pure HTML / CSS / JavaScript (no framework, no bundler)
- **GSAP 3.12** (CDN) with ScrollTrigger for scroll-driven animations
- **Google Fonts CDN**: Bebas Neue (display), Oswald (headings/labels), Inter (body), Space Mono (mono/data)
- Open in browser directly: `open index.html`

## Color palette

| Token      | Hex       | Role                          |
|------------|-----------|-------------------------------|
| `--red`    | `#D10000` | Primary accent (Kawasaki red) |
| `--red-b`  | `#FF1A1A` | Bright red for values/numbers |
| `--red-d`  | `#8B0000` | Dark red                      |
| `--black`  | `#050505` | Page background               |
| `--surf`   | `#0F0F0F` | Card / section surfaces       |
| `--surf2`  | `#181818` | Secondary surface             |
| `--surf3`  | `#1F1F1F` | Tertiary surface              |
| `--white`  | `#FFFFFF` | Primary text                  |
| `--muted`  | `#666666` | Secondary / label text        |
| `--text`   | `#BBBBBB` | Body text                     |
| `--border` | `#1C1C1C` | Borders and dividers          |
| *(no var)* | `#F59E0B` | Amber — `.rsvd` ticket state  |

## Key sections

0. **Banner** (`#banner`) — Centered text-only section at the very top (above `#hero`). Background: `scontent/topo4.jpeg` via `.banner-bg` (blur 3px, brightness 0.38), overlaid by a red radial gradient `::before`. Animated by `bannerTl`, a `gsap.timeline()` chaining `.from()` calls — **not** the individual `gsap.to/from()` pattern used elsewhere.
1. **Hero** (`#hero`) — Full-screen two-column grid. Left: title hierarchy + stats + CTA. Right: 7-photo fade carousel (`Kawa1.png`, `kt1–kt6.jpeg`) inside `.hero-img-frame`.
2. **Specs** (`#specs`) — Sticky left panel (`.specs-pin`) + scrolling right column (`.specs-scroll`) with `.spec-group` blocks. ScrollTrigger reveals each group and its `.sg-item` rows with stagger.
3. **Videos** (`#videos`) — Two `.video-slot` placeholder divs (`#vs1`, `#vs2`). Replace with `<iframe>` or `<video>` when ready.
4. **Topo** (`#topo`) — "¿Quién es el Topo?" section. `scontent/topobg.jpeg` as a blurred/darkened full-section background. Two-column layout: left has a 6-photo carousel (`topo1–topo6.jpeg`), right has the bio text. Below the grid, a `.topo-accident` sub-section shows two hospital photos (`topohospital.jpeg`, `topohospital2.jpeg`) alongside the accident support text.
5. **Contacto** (`#contacto`) — Two-column grid. Left column: 3-step `.steps` list → `#pago` deposit card (BBVA account details) → `#contactCard` (WhatsApp). Right column: `.tickets-panel` with the dynamically rendered ticket grid.

## CSS patterns

- `.eyebrow` — shared Space Mono small-caps label used across all sections
- Diagonal cut corner via `clip-path`:
  - **20px cut** — `polygon(0 0, calc(100% - 20px) 0, 100% 20px, 100% 100%, 0 100%)` — used on `.hero-img-frame` and `.topo-frame`
  - **10px cut** — `polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 0 100%)` — used on `.btn-primary`
  - **12px cut** — `polygon(0 0, calc(100% - 12px) 0, 100% 12px, 100% 100%, 0 100%)` — used on `.topo-hosp-img`
- Responsive breakpoints: `960px` (stacks hero, specs, topo, contacto grids to single column) and `600px` (stacks hero boxes, shrinks ticket grid cells)
- `@media (prefers-reduced-motion)` collapses all animation durations to `0.01ms`

## Carousel pattern

Two carousels exist: hero (`.carousel / .carousel-slide / .carousel-btn / .carousel-dots / .carousel-dot`) and topo (`.topo-carousel / .topo-slide / .topo-cbtn / .topo-cdots / .topo-cdot`). Both use the same logic:
- Slides stack absolutely inside the frame; active slide has `opacity: 1`, others `opacity: 0`; transition is `0.75s ease`
- Arrow buttons appear on hover of the frame, turn red on their own hover
- Dots at the bottom center; active dot scales up in `--red-b`
- Auto-advance every 4.5s, pauses on `mouseenter`, resumes on `mouseleave`
- Touch swipe: 40px threshold, wraps both directions
- Topo carousel auto-hides arrows and dots when only 1 slide exists

When adding a new carousel: duplicate the CSS block and JS IIFE with new class names to avoid conflicts.

## GSAP animation patterns

All animations run once (`once: true` on ScrollTrigger). Two types:
- **Load animations** (banner, hero): `bannerTl.from()` timeline / individual `gsap.from()` / `gsap.to()` with sequential delays starting at ~0.35s
- **Scroll animations**: `gsap.to()` with `scrollTrigger: { trigger, start: 'top 83–88%', once: true }` — elements start with `opacity: 0` and an offset transform in CSS, then animate to natural state

**`gsap.to()` vs `gsap.from()` for load animations**: When an element has `opacity: 0` in CSS, always use `gsap.to(el, { opacity: 1, ... })` so opacity animates to 1. Using `gsap.from(el, { opacity: 0, ... })` when CSS already sets `opacity: 0` means the opacity stays at 0 (only the transform animates). `.hero-causa` and `.hero-note` currently use `gsap.from` with CSS `opacity: 0`, so their opacity doesn't transition — only the x/y offset does.

When adding a new scroll-animated element: set `opacity: 0` and a `transform` in CSS, then add a `gsap.to()` block in the `<script>` following the existing pattern.

## Photos

All photos live in `scontent/`:

| File | Used in |
|------|---------|
| `Kawa1.png` (also at repo root) | Hero carousel — slide 1 |
| `kt1–kt6.jpeg` | Hero carousel — slides 2–7 (HTML order: kt2, kt3, kt5, kt4, kt1, kt6) |
| `topo1–topo6.jpeg` | Topo section carousel |
| `topo4.jpeg` | Banner section background (blur 3px, brightness 0.38) |
| `topobg.jpeg` | Topo section background (blur 8px, brightness 0.18) |
| `topohospital.jpeg` | Topo accident sub-section |
| `topohospital2.jpeg` | Topo accident sub-section |

To add photos to the hero carousel: add `<img class="carousel-slide" src="scontent/FILENAME">` inside `#heroCarousel` and a matching `<button class="carousel-dot">` inside `.carousel-dots`.

To add photos to the topo carousel: add `<img class="topo-slide" src="scontent/FILENAME">` inside `#topoCarousel` and a matching `<button class="topo-cdot">` inside `.topo-cdots`.

## Ticket grid — Google Sheets integration

Tickets are loaded live from Google Sheets via **JSONP** (script tag injection — not `fetch()`, which fails from `file://` origins due to CORS).

- **Spreadsheet ID**: `12VfJZHUeVDf5I57lZgQO999yp8uJyZyFNK_zZAUCWw8`
- **Required sharing**: Google Sheets → Share → "Anyone with the link" → Viewer *(different from "Publish to web")*
- **Sheet structure**: Column B (B3:B152) = ticket number (1–150), Column D (D3:D153) = status
- **Valid status values in column D**: `Disponible` | `Apartado` | `Vendido`

| Status | CSS class | Visual |
|--------|-----------|--------|
| `Disponible` | `.av` | Red number |
| `Apartado` | `.rsvd` | Amber (#F59E0B) number |
| `Vendido` | `.sd` | Strikethrough, muted |

To update ticket statuses: edit column D in the Google Sheet. The page reloads data on every page load — no code changes needed.

## URL anchors

| Anchor | Scrolls to |
|--------|-----------|
| `#hero` | Hero / motorcycle photo |
| `#specs` | Technical specifications |
| `#videos` | Video section |
| `#topo` | ¿Quién es el Topo? |
| `#contacto` | How to buy a ticket |
| `#pago` | Bank deposit details (BBVA) |

## Adding videos

Replace a `.video-slot` placeholder:

```html
<!-- YouTube -->
<div class="video-slot">
  <iframe src="https://www.youtube.com/embed/VIDEO_ID" allowfullscreen></iframe>
</div>

<!-- Local MP4 -->
<div class="video-slot">
  <video src="video.mp4" controls></video>
</div>
```

The `.video-slot` uses `position: relative; overflow: hidden` so `iframe`/`video` must be `position: absolute; inset: 0; width: 100%; height: 100%` (already in CSS).

## Raffle details

- **150 tickets** at **$1,000 MXN** each
- Cause: para ayudar a la operación del Topo (Jorge)
- Draw: tómbola en vivo when all tickets are sold
- WhatsApp: 331 303 9642 (linked as `wa.me/523313039642`)
- BBVA: 153 342 0836 · CLABE: 012 320 01533420836 7 · Jose Antonio Munoz Elguezabal
- Plates: Michoacán, all paid
