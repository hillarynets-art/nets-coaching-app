---
name: apple-design
description: Use when building or restyling UI that should look and feel like a native Apple app (iOS, iPadOS, macOS) or when the user says "Apple style", "iOS look", "Human Interface Guidelines", "HIG", "SF Pro", "like the Settings app", "native feel", or asks why a web UI feels un-Apple. Covers web/React/HTML implementations, not SwiftUI.
---

# Apple Design (HIG for web UIs)

## Overview

Apple's Human Interface Guidelines (HIG) reduce to three rules: **clarity** (text legible at every size, icons precise, no ornament), **deference** (chrome stays out of the content's way, translucency and blur hint at what lies beneath), and **depth** (layers, realistic motion and shadows convey hierarchy). A UI reads as "Apple" when it uses the system type scale, semantic colors, one tint, generous margins, 44pt targets and spring motion. It reads as "not Apple" the moment it uses tracked uppercase micro-labels, emoji as icons, hard 1px borders everywhere, or a second accent color.

Exact values (type scale, colors, sizes, radii, motion) are in `hig-reference.md`. Read it before writing styles; do not guess numbers.

## When to Use

- Request mentions Apple, iOS, HIG, SF Pro, SF Symbols, "native", "like the Health/Fitness/Settings app"
- App is a mobile-first PWA or single-page web app that will run inside Safari / Add to Home Screen
- A design critique says the UI feels "web-y", "Android", "Bootstrap", "dashboard-y"
- User asks for dark mode, tab bars, sheets, inset grouped lists, large titles, blurred bars

**Do not use** for marketing sites, dashboards meant for desktop mouse use, or when the brand already has a strong non-Apple system (Material, Fluent). Do not use to copy Apple's logos, product names or SF Symbols into shipping software without a license check.

## Core Pattern

```jsx
/* Before: web-y */
<div style={{fontFamily:"Space Grotesk",fontSize:10,letterSpacing:1,
  textTransform:"uppercase",color:"#71717A",border:"1px solid #2A2A2C",
  borderRadius:12,padding:12}}>Personal Bests</div>

/* After: Apple */
<div style={{fontFamily:SYSTEM_FONT,fontSize:13,fontWeight:400,
  color:"var(--label-secondary)",padding:"0 16px 6px"}}>Personal Bests</div>
<div style={{background:"var(--bg-secondary-grouped)",borderRadius:10,
  margin:"0 16px"}}>…rows separated by inset hairlines…</div>
```

The section header becomes a 13pt secondary label in sentence case; the card loses its border and becomes an inset-grouped surface with a 10pt radius; separators are 0.5px hairlines inset to the text edge, not full-width borders.

## Quick Reference

| Concern | Rule |
|---|---|
| Font | `-apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", system-ui, sans-serif`; antialiased |
| Body size | 17px / 22px line. Never below 11px. Inputs 16px+ (stops Safari zoom) |
| Headings | Large Title 34/700, Title 1 28/700, Title 2 22/700, Title 3 20/600, Headline 17/600 |
| Secondary text | Same size, `label-secondary` (60% alpha), not smaller + lighter |
| Tracking | `letter-spacing: normal`. `-apple-system` applies SF's optical tracking itself; never add tracking by hand |
| Case | Sentence case everywhere, including inset-list section headers (uppercase headers are pre-iOS 15). No uppercase tracking except tiny badges |
| Tint | One accent used for interactive elements only. Status uses system red/green/orange |
| Margins | 16px edge gutter (20 on wide), 8px grid, 44×44 min touch target |
| Radius | Chips 999, buttons 12 to 14, cards and inset lists 10 to 16, sheets 10 top, app icons/screens 38+ |
| Nesting radius | inner = outer − padding (concentric), never same radius inside |
| Surfaces | Dark: #000 / #1C1C1E / #2C2C2E elevation steps, no borders. Light: #F2F2F7 page, #FFF cards |
| Separators | 0.5px, `rgba(60,60,67,.29)` light / `rgba(84,84,88,.6)` dark, inset to text |
| Bars | Nav 44px (+52 large title), tab bar 49px + `env(safe-area-inset-bottom)`, translucent |
| Material | `backdrop-filter: blur(20px) saturate(180%)` + 70–80% alpha background, `-webkit-` prefix |
| Tab bar | 2 to 5 tabs (6+ means consolidate; iOS would show "More"), icon 25px above 10px label, tinted when selected, gray otherwise |
| Buttons | Filled: tint bg, white 17/600 text, 50px tall. Tinted: tint at 15% bg. Plain: tint text |
| Press state | `transform: scale(.97); opacity: .8` on `:active`, 100ms |
| Motion | 200–350ms, `cubic-bezier(.32,.72,0,1)` for sheets, `cubic-bezier(.25,.1,.25,1)` default; honor `prefers-reduced-motion` |
| Sheets | Slide up, 10px top radius, 36×5 grabber, dimmed backdrop 40% |
| Viewport | `viewport-fit=cover`, `100dvh`, `overscroll-behavior: none`, `-webkit-tap-highlight-color: transparent` |
| Icons | SF Symbols style: 1.5–2px stroke line icons on a 24 grid. Never emoji |
| Copy | Verbs on buttons ("Save", "Log Workout"), never "OK"/"Yes". Title Case for bar titles only |

