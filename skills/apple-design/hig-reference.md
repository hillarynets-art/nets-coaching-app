# HIG values reference (web implementation)

All values are CSS pixels at 1x, which map 1:1 to iOS points. Sources: Apple Human Interface Guidelines (Typography, Color, Layout, Materials, Motion), UIKit default metrics. Where Apple exposes no public number, the value is the measured default and is marked `≈`.

## 1. Type scale (SF Pro, Dynamic Type "Large" default)

| Style | Size | Line | Weight | Use |
|---|---|---|---|---|
| Large Title | 34 | 41 | 700 | Screen title at top of scroll, collapses to Title in bar |
| Title 1 | 28 | 34 | 700 | Page headline inside content |
| Title 2 | 22 | 28 | 700 | Grouped section titles, card titles |
| Title 3 | 20 | 25 | 600 | Sub-sections |
| Headline | 17 | 22 | 600 | List row title, button label |
| Body | 17 | 22 | 400 | Default text, inputs |
| Callout | 16 | 21 | 400 | Secondary body |
| Subheadline | 15 | 20 | 400 | Row subtitle |
| Footnote | 13 | 18 | 400 | Section header/footer in inset lists, captions |
| Caption 1 | 12 | 16 | 400 | Timestamps, meta |
| Caption 2 | 11 | 13 | 400 | Rare; smallest permitted |

- Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", "Helvetica Neue", system-ui, sans-serif`. Safari picks SF Pro Text below 20px and SF Pro Display at 20px and above automatically via `-apple-system`.
- Numbers in stats: same stack with `font-variant-numeric: tabular-nums`; Apple uses SF Pro Rounded for Fitness rings, approximate with `font-family: ui-rounded, -apple-system`.
- `-webkit-font-smoothing: antialiased; text-rendering: optimizeLegibility`.
- Respect Dynamic Type on the web: size text in `rem` and never clamp the root font size below 100%.
- Tracking: leave `letter-spacing: normal`. The system font carries SF's per-size optical tracking (tight at text sizes, looser at display sizes) and Safari applies it automatically. Hand-set tracking only when a fallback font is rendering, and then at most −0.4px at 17px and −0.3px at 28px+.
- Inset grouped section headers are Footnote 13 in sentence case since iOS 15. Uppercase 13px headers were the iOS 13–14 style; do not reproduce them.

## 2. Color

### Tints (system colors)

| Name | Light | Dark | Text on it |
|---|---|---|---|
| Blue (default tint) | #007AFF | #0A84FF | white |
| Green | #34C759 | #30D158 | white (black for small text) |
| Red | #FF3B30 | #FF453A | white |
| Orange | #FF9500 | #FF9F0A | black |
| Yellow | #FFCC00 | #FFD60A | black |
| Purple | #AF52DE | #BF5AF2 | white |
| Indigo | #5856D6 | #5E5CE6 | white |
| Teal | #30B0C7 | #40C8E0 | black |
| Pink | #FF2D55 | #FF375F | white |
| Gray | #8E8E93 | #8E8E93 | white |

Pick one tint for interactive elements. Red is reserved for destructive. Green/orange/red are allowed as status (RPE, trend) but not as a second accent.

### Semantic labels

| Token | Light | Dark |
|---|---|---|
| label | #000000 | #FFFFFF |
| label-secondary | rgba(60,60,67,.60) | rgba(235,235,245,.60) |
| label-tertiary | rgba(60,60,67,.30) | rgba(235,235,245,.30) |
| label-quaternary | rgba(60,60,67,.18) | rgba(235,235,245,.16) |
| placeholder | = label-tertiary | = label-tertiary |
| link | = tint | = tint |

### Backgrounds

| Token | Light | Dark | Use |
|---|---|---|---|
| bg | #FFFFFF | #000000 | Plain screens (content fills screen) |
| bg-secondary | #F2F2F7 | #1C1C1E | Elevated surfaces, cards on plain screen |
| bg-tertiary | #FFFFFF | #2C2C2E | Second elevation (sheets over cards) |
| bg-grouped | #F2F2F7 | #000000 | Grouped screens (Settings style) page background |
| bg-grouped-secondary | #FFFFFF | #1C1C1E | Inset grouped cells |
| bg-grouped-tertiary | #F2F2F7 | #2C2C2E | Controls inside cells |
| elevated dark | – | #1C1C1E → #2C2C2E → #3A3A3C | Each modal layer one step lighter; no shadows in dark |

### Fills and separators

