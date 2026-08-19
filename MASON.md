# leasync.ai — new homepage hero

Everything you need is in this repo. Scope: **the hero section only** (the block with the H1, sub-line, CTA and the pipeline image). Nothing below the fold changes.

## 1. See it running

| | Link |
|---|---|
| **Dashboard version** (recommended) | https://aaraziz.github.io/leasync-hero-preview/dashboard.html |
| Simple version (alternative) | https://aaraziz.github.io/leasync-hero-preview/simple.html |

Add `?v=2` to the end if a page looks stale — GitHub Pages caches for 10 minutes.

## 2. The code

Self-contained drop-in components — HTML + CSS + one script. No images, no video, no libraries, no dependencies.

| | View | Raw (download / fetch) |
|---|---|---|
| Dashboard | [component-dashboard.html](https://aaraziz.github.io/leasync-hero-preview/component-dashboard.html) | https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/component-dashboard.html |
| Simple | [component-simple.html](https://aaraziz.github.io/leasync-hero-preview/component-simple.html) | https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/component-simple.html |

```bash
git clone https://github.com/aaraziz/leasync-hero-preview.git
# or just:
curl -O https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/component-dashboard.html
```

Open it in a browser and it runs exactly as it will on the site.

## 3. The copy — must match exactly

| | |
|---|---|
| Pill | THE SOFTWARE BUILT FOR CRE BROKERS |
| Headline | **Want to grow your business as a** / **commercial real estate broker?** — second phrase on its own line, in mint |
| Sub-line | Every deal on one board, every commission tracked to the dollar, every tenant, broker, landlord and space connected — so you spend the day closing, not chasing. |
| Volume line | Run your deals on the platform trusted with over **$120B** in deal volume. |
| Button | **I'm ready to grow** — one button only, same destination as today's |

The old "$60B in lease volume" sentence is replaced by the $120B line above. Do not keep both.

## 4. What the animation does (dashboard version)

The real dashboard floats in a slight 3D tilt and assembles itself over ~5s: sidebar → six KPI tiles counting up → Agenda, Commission payments, Activity log → Deal pipeline cards dealing in → Commission trend drawing → all-time stat strip. The first agenda item ticks itself off.

Then a cursor clicks **Entire organization → Just me** and every number re-counts to the solo view, and the firm-wide line on the chart fades. Then it flips back and loops.

## 5. Build notes

- **Fits above the fold.** Hero is 736px at 1280×800, 719px at 1024×768. On the live site give it `min-height: calc(100vh - <nav height>)` so it centres on taller screens.
- **Fonts.** The file embeds Inter and Inter Tight as `@font-face` data URIs **only so it runs standalone**. leasync.ai already ships both — **delete those two rules** when porting.
- **Colours** are all existing `brand/tokens.css` values. No monospace anywhere. No red or orange — the dashboard's red overdue dots and the yellow/orange pipeline headers are deliberately replaced with the mint value ramp; status is mint (positive) or sage (pending).
- **Headings.** `<h2 class="h1">` in the file must be the page's only `<h1>` on the live site.
- **Accessibility.** The dashboard is `aria-hidden="true"` (decorative); the caption under it carries the same information as text. `prefers-reduced-motion` is honoured — no timers start, and it renders the finished frame.
- **Performance.** Only `transform`, `opacity`, `width` and SVG `stroke-dashoffset` animate. An `IntersectionObserver` stops every timer when the hero leaves the viewport.
- **Mobile.** Below 760px the sidebar hides, the panels stack, and the 3D tilt is removed. The page never scrolls sideways.
- **Numbers are demo figures** in the style of the real dashboard — not measured results.

## 6. Port it

Paste this into a Claude Code session opened in the leasync.ai repo:

```
Replace the homepage hero section (the block with the H1, sub-line, CTA button and the static pipeline image) with the component at
https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/component-dashboard.html
Fetch that file first and read it in full. Scope is the hero only — do not touch anything below it.

Rules:
- Copy must match the file exactly: the pill "THE SOFTWARE BUILT FOR CRE BROKERS", the headline with "commercial real estate broker?" on its own line in mint, the sub-line, the "$120B in deal volume" line, and ONE button "I'm ready to grow" pointing at the same route the current CTA uses. Remove the old "$60B in lease volume" sentence entirely.
- Port it into our stack as one React/TSX component with CSS modules (or our styled equivalent). Keep the class names and DOM structure so the CSS carries over unchanged.
- Keep the animation engine as-is in spirit: a single scheduler that takes [delayMs, fn] pairs and returns the sequence length; useReducer for {phase, scope}; clearTimeout everything in the effect cleanup.
- Keep these behaviours exactly: the build sequence and its order; the cursor click that flips "Entire organization" <-> "Just me" with every number re-counting; IntersectionObserver pauses all timers off-screen and resumes on return; prefers-reduced-motion renders the finished frame with no timers; count-ups have a setTimeout fallback that writes the final value (requestAnimationFrame is paused in background tabs).
- Delete the two @font-face data-URI rules and use the site's existing Inter / Inter Tight. No monospace, no red, no orange anywhere.
- The hero headline must be the page's only <h1>. The dashboard mock stays aria-hidden="true"; the caption under it is real text.
- Give the hero min-height: calc(100vh - <our nav height>) so it fills the first screen on tall displays.
- Verify before you finish: no horizontal page scroll at 375px; the whole hero visible above the fold at 1280x800; the org/me flip updates all six KPI tiles AND the all-time strip; zero console errors.
- Do not install any dependency for this. It is plain CSS plus one small script.
```

## 7. Acceptance checklist

- [ ] Pill, headline (mint second line), sub-line, $120B line, one button — all exact
- [ ] No "$60B" text anywhere on the page
- [ ] Dashboard assembles, then flips org ↔ just me with every number re-counting
- [ ] Whole hero visible above the fold at 1280×800
- [ ] Fonts are the site's Inter / Inter Tight; the data-URI `@font-face` rules are gone
- [ ] Nothing red or orange; no monospace
- [ ] 375px: no sideways scroll, panels stacked, tilt removed
- [ ] `prefers-reduced-motion`: finished frame, no motion
- [ ] Zero console errors

Questions → Aaron.