## Implementation

1. Define tokens once (CSS custom properties or a JS object) using the semantic names in `hig-reference.md`: `label`, `label-secondary`, `bg`, `bg-secondary`, `bg-grouped`, `fill`, `separator`, `tint`. Map both light and dark; switch on `prefers-color-scheme` or a user toggle.
2. Replace display fonts with the system stack. Keep a brand display font only for the app name or a hero number, never for body or labels.
3. Rebuild the type scale from the table. Delete every 9px and 10px size; the smallest UI text is Caption 2 at 11px and it is rare.
4. Remove card borders in dark mode; use elevation steps. Keep hairline separators between rows.
5. Give bars translucency and safe-area padding. Make the tab bar icons real icons (inline SVG), labels 10px, selected tint.
6. Add press states, spring-ish sheet transitions and reduced-motion fallbacks.
7. Check: 4.5:1 contrast on all text, 44px targets, nothing truncates at 120% text size.

## In this repo (nets-coaching-app)

`index.html` styles everything inline from `CDark`/`CLight` (line ~174). Map them rather than adding a third palette:

| Existing token | Apple role | Dark | Light |
|---|---|---|---|
| `bg` | bg-grouped | #000000 | #F2F2F7 |
| `surface` | bar material base | #1C1C1E @ 82% | #F9F9F9 @ 82% |
| `card` | bg-grouped-secondary | #1C1C1E | #FFFFFF |
| `muted` | fill-tertiary | rgba(118,118,128,.24) | rgba(118,118,128,.12) |
| `mutedFg` | label-secondary | rgba(235,235,245,.6) | rgba(60,60,67,.6) |
| `border` | separator (rows only, not cards) | rgba(84,84,88,.6) | rgba(60,60,67,.29) |
| `accent` | tint | #DFE104 with `accentFg` #000 | #6E6E00 as text, #DFE104 as surface with #000 text |

Space Grotesk may stay for the splash wordmark only. The six-tab `NAV` must become five. `Btn` drops `letterSpacing`/`textTransform` and becomes 50px tall, 17/600, 14px radius.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Tracked uppercase 10px section labels | 13px sentence-case secondary label, or 22px Title 2 for grouped sections |
| Emoji in tab bar / buttons | Inline SVG line icons, SF Symbols style |
| Two accents (brand lime + blue links) | One tint. Brand color may stay as tint if it passes contrast on both schemes |
| 1px borders on every card in dark mode | Elevation by lighter surface; borders only for inputs |
| Hover styles with no `:active` state | Press state first; hover is a bonus on pointer devices |
| `height:100vh` + fixed bottom bar | `100dvh`, bottom bar padded by `env(safe-area-inset-bottom)` |
| Blur without a fallback | Solid 94% alpha background under `@supports not (backdrop-filter)` and `prefers-reduced-transparency` |
| Centered modal cards for everything | Sheets from the bottom for tasks, centered alerts only for destructive confirms |
| Bouncy 500ms+ animations | 250ms ease-out; springs only for sheets and drag |
| "OK / Cancel" alerts | Verb that names the action; destructive action in red on the right |
| Light text on yellow accent | Dark text (#000) on yellow/lime; white only on blue, red, green, purple |

## Red Flags

If any of these appear in the diff, stop and re-check the reference: `letterSpacing:1` on body text, `fontSize:9`, `fontSize:10`, `textTransform:"uppercase"` on a section title, an emoji string inside a `<button>`, `border:1px solid` on a dark card, `100vh` on the root, a second accent hex.
