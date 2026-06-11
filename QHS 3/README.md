# Quicknet Home Security (Q.H.S) LLC — Cinematic 3D Scroll Website

A single-file, cinematic, scroll-scrubbed marketing site for **Quicknet Home Security LLC**, owned and operated by Mark & Juliet Harvey. The hero is a 6-second 4K-style camera-mount video that scrubs in sync with the scroll wheel; the rest of the page is a four-section narrative built with GSAP ScrollTrigger, IntersectionObserver, and a custom gold cursor.

---

## Project Files

| File | Purpose |
|------|---------|
| `quicknet_3d.html` | The entire website — HTML, CSS, and JavaScript inline. |
| `camera.mp4` | 6-second hero background video (security camera flying down and mounting onto a house). |
| `README.md` | This document. |

> Both `camera.mp4` and `quicknet_3d.html` must live in the **same folder** — the video is referenced as a relative path: `<source src="camera.mp4">`.

---

## How to Run

Because the page loads a local `.mp4`, opening the HTML directly with `file://` works on most desktop browsers, but Safari and some mobile browsers will refuse to play the video without an HTTP server. For best results:

```bash
cd "/Users/kingdavid/Desktop/QHS 3"
python3 -m http.server 8080
# then open http://localhost:8080/quicknet_3d.html
```

No build step, no npm, no bundler — it's one HTML file.

---

## Color System

| Token | Hex | Used For |
|-------|-----|----------|
| `--dark` (void) | `#0D1F1A` | Body, nav scrolled state, contact section |
| `--green` (forest) | `#1B4D3E` | Services section background, why-us accents |
| `--green2` | `#4A6741` | Secondary green tints |
| `--cream` | `#F5F0E8` | Why-us section background, primary text on dark |
| `--cream2` | `#EDE8DF` | Why-us card backgrounds |
| `--red` | `#8B1A1A` | CTAs, no-contract strip, card accent lines |
| `--gold` | `#C8A96E` | Cursor, badges, eyebrows, italic display text |
| `--muted` | `#5F5E5A` | Body copy on cream backgrounds |

---

## Typography

Loaded from Google Fonts:

- **Cormorant Garamond** (300 / 500 / 600 + italic) — display headlines, numerals, italics
- **Outfit** (300 / 400 / 500 / 600) — UI, body copy, labels

---

## Section-by-Section

### Section 1 — Hero (video scroll-scrub)

- `camera.mp4` plays as a full-viewport background (`object-fit: cover`) inside a `position: sticky` 100vh container.
- The hero section itself is **220vh tall** — that extra 120vh of scroll distance is what gets mapped to the video timeline.
- **GSAP ScrollTrigger** tweens `video.currentTime` from `0` → `video.duration` with `scrub: 0.5` as the user scrolls through the hero.
- Falls back to a vanilla `scroll` event listener if GSAP fails to load from CDN.
- On screens ≤768px the hero collapses to 100vh and the video plays normally (`autoplay muted loop playsinline`) — **no scrubbing on mobile**, per spec.
- A semi-transparent dark overlay sits on top so the headline stays legible regardless of which frame is showing.
- Above the video: Q.H.S badge → Cormorant headline → single red **"Get a Free Quote"** CTA wired to `tel:13475927733`.

### Section 2 — Services (3D tilt cards)

- Background: `#1B4D3E` (forest green).
- Six service cards in a 3-column grid:
  1. Smart Camera Installation
  2. POE & BNC Systems
  3. Structural Cabling
  4. Internet Access Points
  5. DVR / NVR Setup
  6. System Maintenance
- Each card responds to `mousemove` with a `rotateX` / `rotateY` tilt toward the cursor via CSS `perspective(1200px)` + `transform-style: preserve-3d`.
- Cards fade in on first viewport entry via `IntersectionObserver`, with staggered `transition-delay` per card.

### Section 3 — Why Us (cream)

- Background: `#F5F0E8` (cream).
- Two-column layout:
  - **Left**: 4 numbered "why" items, each with a 3px green left border (turns red on hover).
  - **Right**: 4 stat tiles — `100%` owned, `0` contracts, `Same` day response, `Real` people answer.
- Everything translateY-fade-ups on scroll via the shared `.reveal` IntersectionObserver hook.

### Section 4 — Contact (void dark)

- Background: `#0D1F1A`.
- Three tap-to-call / email cards:
  - `tel:13475927733` (Call or text)
  - `tel:13477054431` (Second line)
  - `mailto:quicknethome.s@gmail.com`
- Manager cards for **Mark Harvey** and **Juliet Harvey**.
- A dark-red "No contracts. No commitments." strip with a red left border anchors the section.

---

## Global UX Details

- **Sticky nav** — frosts to a translucent dark-green glass (`backdrop-filter: blur(16px) saturate(140%)`) once `scrollY > 60px`.
- **Custom cursor** — a 10px gold dot with a 36px ring that lags behind via `requestAnimationFrame` interpolation. Scales up on hover over any anchor, button, card, or tile. Hidden on touch devices.
- **Smooth scroll** — `html { scroll-behavior: smooth }` so the nav's `#services`, `#why`, `#contact` anchor links glide.
- **Tap-to-call everywhere** — every phone number on the page is wrapped in a `tel:` anchor; mobile devices invoke the dialer with one tap.
- **Mobile responsive** — single-column layouts, hidden secondary nav links, larger touch targets, and the video drops scroll-scrubbing in favor of normal autoplay/loop.

---

## Dependencies

All loaded from CDNs (no local install required):

- [Google Fonts](https://fonts.google.com) — Cormorant Garamond + Outfit
- [GSAP 3.12.5](https://greensock.com/gsap/) — core engine
- [GSAP ScrollTrigger 3.12.5](https://greensock.com/scrolltrigger/) — drives the video scrub

No frameworks, no bundlers, no build pipeline.

---

## Browser Notes

- **Desktop Chrome / Edge / Firefox** — full experience, including scroll scrub and custom cursor.
- **Desktop Safari** — works, but `backdrop-filter` already requires the `-webkit-` prefix (already included). If the page is opened via `file://`, Safari will refuse to autoplay the video — serve it over HTTP instead.
- **iOS Safari / Android Chrome** — scroll scrubbing is disabled below 768px. The video plays normally with `muted playsinline autoplay loop`, which iOS requires for inline autoplay.

---

## Company Information

- **Name:** Quicknet Home Security (Q.H.S) LLC
- **Tagline:** Security you can trust. People you can call.
- **Owners / Managers:** Mark Harvey, Juliet Harvey
- **Phone (primary):** 1-347-592-7733
- **Phone (secondary):** 1-347-705-4431
- **Email:** quicknethome.s@gmail.com
- **Promise:** No contracts. No commitments. Same-day response. Real humans answer.

---

## Editing Cheatsheet

| Want to change… | Look for… |
|-----------------|-----------|
| Hero headline | `<h1>` inside `.hero-content` |
| Hero CTA destination | `<a href="tel:13475927733" class="btn-main">` |
| Services list | `.cards-grid` (six `.card-3d` blocks) |
| Why-us bullets | `.why-items` (four `.why-item` blocks) |
| Stats | `.stat-block` (four `.stat` blocks) |
| Contact details | `.contact-cards` |
| Section colors | `:root` variables at the top of the `<style>` block |
| Scroll scrub length | `.hero { height: 220vh }` — increase for slower scrub |
| Mobile breakpoint | `@media (max-width: 768px)` blocks |