| Token | Light | Dark |
|---|---|---|
| fill (buttons, thin controls) | rgba(120,120,128,.20) | rgba(120,120,128,.36) |
| tint-wash (tinted buttons, highlighted cards) | tint @ 12% | tint @ 15% |
| fill-secondary | rgba(120,120,128,.16) | rgba(120,120,128,.32) |
| fill-tertiary (segmented track, search bg) | rgba(118,118,128,.12) | rgba(118,118,128,.24) |
| fill-quaternary | rgba(116,116,128,.08) | rgba(118,118,128,.18) |
| separator | rgba(60,60,67,.29) | rgba(84,84,88,.60) |
| separator-opaque | #C6C6C8 | #38383A |

- Separator thickness is 0.5px (`border-top: .5px solid`; on 1x screens Safari rounds to 1px, that is fine). Inset the separator to the text's leading edge (typically 16px + icon width + 12px), not the cell edge.
- Contrast: 4.5:1 for text under 24px, 3:1 for 24px+ bold and for icons. Lime #DFE104 passes on black (≈15:1) but fails with white text (≈1.4:1), so lime surfaces take black text. In light mode a lime tint fails as text on white (#B8B800 ≈ 2.5:1); use it only as a surface with black text, or darken to ≈#6E6E00 (4.6:1) when it must be text or an icon.

## 3. Layout and sizing

| Metric | Value |
|---|---|
| Edge margin | 16 (compact), 20 (≥ 414px wide), 24+ on iPad |
| Grid | 8; micro spacing 4 |
| Minimum touch target | 44 × 44 |
| Row height | 44 (text only), 52–60 (title + subtitle), 76 (with 44 thumbnail) |
| Row padding | 11–12 vertical, 16 horizontal |
| Status bar | `env(safe-area-inset-top)` (≈ 47–59 on notch/Dynamic Island phones) |
| Nav bar | 44 tall; large title adds 52 (total 96) and collapses on scroll |
| Tab bar | 49 tall + `env(safe-area-inset-bottom)` (≈ 34 on home-indicator phones, 0 on home-button phones). Inside a desktop mock frame `env()` is 0, so use `max(env(safe-area-inset-bottom), 34px)` for the frame only |
| Toolbar | 44 |
| Search field | 36 tall, 10 radius, fill-tertiary bg, 17px text |
| Segmented control | 32 tall, 9 outer radius, 7 inner selected radius, 2 inset, 13/600 text |
| Switch | 51 × 31, 15.5 radius, knob 27 |
| Slider track | 4 tall; thumb 28 white with shadow |
| Filled button, large | 50 tall, 14 radius, 17/600 |
| Filled button, medium | 34 tall, 8–10 radius, 15/600 |
| Filled button, small (capsule) | 28 tall, 999 radius, 13/600, 14 h-padding |
| Sheet grabber | 36 × 5, 2.5 radius, label-tertiary, 5 from top |
| Sheet corner | 10 (UIKit default for every detent). Apps may raise it via `preferredCornerRadius`; if you do, use one value app-wide, 16 is the common choice |
| Numbered or icon-led rows | 29 × 29 leading square, 6.5 radius, tint or system-color fill, 15/600 glyph or number, 12 gap to text. Same spec for step lists |
| Badge / tag | 22 tall capsule, 11/600, 8–10 h-padding, fill or tint at 15%; the only place uppercase is acceptable |
| Focus ring | `outline: 2px solid tint; outline-offset: 2px` on `:focus-visible` only |
| Alert | 270 wide, 14 radius, Title 17/600 centered, body 13, buttons 44 tall split by hairline |
| Inset grouped list | 10 radius, 16 side margin, 35 gap between groups, header 13 footnote 16 left-padded |
| Card (Fitness/Health style) | 16–20 radius, 16 padding, bg-secondary, no border |
| App/screen corner | 38–55 continuous radius; match with `border-radius: 38px` on a full-screen mock frame |

Concentric radius rule: inner radius = outer radius − inset distance. A 16px-radius card with 12px padding holds 4px-radius children. Never nest two 12px radii.

## 4. Materials (translucency)

| Material | Background alpha | Blur | Where |
|---|---|---|---|
| Ultra thin | 55% | 20px | Over imagery |
| Thin | 70% | 20px | Overlays |
| Regular | 82% | 20px | Nav bar, tab bar, toolbars |
| Thick | 90% | 20px | Sidebars, popovers |

```css
.bar {
  background: color-mix(in srgb, var(--bg) 82%, transparent);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  backdrop-filter: blur(20px) saturate(180%);
  border-top: .5px solid var(--separator); /* tab bar */
}
@supports not (backdrop-filter: blur(1px)) { .bar { background: var(--bg); } }
@media (prefers-reduced-transparency: reduce) { .bar { background: var(--bg); backdrop-filter: none; } }
```

