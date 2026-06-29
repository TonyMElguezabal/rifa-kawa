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
| `--black`  | `#050505` | Page background               |
| `--surf`   | `#0F0F0F` | Card / section surfaces       |
| `--white`  | `#FFFFFF` | Primary text                  |
| `--muted`  | `#666666` | Secondary / label text        |

## Key sections

1. **Hero** (`#hero`) — Full-screen two-column. Left: title hierarchy + stats + CTA. Right: motorcycle photo frame. GSAP entrance animations on load.
2. **Specs** (`#specs`) — Sticky left panel, scrolling right specs grouped by category. ScrollTrigger reveals each group and its items with stagger.
3. **Videos** (`#videos`) — Two video slots. Replace `.video-slot` placeholder divs with `<iframe>` (YouTube) or `<video>` tags when ready.
4. **Contacto** (`#contacto`) — 3-step how-to, WhatsApp contact card, and the full 150-ticket number grid.

## Updating sold tickets

In the `<script>` block at the bottom, add sold ticket numbers to the `soldTickets` Set:

```js
const soldTickets = new Set([5, 12, 77]); // tickets 5, 12, 77 are sold
```

Sold tickets render with a strikethrough and muted color; available tickets appear in red.

## Adding the motorcycle photo

In the hero section, replace the `.img-placeholder` div:

```html
<!-- Before -->
<div class="img-placeholder">...</div>

<!-- After -->
<img src="kawasaki-636.jpg" alt="Kawasaki 636 Racing">
```

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

## Raffle details

- **150 tickets** at **$1,000 MXN** each
- Cause: para ayudar a la operación del Topo
- Draw: tómbola en vivo when all tickets are sold
- WhatsApp: 331 303 9642 (linked as `wa.me/523313039642`)
- Plates: Michoacán, all paid
