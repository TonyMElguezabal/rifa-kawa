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

## Key sections

1. **Hero** (`#hero`) — Full-screen two-column grid. Left: title hierarchy + stats + CTA. Right: `Kawa1.png` motorcycle photo inside `.hero-img-frame`. GSAP entrance animations on load.
2. **Specs** (`#specs`) — Sticky left panel (`.specs-pin`) + scrolling right column (`.specs-scroll`) with `.spec-group` blocks. ScrollTrigger reveals each group and its `.sg-item` rows with stagger.
3. **Videos** (`#videos`) — Two `.video-slot` placeholder divs (`#vs1`, `#vs2`). Replace with `<iframe>` or `<video>` when ready.
4. **Contacto** (`#contacto`) — Two-column grid: left has 3-step `.steps` + `.contact-card`; right is `.tickets-panel` with the dynamically rendered ticket grid.

## CSS patterns

- `.eyebrow` — shared Space Mono small-caps label used across all sections
- `clip-path: polygon(0 0, calc(100% - 20px) 0, 100% 20px, 100% 100%, 0 100%)` — diagonal cut corner, used on `.hero-img-frame` and `.btn-primary`
- Responsive breakpoints: `960px` (stacks hero, specs, contacto grids to single column) and `600px` (stacks hero boxes, shrinks ticket grid cells)
- `@media (prefers-reduced-motion)` collapses all animation durations to `0.01ms`

## GSAP animation patterns

All animations run once (`once: true` on ScrollTrigger). Two types:
- **Load animations** (hero): `gsap.from()` / `gsap.to()` with sequential delays starting at ~0.35s
- **Scroll animations**: `gsap.to()` with `scrollTrigger: { trigger, start: 'top 83-88%', once: true }` — elements start with `opacity: 0` and an offset transform in CSS, then animate to natural state

When adding a new scroll-animated element: set `opacity: 0` and a `transform` in CSS, then add a `gsap.to()` block in the `<script>` following the existing pattern.

## Updating sold tickets

In the `<script>` block at the bottom, add sold ticket numbers to the `soldTickets` Set:

```js
const soldTickets = new Set([5, 12, 77]); // tickets 5, 12, 77 are sold
```

Sold tickets render with `.sd` class (strikethrough, muted color); available tickets use `.av` (red). The grid is built dynamically: 150 `<div class="tn av|sd">` elements are appended to `#ticketsGrid`.

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
- Cause: para ayudar a la operación del Topo
- Draw: tómbola en vivo when all tickets are sold
- WhatsApp: 331 303 9642 (linked as `wa.me/523313039642`)
- Plates: Michoacán, all paid
