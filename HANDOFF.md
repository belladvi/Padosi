# Padosi Hero Page — Integration Handoff

For the engineer integrating this scroll hero into the Padosi product. It's a
**self-contained static page** (vanilla HTML/CSS/JS) — you can ship it as-is, embed
it, or port its sections into the app. Everything below is what you need to do any of
those.

---

## 1. Fastest path — ship as static
Copy the whole folder into your host's static root (or a subpath) and serve it. It
needs no build and no server code — just static file serving over HTTP.
- Next.js: drop the files under `public/hero/` → served at `/hero/`.
- Or deploy this folder directly to Vercel/Netlify/GitHub Pages.
- Link or route your "hero"/landing entry to `index.html`.

The two CTAs ("Order home food" / "I'm a home cook") already link to
`https://padosi-self.vercel.app` — change those `href`s to your routes.

## 2. Embed inside an existing page
Host it as above and embed:
```html
<iframe src="/hero/index.html" style="width:100%;height:100vh;border:0"></iframe>
```
(Simple, isolated, but heavier than a native port.)

## 3. Port into React / Next.js (recommended for the real app)
The page has two independent parts:

### a) The scroll film (canvas)
- `main.js` is the engine: it fetches `frames/frames.json`, preloads the WebP frame
  blobs, and draws the frame matching scroll position onto `<canvas id="film">`.
- Wrap it in a **client component** (`"use client"`). Keep `#track` (the tall spacer),
  the sticky `#stage`, `<canvas id="film">`, `#vignette`, and the `.caption` divs.
  Port `main.js` as a module and run it in a `useEffect` on mount.
- The captions are plain divs with `data-in / data-hold / data-out` scroll fractions;
  the engine computes their opacity per frame. Chapter fractions are baked to the
  current film: title 0.00 · cook 0.16 · kadhai 0.33 · seal 0.49 · door 0.66 · close 0.82.

### b) Everything below the film
Standard sections — port each to a component: `LiveMenu`, `Manifesto`, `FoodRing`,
`HowItWorks`, `ForCooks`, `ForResidents`, `Trust`, `Stats`, `Testimonials`, `Faq`,
`CtaBand`, `Footer`. The reveal/parallax/counter/tilt/food-ring logic is in the
inline `<script>` at the bottom of `index.html` — move it into a `useEffect`.

### Dependencies (currently CDN — swap to npm for the app)
- `gsap` + `gsap/ScrollTrigger` (reveals, parallax, counters)
- `lenis` (smooth scroll) — `npm i lenis gsap`
- Fonts: **Fraunces** (display) + **Inter** (body). Use `next/font/google` instead of
  the `<link>`.
- Everything degrades gracefully if GSAP/Lenis are absent (there's an
  IntersectionObserver fallback and a `prefers-reduced-motion` guard).

### The `?record=1` guard
`index.html` disables Lenis when the URL contains `record` (used to capture the demo
video with clean programmatic scrolling). Harmless; keep or remove.

---

## 4. The live-menu phone — keep it deterministic
The phone mockup in the "One live batch, one link" section is **real HTML/CSS**, not an
image or AI render. This is intentional and a standing project rule: never let
generated imagery render product UI/text (it distorts). If you wire it to real data,
replace the hard-coded dish rows (`.item`) and the countdown (`#countdown`) with your
API — the markup/classes are ready for it.

## 5. Brand tokens
Defined as CSS variables at the top of `index.html` (`:root`):
- Background `#0e0906` / `#17100a`
- Accent (terracotta) `#c0603e`, highlight `#ec9a6b`, gold `#e8b981`
- Text `#f7f0e8`, dim `rgba(247,240,232,.6)`
- Fonts: `--serif: Fraunces`, `--sans: Inter`
- Logo: `img/logo-icon.webp` (transparent bowl mark), `img/logo-mark.webp` (full
  bowl + steam-roof render for large/hero use). Favicon uses `logo-icon.webp`.

## 6. The walkthrough video
`padosi-hero-4k.mp4` — 4K 16:9, ~65s, a screen-recording of this page ending on the
live app, with an original (copyright-free) soundtrack. For the waitlist "A peek at the
app" section, replace the static phone render with:
```html
<video src="padosi-hero-4k.mp4" autoplay muted loop playsinline preload="metadata"
       style="width:100%;border-radius:20px"></video>
```
(Autoplay must be muted; add a mute/unmute control if you want the music audible.)

## 7. How the assets were made (for regeneration)
- Film stills + clips: AI-generated (Higgsfield — GPT-Image for stills, Kling v3.0
  image-to-video for the 5 clips), assembled + sliced to WebP frames with ffmpeg.
- Food-ring dishes + section imagery: AI-generated stills → WebP.
- The film clips are **~720p at source** (Kling standard tier). If you need the cook
  footage crisper for very large screens, the clips can be regenerated at a higher
  tier and the frames re-sliced (see project notes in the ProductBrain vault).

## 8. Known limitations
- CDN dependencies (GSAP, Lenis, Google Fonts) — vendor them for offline/enterprise.
- The film is ~720p source; page UI/text is fully sharp.
- `frames/` is ~20 MB (339 files); it loads progressively with a gated loader.

## Quick checklist to go live in the app
1. Serve the folder statically (or port sections per §3).
2. Point the CTA `href`s at real routes.
3. Swap CDN libs → npm; fonts → `next/font`.
4. Wire the live-menu phone to real batch data (optional).
5. Keep the phone UI as HTML/CSS — never an image.
