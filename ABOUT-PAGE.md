# leasync.ai/about — rebuild

Replaces everything below the hero. The hero, the roadmap block and the footer stay.

## See it

| | |
|---|---|
| **Full page** | https://aaraziz.github.io/leasync-hero-preview/about-v2.html |
| **Just the new sections** | https://aaraziz.github.io/leasync-hero-preview/leasync-about-preview.html |

## The code

**One paste-ready file:**
https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/leasync-about-sections.html

```bash
curl -O https://raw.githubusercontent.com/aaraziz/leasync-hero-preview/main/leasync-about-sections.html
```

Three blocks, in this order:

1. `<style>` — all CSS, scoped under `#leasync-about`
2. `<div id="leasync-about"> … </div>` — the markup
3. `<script>` — one script, no dependencies

Put the div where **Chapter 01** starts today. Delete Chapters 01–06 and all 14 old feature cards.

## One change above it

Hero `<h1>`:

```
- Built Exclusively For Commercial Leasing.
+ Built Exclusively For Commercial Real Estate Brokers.
```

Recommended while you're in there: the hero's second sentence still says *"designed for retail and restaurant brokerages."* That contradicts the CRE-wide positioning — suggest *"commercial real estate brokerages."* Aaron's call.

## What the new page is

| Section | What it does |
|---|---|
| **See It For Yourself** | 14 clickable feature previews. Auto-plays and loops; clicking a tab pins it. Each one shows six notes beside a live panel. This is where the old chapters went — their bullet lists are folded into the feature they belong to. |
| **Everything else** | Map view, Calendar, Documents, Notifications, Permissions, Security & import. Six short cards instead of six full chapters. |
| **Built For Everyone On The Deal** | Leasing brokers · Investment sales brokers · Landlords & owners · Management & principals. |
| **What Makes It Different** | Built by a working broker · It's your book not a landlord's · Co-broke splits and payouts tracked · Published pricing. |
| **A Broker And An Engineer** | Aaron and Mason, matching the main page's founders section. |
| **Closing CTA** | Book a demo · Get Started · See the roadmap. |

**The 14 features, in order:** AI Agenda · Email · Ask Lisa · Contacts · Tenants · **Web Extension** · Tour books · Smart Match · Drag in LOI · Deal board · Commissions · Settings · Marketplace · Dashboard

## Why it changed

The current page is six chapters plus fourteen feature cards — about **11,200px of scrolling**, every feature the same size and weight, so nothing is prioritised. The main page does the opposite: six short sections, 600–750px each.

The new page is **~4,900px**, roughly a third the length, and follows the main page's rhythm: centred uppercase 800-weight headings, 18/28 ledes capped at 672px, small mint eyebrows, 16px-radius cards.

## Behaviour to preserve if you rewrite it in React

- Auto-plays through all 14 and loops; a click pins that feature and stops auto-advance.
- The card heading, description and bullet list all follow the active tab.
- `IntersectionObserver` stops every timer when the section leaves the viewport and resumes the same feature on return.
- `prefers-reduced-motion`: no timers start, it renders a finished frame.
- Count-ups use `requestAnimationFrame` **plus** a `setTimeout` that writes the final value — rAF is paused in background tabs.
- The deal board's travel step is measured in px on play and on resize. A `%` inside `transform` refers to the element's own box, so it can't be derived in CSS.

## Design tokens

Read off the live site rather than eyeballed.

| | |
|---|---|
| Page | `#0B1C18` |
| Section heading | 36px / 800 / uppercase / `-0.025em` / `#F6F1E6` / centred |
| Lede | 18/28 `#A9BCB3`, centred, max 672px |
| Eyebrow | 11px, `0.28em`, `#7FC4A9` |
| Card | radius 16px, border `rgba(255,255,255,.10)`, bg `rgba(255,255,255,.024)` |
| Highlighted card | border `#7FC4A9`/50 on `#7FC4A9`/8% |
| Feature card | `#F7F4EC`, 2px `#D3D8C4`, radius 24px, shadow `0 30px 60px -30px rgba(0,0,0,.5)` |
| Font | Inter only |

The 14 panels are themed through `--d-*` custom properties on `.lightstage` — if the card colour ever changes, that one block is the only thing to touch.

## Responsive

| Width | Tabs | Layout |
|---|---|---|
| ≥1100px | 2 rows (7 + 7) | notes beside panel |
| 820–1100px | 3 rows | notes beside panel |
| 560–820px | 4 rows | notes above panel |
| <560px | 2 columns | stacked |

No horizontal page scroll at any width. The deal board scrolls sideways inside its own container.

## Checklist

- [ ] Hero H1 says "Commercial Real Estate Brokers"
- [ ] Chapters 01–06 and the 14 old feature cards removed
- [ ] The two `@font-face` rules deleted; text renders in the site's Inter
- [ ] 14 tabs, two rows at desktop, nothing clipped
- [ ] Clicking a tab pins it; heading, description and six notes all update
- [ ] Scrolling away and back resumes the same feature
- [ ] No "NYC" or "New York" anywhere on the page
- [ ] No horizontal scroll at 375px
- [ ] `prefers-reduced-motion` renders a static finished frame
- [ ] Zero console errors

Questions → Aaron.
