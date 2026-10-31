# Harmony+ — Audit Report

**Date:** 2026-10-31  
**Auditor:** OpenCode mission run  
**Resolved project root:** `<workspace>/Harmony+`  
**Installed copy used for runtime checks:** `C:\Program Files (x86)\Steam\millennium\themes\HarmonyPlus`  
**Runtime environment:** Steam client changelist `11146165`, CEF `126.0.6478.183`, Millennium Core `2.30.0`

---

## 1. Starting state — no usable source

The workspace `Harmony+` folder contained **only reference images and a ZIP of third-party themes**:

- `exact font and icons.png`
- `theme colors.png`
- `Screenshot_1.png`
- `other skins for idea.zip` (ATOMS / NEVKO-UI / Steam2.0-modular)

There was **no Harmony+ source tree, no Git repository, and no remote**.

A previous attempt was found only in the installed Millennium themes folder (`%STEAM%\millennium\themes\HarmonyPlus`) and was preserved before replacement in `_archive/2026-10-31/HarmonyPlus-installed-skeleton`.

### Previous installed skeleton — findings

| Finding | Severity | Evidence | Action |
| --- | --- | --- | --- |
| Used `manifest.json` + `theme.js` + `steam.stylesheets` instead of Millennium’s required `skin.json` | Blocker | File read; Millennium docs require `skin.json` | Replaced with a valid `skin.json` |
| `steam.stylesheets` contained Tailwind directives (`@tailwind base`) that are not compiled | Blocker | File read | Removed; vanilla CSS only |
| `assets/fonts/YourCustomFont-Regular.ttf` and `.woff2` were **0 bytes** | Blocker | File size check | Replaced with verified Montserrat TTF files |
| `css/main.css` was an 81-byte placeholder | Blocker | File read | Removed |
| Selectors such as `.sidebar`, `.steam-main-container`, `.button` do not exist in Steam | Blocker | Live DOM inspection | Replaced with verified Steam selectors |
| `theme.js` looked for `.sidebar` and `.layout-toggle`, injected fake chat hooks, and never worked | High | File read | Removed; no JS in the rebuild |
| `manifest.json` described a “Millennium plugin compatible” theme but no plugin existed | High | File read | Removed claim and file |
| Font/icon placeholders (`sample-icon.svg`, empty fonts) were presented as assets | High | File read | Replaced with real TTFs and original SVGs |
| No documented settings, no conditions, no root colours | High | File read | Implemented five real `Conditions` groups |

The skeleton was **not** a working theme. It was not modified in place; a new source tree was built in the workspace and then installed.

---

## 2. Design brief audit

| Requirement | Status | Evidence |
| --- | --- | --- |
| Exact accent `#105FEB` | Implemented | `--hx-blue: #105feb` in `libraryroot.custom.css`; computed value in runtime |
| Milk-white / near-white text | Implemented | `--hx-text: #f7f9ff`; body computed colour |
| Montserrat ExtraBold, TTF, not WOFF2 | Implemented | `assets/fonts/Montserrat-ExtraBold.ttf`, 455,468 bytes, family “Montserrat ExtraBold”, 2,747 glyphs |
| Windows 11 Fluent / Microsoft Store inspiration | Implemented | Rounded glass panels, hairlines, soft blue atmosphere, restrained shadows |
| Cozy, soothing, cute, minimal, premium | Implemented by design system | Palette, radii, type scale, motion, copy tone |
| Rounded buttons/inputs/panels/cards/chat | Implemented | Token radii 9/12/18/24px; menus, dialogs, chat composer |
| Redesigned sidebar hierarchy | Implemented | Original `harmony+` wordmark, uppercase section labels, selected pill, rail surface |
| Compact / spacious settings | Implemented | `density-compact`, `density-cozy`, `density-spacious` |
| Keyboard accessibility & focus states | Implemented | `:focus-visible` rings on buttons, tiles, rows, links, chat controls |
| Preserve native chat behaviour | Implemented | CSS only; no JS, no DOM replacement, no fake features |
| Preserve SVG fills/strokes | Implemented | No global SVG fill overrides; original icons keep their own colours |
| Keep native Steam features | Implemented | No hiding of window controls, no fixed overlays, no pointer-blocking pseudo-elements |

