# Figma File & Corrections

**Date:** 2026-10-14
**Figma file:** https://www.figma.com/design/kfGHazLuR6aPKTMw0lIYjn (ZooLander – UIKit Design System, Figma plan "Greenbrain", Starter tier)
**Status:** Authoritative for colours, contrast and breakpoints. Where `01-tokens.md`, `02-layout.md`, `04-theming.md`, `05-figma-spec.md` or `README.md` disagree with this file, this file wins.

---

## 1. What is in the Figma file

| Page | Content |
|------|---------|
| **Foundations** | Colour specimen (Light and Dark, 24 tokens each, with hex), type ramp (9 text styles), spacing and radius scales |
| **Components** | 9 component sets, 41 variants, all colours/spacing/radius bound to variables: Button (12), Input (6), Badge (3), Nav Item (6), Panel Header (2), Production Card (2), Table Row (4), Header (4), Footer (2) |
| **Screens** | Home page at Mobile 375, Tablet 768, Desktop 1280, each in Light and Dark (6 frames), built only from component instances |

**Variable collections:** `Primitives` (32), `Color Light` (24), `Color Dark` (24), `Spacing` (8), `Radius` (4), `Layout` (8: container max, UIKit breakpoints, viewport widths). All variables carry explicit scopes and a WEB code syntax such as `var(--color-bg-page)`.

### Light/Dark in Figma (plan limitation)

The Figma Starter plan allows **one mode per variable collection**, so a single collection with Light and Dark modes is not possible. Workaround used:

- Two collections, `Color Light` and `Color Dark`, with identical variable names.
- Every colour-dependent component has a `Theme=Light|Dark` variant property bound to the matching collection.
- Badge and Panel Header have no Theme property because their fills and text colours are identical in both themes.

On a Professional plan the two collections can be merged into one collection with two modes and the Theme property dropped. In code nothing changes: the CSS custom property names are the same in both themes and `prefers-color-scheme` switches their values.

### Fonts (stand-ins)

| Site font | Figma stand-in | Reason |
|-----------|----------------|--------|
| FabricaRegular (display) | Archivo | Fabrica is not available in Figma |
| System font stack (body) | Inter | Closest neutral UI sans |
| DejaVu Sans Mono (code) | JetBrains Mono | DejaVu is not available in Figma |

In the real build keep the `@font-face` files from `/static/font/`.

### Not done

- **No visual comparison against the live site.** The Playwright browser could not be installed in this environment, so `plan/screenshots/` does not exist. The Figma design is derived from `dz.css`, the page HTML and the ZooKeeper source, not from rendered screenshots.
- Only the homepage is designed. Production, party, forum and form pages are not.
- No Code Connect mappings, no Figma variables-to-CSS export.

---

## 2. Verified colour palette (WCAG 2.1, contrast computed)

Earlier documents claimed every colour pair passed WCAG AA. **That was not verified and several pairs fail**, including some in the existing site. The palette below was computed and is what the Figma file uses.

### Existing-site colours that fail, and the replacement

| Use | Existing | Contrast | Replacement | Contrast |
|-----|----------|----------|-------------|----------|
| Meta / secondary text on white | `#888` | 3.54:1 | `#666` (`color/text/muted`) | 5.74:1 |
| Active nav item, white text | `#ec5a64` | 3.38:1 | `#c32e3b` (`color/action/primary`) | 5.56:1 |
| Badge / signpost text, white on `#6bc7f1` | white | 1.90:1 | `#1a1a1a` on `#6bc7f1` | 9.17:1 |
| Panel title text, white on `#a5b45d` | white | 2.26:1 | `#1a1a1a` on `#a5b45d` | 7.70:1 |
| Input border on white | `#ddd` | 1.4:1 (needs 3:1) | `#767676` (`color/border/strong`) | 4.54:1 |

`#ec5a64` stays only as a decorative nav hover line (`color/accent/decor`), which has no text contrast requirement.

### Dark palette corrections

| Earlier proposal | Problem | Used instead |
|------------------|---------|--------------|
| `#e74c56` as button hover with white text | 3.77:1, fails | Fill stays `#c32e3b` (5.56:1); `#ff7a84` only as text colour on dark (`color/text/brand`, 6.34:1 on `#222`) |
| `#808080` tertiary text on `#1a1a1a` / `#222` | 4.41:1 / 4.03:1, fails | `#a3a3a3` (6.90:1 / 6.31:1) |
| `#9966ff` visited link on `#222` | 4.32:1, fails | `#b399ff` (6.76:1) |

### Token table

