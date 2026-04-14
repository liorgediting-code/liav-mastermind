# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file Hebrew RTL landing page for a sales & marketing mastermind event (ליאב וליאור). The entire site lives in `index.html` — all CSS, HTML structure, and content are in one file. There is no build system, no framework, no JavaScript (beyond inline event handlers), and no package manager.

## Development

Open `index.html` directly in a browser — no server or build step required.

To preview with live reload, any local HTTP server works:
```bash
python3 -m http.server 8080
```

## Architecture

**Single-file structure:** All styles (`<style>` block in `<head>`), HTML sections, a `<script>` block at the bottom, and inline comments are in `index.html`. The file is organized top-to-bottom as:
1. CSS — full design system using CSS custom properties (`--orange`, `--text-dark`, etc.)
2. HTML sections in page order: Spots bar → Hero → Problem → **Testimonials** → Agitate → What Is It → What We Learn → Benefits → About → Price → FAQ → Legal
3. `<script>` block at end of `<body>` — carousel logic (`carouselInit`, `carouselNext`, `carouselPrev`, `carouselGoto`, `carouselRender`)

**Design system:** Colors and spacing are controlled via CSS variables in `:root`. The primary brand color is `--orange: #F5700A`. Always modify colors through these variables, not inline.

**RTL layout:** The page uses `dir="rtl"` and `lang="he"`. All text is Hebrew. Text alignment and flex direction follow RTL conventions throughout.

**Section pattern:** Each `<section>` contains a `.container` div (max-width 820px). Section backgrounds alternate using inline `style="background: var(--section-bg);"` — do not add CSS rules that would globally alter section backgrounds.

**CTA buttons:** All CTA buttons link to either `https://mrng.to/kNNAEvwuIj` (external payment link) or `#payment` (internal anchor). The payment section has `id="payment"`.

**Testimonials:** Two side-by-side carousels inside `.testimonial-carousels` (grid, stacks to 1 column on mobile). Photo carousel (`id="photo-carousel"`, 2 slides) shows `.screenshot-card` items with `hadar.jpeg` / `hagi.jpeg`. Video carousel (`id="video-carousel"`, 6 slides) shows `.video-testimonial` items with YouTube iframes at a fixed `height: 420px`. Each carousel has a `.carousel-viewport` (overflow hidden), `.carousel-track` (flex, animated via `translateX`), and `.carousel-controls` (prev button · dots · next button). The JS state is keyed by short IDs `'photo'` and `'video'` — track element IDs are `photo-track` / `video-track`, dots containers are `photo-dots` / `video-dots`. To add a new video slide, copy an existing `.carousel-slide` block inside `#video-track`, increment `carouselInit('video', N)`, and add a matching dot button in `#video-dots`.

## Key Content Facts

- **Event details:** 28.04.2025, 15:00, "המלאכה 16, פארק אפק, ראש העין", 4 hours, 12 spots, 97 ₪
- **Hosts:** ליאב (sales coach, 1.5 years coaching, 5M+ ₪ in deals) and ליאור (marketing, 2 years experience)
- **Payment link:** `https://mrng.to/kNNAEvwuIj` (appears in multiple CTAs — update all occurrences together)
- **Contact:** WhatsApp `054-2352910` (ליאב), linked as `https://wa.me/972542352910`
