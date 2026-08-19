# Interactive feature section for /about

One self-contained section to add to **leasync.ai/about**. Nothing else on the page changes.

## See it

**Live preview:** https://aaraziz.github.io/leasync-hero-preview/leasync-features-preview.html

**In context** (a static copy of the real about page, opens at the new section):
https://aaraziz.github.io/leasync-hero-preview/about-integrated.html

## The code

**One file, paste-ready:**
https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/leasync-features-section.html

```bash
curl -O https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/leasync-features-section.html
```

It contains three blocks, in order:

1. `<style>` — all CSS
2. `<div id="leasync-features">…</div>` — the markup
3. `<script>` — ~250 lines, no dependencies

Paste them in that order. That's the whole install.

## Where it goes

After **Chapter 06 · Lisa AI**, before **"Want to see what's next?"**.

It brings its own chapter label (`CHAPTER 07`), heading and lede, so it doesn't need a wrapper — drop it straight into `<main>` alongside the other `<section>`s.

## What it does

Thirteen feature previews — AI Agenda, Email, Ask Lisa, Contacts, Tenants, Tour books, Smart Match, Drag in LOI, Deal board, Commissions, Settings, Marketplace, Dashboard.

- Auto-plays through all thirteen and loops.
- Clicking a tab pins that feature and stops the auto-advance.
- The card heading and its one-line description follow the active feature, so the section still reads correctly when paused.
- `IntersectionObserver` stops every timer when the section scrolls out of view and resumes the same feature on return.
- `prefers-reduced-motion` is honoured: no timers start, it renders a finished frame.

## Two things to change when you port it

1. **Delete the two `@font-face` rules** at the top of the `<style>` block. They embed Inter as base64 only so the file runs standalone — the site already ships Inter.
2. Nothing else is required.

## Why it's safe to drop in

Every CSS rule is scoped under `#leasync-features`. Nothing in this section can affect the rest of the about page, and Tailwind can't reach into it. I did this because the section uses semantic class names (`.card`, `.panel`, `.board`) that would otherwise be a collision risk.

`#leasync-features` is capped at `max-width: 1201px` with auto margins so it lines up with the other chapters. That's harmless inside your `max-w-7xl` container.

## Design tokens

All measured off the live /about page rather than eyeballed:

| | |
|---|---|
| Container | 1201px |
| Chapter label | 11px, `0.28em`, `#7FC4A9`, 2px `#3AA57A` left rule |
| H2 | 48/48, 700, `-0.025em`, `#F6F1E6` |
| Lede | 18/28, `#A9BCB3`, max 576px |
| Feature card | `#F7F4EC`, 2px `#D3D8C4`, radius 24px, shadow `0 30px 60px -30px rgba(0,0,0,.5)` |
| Card heading | `#0E2A23` |
| Font | Inter only — no second family |

The 13 panels are themed entirely through `--d-*` custom properties defined on `.lightstage`, so if the card colour ever changes, that one block is the only thing to touch.

## Responsive

| Width | Tabs |
|---|---|
| ≥1100px | 2 rows (7 + 6) |
| 820–1100px | 3 rows |
| 560–820px | 4 rows |
| <560px | 2 columns |

Panels stack on narrow screens; the deal board scrolls sideways inside its own container. The page never scrolls sideways.

## Checklist

- [ ] Section sits between Lisa AI and the roadmap block
- [ ] The two `@font-face` rules removed; text renders in the site's Inter
- [ ] Tabs on two rows at desktop, nothing clipped
- [ ] Clicking a tab pins it; heading and description update
- [ ] Scrolling away and back resumes the same feature
- [ ] No horizontal page scroll at 375px
- [ ] `prefers-reduced-motion` renders a static finished frame
- [ ] Zero console errors

Questions → Aaron.
