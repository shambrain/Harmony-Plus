# Harmony+ — Validation Report

**Date:** 2026-10-31  
**Theme version:** 1.0.0  
**Validator:** `_tools/validate_theme.py` (tinycss2, fontTools, ElementTree)  
**Result:** **Static checks passed** — 0 errors.

This report separates static validation from Steam runtime testing.

---

## 1. Static checks (passed)

```
=== Harmony+
strategy: Patches
css_files: 15
important_count: 52
unreferenced_css: []
svg_count: 13
fonts: Montserrat ExtraBold / Regular / SemiBold, 2747 glyphs each
js_files: []
file_count: 45
STATIC RESULT: PASS (no errors)
```

Checks performed:

- `skin.json` parses, has required metadata and owner `shambrain`.
- Exactly one injection strategy: custom `Patches`; no forbidden combination.
- Every patch and condition target exists.
- Every `affects` regex compiles.
- Every condition default exists.
- All 15 CSS files parse without syntax errors.
- All relative `url()` references resolve.
- All 13 SVGs parse as XML.
- All 3 TTF files have valid sfnt headers, non-zero sizes and correct metadata (Montserrat ExtraBold/Regular/SemiBold, 2,747 glyphs, unitsPerEm 1000).
- No JS files.
- No `.steamui_*` guesses.
- No unreferenced CSS files.

### Warnings and assessment

- Universal-selector warnings are from `prefers-reduced-motion` resets in `bigpicture`, `friends`, `libraryroot` and `webkit` (`html body *` in the store file). No universal colour/background/transform rules exist.
- `!important` count is 52. Targeted uses:
  - store surfaces (store CSS loads after theme CSS);
  - primary dialog buttons (Steam’s `.DialogButton.Primary` gradient wins by specificity);
  - critical top-bar/page backgrounds and reduced-motion resets.
  No global `* { color: … !important }` rule exists.

---

## 2. Asset validation

| Asset | Check | Result |
| --- | --- | --- |
| `assets/fonts/Montserrat-ExtraBold.ttf` | 455,468 B, family “Montserrat ExtraBold”, 2,747 glyphs | Valid |
| `Montserrat-SemiBold.ttf` | 454,716 B, family “Montserrat SemiBold” | Valid |
| `Montserrat-Regular.ttf` | 445,928 B, family “Montserrat Regular” | Valid |
| `assets/fonts/Montserrat-OFL.txt` | SIL OFL 1.1 licence | Valid |
| `assets/logo-harmony.svg` + 10 icons | XML parse, own fills/strokes | Valid |
| `assets/previews/*.png` | original generated artwork | Present |

No 0-byte fonts, no placeholder icons, no third-party campaign art or logos.

`_references/` (owner-supplied images and the third-party theme ZIP) is preserved locally and excluded from Git and release packages.

---

## 3. Steam runtime testing (actually performed)

**Environment:** Steam changelist `11146165`, CEF `126.0.6478.183`, Millennium Core `2.30.0`, Windows. CEF remote debugging enabled.

| Surface / check | Evidence | Status |
| --- | --- | --- |
| Theme discovery/load | CSS links served from `https://millennium.host/v1/themes/HarmonyPlus/...` | Runtime tested |
| Active nav accent | Computed `linear-gradient(rgb(61,124,255), rgb(16,95,235))` | Runtime tested |
| Default conditions | `density-cozy`, `motion-calm`, `glow-soft`, `radius-soft`, `type-montserrat` loaded | Runtime tested |
| Non-default conditions | `density-compact`, `motion-still`, `glow-none`, `radius-crisp`, `type-system` loaded; body font changed to Motiva Sans; glow token `0` | Runtime tested |
| Cascade fix | Base defaults moved to `:where(:root)`; System option then took effect | Runtime tested |
| Library home | Sidebar wordmark, section labels, selected Home pill, rounded tiles, shelves inspected | Runtime tested |
| Game details | Rounded play bar, blue Play button, glass panels, uppercase headings inspected | Runtime tested |
| Friends/Chat | Detached chat inspected: titlebar, tabs, composer, invite button, message list | Runtime tested |
| Settings window | Theme frame/buttons/cards/typography inspected; screenshot withheld (private account data) | Runtime tested |
| Store shell | Dark surface, Montserrat heading, accent applied | Runtime tested |
| Big Picture | Not opened | Not verified |
| Full chat interaction matrix | Not executed | Not verified |
| Community / checkout | Not executed | Not verified |
| DPI/multi-monitor/keyboard-only | Not executed | Not verified |

Screenshots were inspected locally. Published preview images are **original design artwork**, not client screenshots, to avoid exposing private account/friend data.

---

## 4. Package checks

| Check | Result |
| --- | --- |
| Release ZIP | `dist/Harmony-Plus-1.0.0.zip` |
| Extracted root | `HarmonyPlus/skin.json` directly inside the ZIP root folder |
| Extracted file count | 42 |
| `_references`, `.git`, `.cursor`, local backups | Absent |
| Personal data strings | No account names, emails or local user paths |
| Preview images | Original generated artwork, small PNGs |

ZIP size and SHA-256 are recorded in the workspace release notes (`OPENCODE_PROGRESS.md`) after the final build, because a ZIP cannot contain its own hash.

The ZIP was extracted to a temporary folder and inspected before publication.

---

## 5. Honest conclusion

- **Implemented:** yes, built from scratch.
- **Static checks passed:** yes.
- **Runtime tested:** yes, for the surfaces listed above.
- **Fully verified across all Steam surfaces/options:** no.
- **Known blockers:** none for the core theme; remaining items are unverified coverage, not known failures.