### Reference handling

The owner’s references were used for **palette, weight, roundness and mood only**. The Paramount+ mountain/stars mark, logo and artwork are **not** included. The `harmony+` mark and icon set are original vector artwork. Third-party reference themes in `other skins for idea.zip` were inspected for supported Millennium mechanisms, not copied.

---

## 3. Implementation

### Files created

```
Harmony+/
├── skin.json
├── libraryroot.custom.css
├── friends.custom.css
├── bigpicture.custom.css
├── webkit.css
├── options/
│   ├── density-compact.css, density-cozy.css, density-spacious.css
│   ├── motion-calm.css, motion-still.css
│   ├── glow-soft.css, glow-none.css
│   ├── radius-soft.css, radius-crisp.css
│   └── type-montserrat.css, type-system.css
├── assets/
│   ├── fonts/ (3 real TTF + OFL)
│   ├── icons/ (10 original SVGs)
│   ├── logo-harmony.svg
│   └── previews/ (original artwork)
├── docs/DESIGN_RESEARCH.md
├── AUDIT_REPORT.md
└── VALIDATION_REPORT.md
```

### Architecture

- Custom `Patches` list (documented) rather than `UseDefaultPatches`, so store/community browser views can be included. No `Steam-WebKit` combination.
- `libraryroot.custom.css` for the main window, `friends.custom.css` for Friends/chat, `bigpicture.custom.css` for Gamepad UI, `webkit.css` for store/community.
- Base defaults use `:where(:root)` so the selected `Conditions` file always wins the cascade, regardless of injection order. This was found and fixed after a runtime test showed the System font option being overridden by the base file.

---

## 4. Runtime evidence

Verified with the live Steam client and Millennium:

1. **Theme load:** HarmonyPlus CSS links were present and the active nav pill used `linear-gradient(rgb(61,124,255), rgb(16,95,235))`.
2. **Default conditions:** `density-cozy.css`, `motion-calm.css`, `glow-soft.css`, `radius-soft.css`, `type-montserrat.css` loaded.
3. **Non-default conditions:** `density-compact.css`, `motion-still.css`, `glow-none.css`, `radius-crisp.css`, `type-system.css` loaded; computed body font changed to Motiva Sans and `--hx-glow-opacity` became `0`.
4. **Library home:** initial, selected game, sidebar, shelves and tiles inspected at 1812×982.
5. **Game details:** rounded play bar, blue Play button, glass panels, uppercase Montserrat section headings inspected.
6. **Friends/Chat:** detached chat window inspected; composer, titlebar, tabs, invite button and message list rendered; native controls remained present.
7. **Settings:** Steam Settings window inspected with the theme loaded; buttons, rounded cards, frame and typography rendered.
8. **Store:** store page background, font and section heading rules applied; `FEATURED & RECOMMENDED` rendered in Montserrat uppercase blue.

### Not verified

- Big Picture.
- Full chat interaction matrix (typing, attachments, emoji, reactions, unread/typing states).
- Community profiles and store checkout.
- 125%/150% DPI, multi-monitor, maximized/restored transitions.
- Keyboard-only walkthrough and screen-reader output.
- Windows 10.

The `Settings` runtime screenshot contained private account details and was deliberately not published. Published previews are original design artwork, not client screenshots.

---

## 5. Remaining risks

1. Steam DOM changes can invalidate hashed anchors; semantic classes are used where possible.
2. Millennium Core 2.30.0 was used. Current 3.x should load the same schema but was not tested.
3. Store CSS order required raised specificity; a future store rewrite may require updates.
4. Big Picture is token-only and unverified.
5. Font rendering/weight at 125–150% scaling was not inspected.

---

## 6. Next exact task

Install through the Millennium UI on the owner’s current client, then verify in order:

1. Big Picture / Quick Access.
2. Chat typing, attachment picker, emoji and reaction states in a test conversation.
3. Store checkout smoke test and a community profile.
4. 125%/150% DPI and a second monitor.