Variable names are the CSS custom properties without the `--color-` prefix, e.g. `color/bg/page` is `--color-bg-page`.

| Token | Light | Dark |
|-------|-------|------|
| `color/bg/page` | `#f5f5f5` | `#1a1a1a` |
| `color/bg/surface` | `#ffffff` | `#222222` |
| `color/bg/footer` | `#e8e8e8` | `#2a2a2a` |
| `color/bg/stripe` | `#f8f8f8` | `#262626` |
| `color/bg/control` | `#f5f5f5` | `#333333` |
| `color/border/subtle` | `#eeeeee` | `#333333` |
| `color/border/default` | `#dddddd` | `#444444` |
| `color/border/strong` (inputs) | `#767676` | `#8a8a8a` |
| `color/text/primary` | `#333333` | `#e8e8e8` |
| `color/text/secondary` | `#555555` | `#b0b0b0` |
| `color/text/muted` | `#666666` | `#a3a3a3` |
| `color/text/on-primary` | `#ffffff` | `#ffffff` |
| `color/text/on-accent` | `#1a1a1a` | `#1a1a1a` |
| `color/text/brand` | `#c32e3b` | `#ff7a84` |
| `color/link/default` | `#0022aa` | `#66ccff` |
| `color/link/visited` | `#4422aa` | `#b399ff` |
| `color/action/primary` | `#c32e3b` | `#c32e3b` |
| `color/action/primary-pressed` | `#a3252f` | `#a3252f` |
| `color/accent/decor` | `#ec5a64` | `#ec5a64` |
| `color/accent/signpost` | `#6bc7f1` | `#6bc7f1` |
| `color/accent/olive` | `#a5b45d` | `#a5b45d` |
| `color/accent/panel-border` | `#ed89a3` | `#ed89a3` |
| `color/accent/search-field` | `#77c6ef` | `#77c6ef` |
| `color/accent/search-button` | `#386580` | `#386580` |

Key text pairs (all ≥ 4.5:1): `#333` on `#f5f5f5` 11.59; `#666` on `#f8f8f8`/`#eee` ≥ 4.95; `#0022aa` on `#fff` 11.61; white on `#386580` 6.28; `#1a1a1a` on `#77c6ef` 9.21; `#e8e8e8` on `#1a1a1a` 14.20; `#b0b0b0` on `#222` 7.34; `#66ccff` on `#222` 8.82.

---

## 3. UIKit breakpoints (correction)

`02-layout.md` and `05-figma-spec.md` list `@m = 768px`, `@l = 1024px`, `@xl = 1400px`. **UIKit's actual breakpoints are:**

| Suffix | Min width |
|--------|-----------|
| `@s` | 640px |
| `@m` | 960px |
| `@l` | 1200px |
| `@xl` | 1600px |

Consequences used in the Figma screens:

| Viewport | Width | UIKit state | Header | Card grid |
|----------|-------|-------------|--------|-----------|
| Mobile | 375 | below `@s` | Compact (toggle + offcanvas) | 1 column |
| Tablet | 768 | `@s` up, below `@m` | Compact (nav hidden until 960) | 2 columns (`uk-child-width-1-2@s`) |
| Desktop | 1280 | `@l` up | Full navbar | 4 columns (`uk-child-width-1-4@l`) |

The existing site switches its nav layout at 1081px and 601px/501px and its two-column activity block at 768px. The Figma design keeps the 768px two-column activity block but follows UIKit for the header.

Other deviations from the existing CSS, all deliberate:

- Base font size is 14px on every viewport (existing site uses 12px below 768px).
- Desktop content padding is 58px (4.5% of 1280, as on the existing site); mobile 16px, tablet 24px.
- Buttons have square corners (UIKit default, `radius/none`); the old `.action_button` had 3px.

---

## 4. Other corrections to earlier documents

- **Effort and timeline** in `07-implementation-roadmap.md` (8 to 10 weeks, about 500 hours, named team roles) are estimates I made up, not derived from this codebase. Treat them as placeholders.
- **`04-theming.md` CSS variable names** (`--bg-page`, `--text-primary`, …) differ from the Figma code syntax (`--color-bg-page`, `--color-text-primary`, …). Use the Figma names; they are consistent across Light and Dark.
- **UIKit dark mode:** `04-theming.md` says UIKit has no built-in dark mode switch and recommends CSS custom properties. This was not checked against the UIKit docs or source in this session; verify before relying on it.
- **`06-issues-uikit.md`:** the issue assessments came from an agent reading issue titles, not full issue threads or the UIKit source. Check each "Yes" against the UIKit docs before committing to it.
