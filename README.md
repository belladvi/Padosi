# Padosi — Scroll-Driven Hero Page

A cinematic, scroll-scrubbed 3D hero page + full brand page for **Padosi** — a live,
today-only home-food menu for apartment societies. Home cooks post one live batch;
residents open one society link and order only what's still available.

**Live app:** https://padosi-self.vercel.app

## What this is
A single self-contained static page:
- An **Apple-style scroll film** (an AI-generated cook film sliced into WebP frames,
  scrubbed by scroll position on a `<canvas>`).
- A full **brand page** below it: live-menu phone mockup, manifesto, a 3D rotating
  food ring, how-it-works, for-cooks / for-residents, hygiene, stats, testimonials,
  FAQ, dual CTA, footer.

No build step. Vanilla HTML/CSS/JS with GSAP + Lenis (smooth scroll) from CDN, and
the Fraunces + Inter fonts from Google Fonts.

## Run locally
```bash
python -m http.server 4190
# open http://localhost:4190
```
Any static file server works (it must be served over http, not opened as a file://,
because the engine `fetch()`es the frame manifest).

## Files
| Path | What |
|------|------|
| `index.html` | The whole page — markup, styling, brand, section scripts |
| `main.js` | The scroll-film canvas engine (frame preload + scrub) |
| `frames/` | 339 WebP frames + `frames.json` manifest (the film) |
| `img/` | Section imagery + logo (`logo-icon.webp`, `logo-mark.webp`) + food-ring dishes |
| `padosi-hero.mp4` | 2K 16:9 walkthrough video of this page (for the waitlist etc.) |
| `HANDOFF.md` | How to integrate this into the product codebase |

See **HANDOFF.md** for integration guidance.

Imagery is AI-generated and illustrative. Built for the Rethink buildathon (Padosi / C-8).
