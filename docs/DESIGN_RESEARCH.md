# Harmony+ — Design Research

**Date checked:** 2026-10-31  
**Purpose:** record the Fluent/cozy design decisions and what is original.

---

## 1. Owner references (used for direction only)

The workspace `Harmony+` folder contained:

- `exact font and icons.png` — a streaming-service campaign composition: deep blue field, milk-white ExtraBold uppercase type, star/arc logo language.
- `theme colors.png` — a single blue swatch.
- `Screenshot_1.png` — a form with a white rounded input and a blue rounded submit button.
- `other skins for idea.zip` — ATOMS, NEVKO-UI, Steam2.0-modular reference themes.

Extracted principles:

- **Exact blue `#105FEB`** as the only brand accent.
- **Milk-white type**, not default Steam grey.
- **ExtraBold, tightly tracked uppercase** for display.
- **Large rounded controls** (the reference input/button are pill/soft-rounded).
- **Calm, premium composition** with plenty of breathing room.

Not used: the reference brand’s mountain/stars logo, wordmark, campaign art, or any third-party theme code/assets.

---

## 2. Technical sources (verified)

| Source | Key fact used | Consequence |
| --- | --- | --- |
| [Millennium structure](https://docs.steambrew.app/themes/basics/structure) | `skin.json` required; conventional file names with `UseDefaultPatches` | Custom `Patches` chosen for store/community coverage |
| [Config](https://docs.steambrew.app/themes/basics/config) | Patches match body class, title, browser URL; do not combine `UseDefaultPatches` with `Patches`/`Steam-WebKit` | One documented strategy |
| [Conditions](https://docs.steambrew.app/themes/intermediate/user) | Dropdowns/checkboxes with `TargetCss` and `affects` | Five real options |
| [Root colors](https://docs.steambrew.app/themes/intermediate/colors) | `:root`-only colour editing | Not used; the brief fixes the accent and the options are structural |
| [Performance](https://docs.steambrew.app/themes/advanced/performance) | Avoid costly animations and giant blurs | Transform/opacity, one blur layer, reduced-motion support |

---

## 3. Visual references and extracted principles

| Reference | Principle taken | Not taken |
| --- | --- | --- |
| Windows 11 Fluent Design / Microsoft Store | Layered surfaces, acrylic-like translucency, 1px strokes, rounded geometry, quiet blue accent, generous spacing | No Microsoft logos, icons or exact layouts |
| [Dribbble — Fluent design](https://dribbble.com/search/fluent-design) | Soft depth and restrained hierarchy | No specific composition |
| [Dribbble — glassmorphism](https://dribbble.com/search/glassmorphism) | Translucent panels with a clear edge, not a blur-heavy look | No heavy full-screen blur |
| [Pinterest — Windows 11 Fluent UI](https://www.pinterest.com/search/pins/?q=windows%2011%20fluent%20ui) | Calm neutral surfaces, blue accent, rounded cards | Pinterest results were not consistently openable; no specific pin claimed |
| Steam’s own library/game-detail layout | Preserve the real information architecture and native controls | No replacement of Steam functionality |

---

## 4. Design system

### Palette

| Token | Value | Use |
| --- | --- | --- |
| `--hx-blue` | `#105FEB` | Exact accent, primary actions, selection, focus |
| `--hx-blue-hi` | `#3D7CFF` | Gradient top, hover |
| `--hx-blue-lo` | `#0A46B8` | Gradient bottom |
| `--hx-bg` / `--hx-bg-2` | `#080C15` / `#0B1120` | Calm midnight canvas |
| `--hx-surface` | `rgba(20,28,46,0.72)` | Glass panels |
| `--hx-text` | `#F7F9FF` | Milk-white primary text |
| `--hx-text-soft` | `#C8D2E6` | Secondary text |
| `--hx-text-muted` | `#8D9AB4` | Labels, metadata |
| `--hx-line` | `rgba(255,255,255,0.08)` | Hairlines |
| Semantic | `#5BD6A0` online, `#F2BF5E` away, `#7F8BA3` offline, `#F07A7A` danger | Preserved, never recoloured away |

### Typography

- **Display:** Montserrat ExtraBold (TTF, weight 800), uppercase, letter-spacing `0.10em`.
- **UI/body:** Montserrat Regular/SemiBold for a softer geometric read.
- **Fallback option:** Steam’s Motiva Sans stack.
- Scale: 10–11px uppercase labels, 13–14px body, 16–20px section headings, 22–24px dialog headings, wordmark 20px.

### Geometry and material

- Radii: 9 / 12 / 18 / 24px, full pills for navigation and chat badges.
- Glass panels: gradient from `rgba(23,32,52,0.66)` to `rgba(13,19,32,0.70)`, 1px border, inset top highlight.
- Shadows: soft, low-opacity, no giant stacks.
- Hover: 3px lift on game tiles, blue 1px ring and a soft blue bloom.
- Focus: 2px blue outline with 4px offset ring on interactive surfaces.
- In-client frame: 1px white/7% hairline + a cool top light; native titlebar untouched.

### Wordmark and icons

- Original `harmony+` mark: a rounded blue squircle with a wave-like white “h” and a spark; used at 18–20px in the top bar and rail.
- Ten original 24px line icons: home, library, friends, chat, store, downloads, settings, search, play, bell, star, sparkle. Kept as standalone assets; the theme does not globally override Steam’s icons.

### Motion

- Calm: 140/220/360 ms, 3px lift.
- Still: 0 ms and no lift.
- Reduced-motion media query always wins.

---

## 5. Originality statement

The `harmony+` mark, icon set and preview artwork are original. No streaming-service logo, campaign art, third-party theme code, or unlicensed font is included. Montserrat is bundled under the SIL Open Font License.
