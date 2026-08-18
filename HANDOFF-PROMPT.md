# leasync.ai homepage hero — handoff for Mason

Prepared 18 Aug 2026 by Aaron (with Claude). Scope: **the hero section only** — nothing below the fold changes.

## 1. Look at it first

- **Live spec page (share this):** https://claude.ai/code/artifact/f7689d65-2d05-4472-91d1-292cea50a447
  Shows the finished hero running, then explains the copy, the 13 previews, and the build notes.
- **`hero-section.html`** — the drop-in component on its own. Open it in a browser; it is exactly what goes on the site (HTML + CSS + one script, no images, no video, no libraries, no dependencies).
- **`hero-spec-page.html`** — the same thing as the artifact link, offline.

## 2. What changes on the homepage

| | Today | New |
|---|---|---|
| Headline | Do you want to grow your CRE business? | **Want to grow your business as a** / **commercial real estate broker?** (second phrase on its own line, in mint) |
| Pill above headline | — | THE SOFTWARE BUILT FOR CRE BROKERS |
| Sub-line | Run your deals on the platform trusted with over $60B in lease volume. | The software built for you. Your deals, exclusives, co-broke splits and what you're owed — all in one system. |
| Buttons | one | **one** — "I'm ready to grow" (unchanged), same destination as today |
| Trust line | — | Free plan, no card. Paid plans from $59 · 30-day money back. |
| Under the button | static pipeline image | **"Preview our features below"** label + **13 clickable feature previews** that auto-play in order and loop |

**Drop the "$60B" line.** It is hardcoded text, not a measured number, and it is the easiest thing for a broker to challenge on a call. Every number in the new hero is either a demo figure inside the product mock-up or a published fact (prices, guarantee).

## 3. The 13 previews, in order

1. AI Agenda · 2. Email · 3. Ask Lisa · 4. Contacts · 5. Tenants · 6. Tour books · 7. Smart Match (Spaces) · 8. Drag in LOI (Deals) · 9. Deal board (Deals) · 10. Commissions · 11. Settings · 12. Marketplace · 13. Dashboard

Each is a short scripted interaction (7–11 s), ~2½ minutes end to end, then it loops. Clicking a label pins that feature and stops auto-advance. Scrolling away pauses; scrolling back resumes the same feature.

## 4. How it is built (so it ports cleanly)

- **Stacking:** all 13 `.act` panels share one grid cell (`grid-area: 1/1`), so the stage is always as tall as the tallest act and the page never jumps. Hidden acts are `opacity:0; visibility:hidden` — visibility matters, it stops them catching clicks/screen readers.
- **Sequencer:** every act is an array of `[delayMs, fn]` pairs run through one `run(steps)` that returns the act's total length; that length drives the tab progress bar and the hand-off. Retime a beat and nothing else needs touching.
- **Deal board card:** the travelling card is one absolutely-positioned element animated with a single `translate3d()`. Its travel step is **measured in JS** (`--step`) on play and on resize — a `%` inside `transform` refers to the element's own box, so it can't be derived in CSS.
- **Count-ups** use `requestAnimationFrame` plus a `setTimeout` that writes the final value — rAF pauses in background tabs.
- **Numbers can't disagree:** board counts/totals derive from base values + the moving deal; the $48,200 commission, the two $24,100 halves, the installments, the received meter and the agenda item all read from one constant.
- **Accessibility:** stage is `aria-hidden` (the caption under it says the same thing in text); tab labels are real `<button role="tab" aria-selected>`; `prefers-reduced-motion` renders a finished frame with no timers.
- **Mobile:** two-column acts collapse below 860 px; the board scrolls sideways inside its own container; grid items have `min-width:0` so nothing forces the page wider than the phone. Verified at 375 px.
- **Perf:** only `transform`, `opacity`, `width` and SVG `stroke-dashoffset` animate; an `IntersectionObserver` stops all timers when the hero is off-screen.
- **Fonts/colours:** Inter + Inter Tight (already on the site — delete the two `@font-face` data-URI rules from the file). Palette is `brand/tokens.css` values only. No monospace anywhere, no red/orange anywhere; status pills are mint (positive) / sage (pending).

## 5. Ready-to-paste prompt for a Claude Code session in the leasync.ai repo

```
Replace the homepage hero section (the block with the H1, sub-line, CTA button and the static pipeline image) with the component in marketing/site-hero/hero-section.html. Scope is the hero only — do not touch anything below it.

Rules:
- Copy must match the file exactly (pill "THE SOFTWARE BUILT FOR CRE BROKERS", headline with the mint second line on its own line, sub-line, one button "I'm ready to grow" pointing at the same route the current button uses, trust line). Do NOT keep the "$60B" sentence anywhere.
- Port it into our stack (React/TSX): one component, CSS-modules or styled equivalent of the file's CSS. Keep the class/structure so the CSS carries over. Keep the sequencer pattern: each act = array of [delayMs, fn]; a single scheduler returns the act's duration; that duration drives the tab progress bar and auto-advance. useReducer for {act, pinned} + per-act state; clearTimeout everything in the effect cleanup.
- Keep these behaviours exactly: 13 tabs in a 7-column grid on ≥700px (always two rows, 7 + 6); click pins an act; IntersectionObserver pauses off-screen and RESUMES THE SAME ACT on return; prefers-reduced-motion renders the finished frame with no timers; measure the deal-board travel step in px on play and on resize (do not derive it in CSS); count-ups have a setTimeout fallback that writes the final value.
- Use the site's existing Inter / Inter Tight — drop the @font-face data-URI rules. No monospace, no red or orange, anywhere.
- The hero headline is the page's only <h1>. The stage is aria-hidden; the caption under it is real text.
- Verify: no horizontal page scroll at 375px; all 13 acts reach their end state with no console errors; the deal card lands exactly on each column at 900px and 1200px wide.
- Do not install any dependency for this. It is plain CSS + one small script.
```

## 6. Acceptance checklist

- [ ] Headline reads on two lines on desktop, second line mint; wraps naturally on phones
- [ ] One button only; goes where today's button goes
- [ ] No "$60B" text anywhere on the page
- [ ] Pill reads THE SOFTWARE BUILT FOR CRE BROKERS
- [ ] "Preview our features below" label + 13 tabs, two rows on desktop
- [ ] Auto-plays 1→13 and loops; clicking a tab pins it
- [ ] Deal card lands on each column exactly (no drift) at any width
- [ ] Nothing red/orange, no monospace, fonts are Inter / Inter Tight
- [ ] 375 px: no sideways page scroll; board scrolls inside its own box
- [ ] Reduced-motion: finished frame, no motion
- [ ] Zero console errors