Bars become opaque when nothing scrolls beneath them (iOS 15+ "scroll edge" behaviour): toggle the hairline and material on when `scrollTop > 0`.

## 5. Motion

| Interaction | Duration | Curve |
|---|---|---|
| Button press | 100 in / 150 out | ease-out; `scale(.97)` + `opacity:.8` (plain) or darken 10% (filled) |
| Push navigation | 350 | `cubic-bezier(.32,.72,0,1)`; new view slides from right, old parallaxes left 30% |
| Sheet present | 350–500 | spring ≈ `cubic-bezier(.32,.72,0,1)`; backdrop fades to 40% black |
| Sheet dismiss | 250 | ease-in |
| Tab switch | 0 | none, instant; optional 150 cross-fade |
| Fade/appear | 200–250 | `cubic-bezier(.25,.1,.25,1)` |
| List reorder / layout | 300 | spring |
| Haptic | – | `navigator.vibrate` is unsupported on iOS Safari; do not fake it |

Rules: motion must convey hierarchy or state, never decoration. Never bounce chrome. Under `@media (prefers-reduced-motion: reduce)` replace slides with 150ms fades and remove parallax and auto-playing effects.

## 6. Components quick specs

- **Tab bar**: 2–5 items (HIG caps at 5; UIKit folds extras into "More", so a 6-tab web app should merge two tabs), equal width, icon 25×25 for circular glyphs and 23×23 for square ones, label 10/500 below with 2px gap, selected = tint for icon + label, unselected = gray #8E8E93. Never show text-only abbreviations instead of icons. Never hide the label.
- **Navigation bar**: centered 17/600 title or leading 34/700 large title; back button is chevron + previous title in tint; trailing actions are tint text or symbols, 44 wide.
- **Inset grouped list**: used for settings, forms, menus. Rows: leading icon in a 29×29 rounded (6.5 radius) colored square, 17 label, trailing value in label-secondary, chevron `›` in label-tertiary 14px.
- **Sheet**: medium detent ≈ 50% height, large detent to within 10px of top with the underlying view scaled 0.92 and darkened. Close by drag, swipe down, or a "Cancel"/"Done" in the sheet's own nav bar. No "X" in a circle at top-right except for full-screen media.
- **Alert**: for destructive or irreversible decisions only. Two buttons side by side, destructive in red, default in bold.
- **Form inputs**: 44 tall row, 17 text, placeholder = label-tertiary, no visible border in grouped lists (the cell is the border), `font-size ≥ 16px` to stop Safari zoom, `inputmode` set for numeric fields.
- **Empty state**: SF Symbol-style icon 48 in label-tertiary, Title 3 headline, Subheadline body, one tinted button.
- **Toasts**: iOS has none. Use a top banner (like Now Playing) or inline status instead.

## 7. Icons (SF Symbols style on the web)

- 24 × 24 viewBox drawn to fit 25×25 (circular) or 23×23 (square) in a tab bar, 1.75–2px stroke, round caps and joins, optical centering. Weight matches adjacent text weight.
- Use `currentColor` so tint/secondary propagates.
- Tab bar icons: filled variant when selected, outline when not (iOS 14 behaviour), or keep outline both states and change color only.
- Do not ship Apple's actual SF Symbols glyphs outside Apple platforms; draw equivalents (Lucide/Feather are the closest licensed sets).

## 8. Web shell checklist for a native feel

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)">
<meta name="theme-color" content="#F2F2F7" media="(prefers-color-scheme: light)">
```

```css
html { height: 100%; -webkit-text-size-adjust: 100%; }
body { min-height: 100dvh; overscroll-behavior: none; -webkit-tap-highlight-color: transparent;
       font: 17px/22px -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
       -webkit-font-smoothing: antialiased; }
button, input, select { font: inherit; -webkit-appearance: none; appearance: none; }
.scroll { -webkit-overflow-scrolling: touch; overscroll-behavior: contain; }
.tabbar { padding-bottom: env(safe-area-inset-bottom); }
.navbar { padding-top: env(safe-area-inset-top); }
a, button { touch-action: manipulation; user-select: none; -webkit-user-select: none; }
```

## 9. Writing

- Sentence case for everything except bar titles and proper nouns.
- Buttons are verbs that complete the sentence "I want to…": "Log Workout", "Save", "Delete Exercise". Never "OK", "Yes", "Submit".
- Keep labels to one or two words; let the layout, not the label, explain.
- Numbers get a thin space before units: "72 kg", "12 min".
- Error text says what to do, not what went wrong: "Enter a weight above 0".
