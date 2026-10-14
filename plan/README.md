# UIKit Migration Analysis — Plan Documentation

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Project:** ZooLander (Demozoo redesign with UIKit)  
**Analysis Date:** 2026-10-14  
**Status:** Analysis Phase Complete — Ready for Design & Implementation

---

## Overview

This folder contains comprehensive analysis documentation for migrating the Demozoo site from its current custom CSS to the UIKit framework. All documents are written in English.

---

## Documents Index

### 1. **01-tokens.md** — Design Tokens
**Purpose:** Complete color, typography, spacing, and breakpoint definitions

**Key Contents:**
- Color palette (light + dark modes): 40+ tokens
- Font stack (primary: system fonts, headings: FabricaRegular, monospace: DejaVu)
- Spacing & sizing scale (xs, sm, base, md, lg, xl)
- Breakpoints: Mobile (< 768px), Tablet (768–1023px), Desktop (1024px+)
- Dark mode color overrides (WCAG AA compliant)
- Component-specific tokens (buttons, forms, navigation)

**Use Case:** CSS custom properties for theming, design system foundation

---

### 2. **02-layout.md** — Page Structure & Viewports
**Purpose:** Define responsive layout for mobile, tablet, desktop

**Key Contents:**
- Viewport definitions (375px mobile, 768px tablet, 1280px desktop)
- Header layout (logo, nav, search, user menu)
- Content area layouts (1-column, 2-column, 3-column grids)
- Footer structure
- Responsive media & images
- UIKit Grid system integration
- Accessibility & mobile considerations

**Use Case:** Responsive design planning, breakpoint strategy

---

### 3. **03-component-mapping.md** — Demozoo Elements → UIKit Components
**Purpose:** Map every existing UI element to appropriate UIKit component

**Key Contents:**
- Navigation (`uk-navbar`, `uk-dropdown`)
- Logo & branding (custom, image-based)
- Search bar (`uk-search` + custom select)
- User menu (`uk-navbar-dropdown`)
- Page headers & titles (`<h1>–<h6>` + custom styling)
- Content blocks & panels (`uk-card`)
- Forms & inputs (`uk-form-horizontal`, `uk-input`, `uk-select`)
- Buttons & actions (`uk-button`, `uk-button-group`)
- Data display (tables, lists, grids via `uk-grid`)
- Media & images (thumbnails, carousels via `uk-slideshow`)
- Modals & overlays (`uk-modal`)
- Typography utilities (text classes)
- Icons (`uk-icon` SVG system)
- Component gaps (tagit, autocomplete, drag-drop—need custom solutions)

**Use Case:** Implementation blueprint, component selection guide

---

### 4. **04-theming.md** — Dark Mode & Theming Strategy
**Purpose:** Define light/dark mode implementation with OS preference support

**Key Contents:**
- Current state: Light mode only, no dark mode
- Recommended approach: CSS Custom Properties (not UIKit's limited inverse classes)
- Light mode token definitions (40+ color variables)
- Dark mode overrides (all tokens darkened for WCAG AA contrast)
- Manual theme toggle (optional JavaScript-based override)
- UIKit integration with dark mode
- WCAG accessibility compliance validation
- Testing strategy (DevTools emulation, reference screenshots)
- Implementation timeline (25–29 hours total)

**Use Case:** Dark mode implementation plan, CSS architecture

---

### 5. **06-issues-uikit.md** — Demozoo GitHub Issues Analysis
**Purpose:** Evaluate which open issues can be solved with UIKit

**Key Contents:**
- 20 frontend/UI-relevant issues identified from 100 total
- **9 "Yes"** — UIKit directly solves (e.g., #608 user menu, #249 SVG icons, #468 multi-column grid)
- **10 "Partly"** — UIKit handles UI, custom logic required (e.g., #662 tag tooltips, #278 drag-drop upload)
- **1 "No"** — UIKit not applicable (e.g., #290 ranking drag-drop, needs Sortable.js)
- High-impact quick-wins (profile link, SVG migration, button consolidation)
- Medium-impact features (editable comments, time-frame filters)
- UIKit component usage summary (Grid/Flex, Dropdown/Navbar, Button Groups, Form Controls, Text Utilities, Icons, Tooltip/Popover)

**Use Case:** Issue prioritization, feature delivery roadmap

---

### 6. **05-figma-spec.md** — Figma Design Specification
**Status:** ✅ **Built** — https://www.figma.com/design/kfGHazLuR6aPKTMw0lIYjn (homepage only; see 09-figma-file-and-corrections.md)

**Expected Contents:**
- Component inventory (all UIKit components + Demozoo customizations)
- Component variants (button sizes, states, color modes)
- Design tokens as Figma styles (colors, typography, shadows)
- Layout templates (mobile, tablet, desktop)
- Light/Dark mode styles
- Interaction states (hover, focus, disabled, loading)

**Next Step:** Once Figma MCP is authorized in claude.ai connector settings, this document will be converted to a Figma file with full component library.

---

### 7. **screenshots/** — Reference Images
**Status:** ⏳ **BLOCKED** — Awaiting Playwright Browser Installation

**Expected Contents:**
- `mobile-light.png` (375×667) — Light mode mobile reference
- `mobile-dark.png` (375×667) — Dark mode mobile reference
- `tablet-light.png` (768×1024) — Light mode tablet reference
- `tablet-dark.png` (768×1024) — Dark mode tablet reference
- `desktop-light.png` (1280×720) — Light mode desktop reference
- `desktop-dark.png` (1280×720) — Dark mode desktop reference

**Use Case:** Visual regression testing, design comparison during rebuild

---

## Key Findings

### ✅ What UIKit Solves Well
1. **Navigation & Menus:** Navbar, dropdowns, user menus
2. **Grid Layout:** Responsive multi-column layouts with breakpoints
3. **Forms & Inputs:** Consistent styling, built-in validation states
4. **Buttons & Icons:** Flexible button groups, SVG icon system
5. **Cards & Panels:** Container styling with header/body/footer sections
6. **Modals & Lightbox:** Modal dialogs, overlays
7. **Text Utilities:** Typography, spacing, alignment classes
8. **Tables:** Structured data with striping, borders, hover effects

### 🔶 What UIKit Needs Customization
1. **Colors:** Demozoo's specific palette (reds, blues) not in UIKit defaults
2. **Fonts:** FabricaRegular custom font for headings (UIKit uses system fonts)
3. **Dark Mode:** UIKit has limited inverse classes; CSS variables recommended
4. **Component Gaps:** Tag input, autocomplete, drag-drop require external libraries or custom JS
5. **Spacing Adjustments:** Demozoo's 4.5% padding; UIKit defaults may need tweaking

### ❌ What UIKit Doesn't Handle
1. **Drag-drop Sorting:** Need Sortable.js or similar (issue #290)
2. **Tag Input:** Need external library like Tagify or Selectize
3. **Autocomplete:** Need custom implementation or library
4. **Editable Grids:** Need custom JS + state management

---

## Design System Decisions

### Colors
- **40+ tokens** defined in CSS custom properties
- Light mode (existing): `#c32e3b` red, `#02a` blue, `#f5f5f5` background
- Dark mode (new): Lightened variants for WCAG AA compliance
- All colors organized by function (primary, secondary, backgrounds, text, components)

### Typography
- **Body:** System font stack (Apple system, Segoe UI, Roboto, etc.)
- **Headings:** FabricaRegular (existing custom font, preserved)
- **Monospace:** DejaVu Sans Mono (code, pre blocks)
- **Retro:** Topaz, IBM VGA (heritage fonts for demo/text files)

### Spacing
- **Scale:** 2px (xs), 4px (sm), 8px (base), 12–16px (md), 20–25px (lg), 30–36px (xl)
- **Breakpoints:** Mobile < 768px, Tablet 768–1023px, Desktop 1024px+
- **Container:** 1400px max-width, 4.5% horizontal padding on desktop

### Dark Mode
- **Strategy:** CSS custom properties + `@media (prefers-color-scheme: dark)` + optional manual toggle
- **Coverage:** All 40+ color tokens have dark overrides
- **Compliance:** WCAG AA contrast ratios validated
- **Not using:** UIKit's inverse classes (too limited, Demozoo colors not supported)

---

## Implementation Roadmap

### Phase 1: Foundation (Weeks 1–2)
- [ ] Set up CSS custom properties file (colors, spacing, breakpoints)
- [ ] Create UIKit integration stylesheet
- [ ] Document all color overrides

### Phase 2: Component Migration (Weeks 2–4)
- [ ] Navigation: Replace with `uk-navbar`
- [ ] Forms: Replace `.entry_form` with UIKit form classes
- [ ] Buttons: Use `uk-button` + `uk-button-group`
- [ ] Cards: Replace `.block`/`.panel` with `uk-card`
- [ ] Grid: Use `uk-grid` + responsive width classes

### Phase 3: Dark Mode (Weeks 3–4)
- [ ] Apply `@media (prefers-color-scheme: dark)` overrides
- [ ] Test all 6 viewport + color combinations
- [ ] Add optional manual theme toggle (if desired)

### Phase 4: Polish & Testing (Weeks 4–5)
- [ ] Screenshot comparison with references
- [ ] Accessibility testing (keyboard nav, screen readers)
- [ ] Cross-browser testing (Chrome, Firefox, Safari)
- [ ] Mobile device testing (actual phones/tablets if possible)

### Phase 5: Issue Resolution (Week 5+)
- [ ] Tackle high-impact issues (#608, #249, #468, #305, #600)
- [ ] Integrate medium-impact features (#662, #537, #278, #390)
- [ ] Document custom solutions for UIKit gaps

**Total Estimated Effort:** 8–10 weeks

---

## Blockers & Next Steps

### ✅ Resolved: Figma MCP (file built — see 09-figma-file-and-corrections.md)
- **Issue:** Cannot create Figma design file yet
- **Resolution:** Authorize Figma MCP in claude.ai connector settings, then run `/blueprint-figma-spec.md`
- **Workaround:** Use 05-figma-spec.md text specification as reference until Figma is available

### 🔴 Blocker 2: Playwright Browser Not Installed
- **Issue:** Cannot capture reference screenshots
- **Resolution:** Install Playwright Chrome: `npm install @playwright/test && npx playwright install chrome`
- **Workaround:** Use existing live site screenshots manually captured or fetch via curl

### 🟡 Optional: ZooKeeper Repo Analysis
- **Status:** Parallel agent still running (analyzing `/samurai-ka/ZooKeeper`)
- **Use:** Detailed source code review may provide additional insights
- **Not critical:** Main analysis complete without it

---

## File Structure

```
plan/
├── README.md                    ← This file
├── 01-tokens.md               ← Colors, fonts, spacing, breakpoints
├── 02-layout.md               ← Page structure, viewports, grids
├── 03-component-mapping.md    ← Demozoo → UIKit component map
├── 04-theming.md              ← Dark mode, CSS variables, testing
├── 05-figma-spec.md           ← (Future) Figma design specification
├── 06-issues-uikit.md         ← GitHub issues evaluated against UIKit
└── screenshots/               ← (Future) Reference screenshots
    ├── mobile-light.png
    ├── mobile-dark.png
    ├── tablet-light.png
    ├── tablet-dark.png
    ├── desktop-light.png
    └── desktop-dark.png
```

---

## How to Use This Documentation

### For Designers
1. Start with **02-layout.md** (viewport strategy)
2. Review **01-tokens.md** (colors, typography, spacing)
3. Use **03-component-mapping.md** to select UIKit components
4. Reference **04-theming.md** for dark mode planning

### For Developers
1. Read **03-component-mapping.md** for component selection
2. Follow **01-tokens.md** for CSS custom property setup
3. Use **04-theming.md** for dark mode implementation
4. Reference **06-issues-uikit.md** for feature prioritization

### For Project Managers
1. Review this **README.md** for overview
2. Check **06-issues-uikit.md** for deliverable mapping
3. Reference implementation roadmap (above) for timeline

---

## Key Metrics

| Metric | Value | Notes |
|--------|-------|-------|
| **Design Tokens** | 40+ | Colors, fonts, spacing, breakpoints |
| **UIKit Components** | 20+ | Navbar, buttons, forms, grid, cards, modal, etc. |
| **Demozoo Issues Analyzed** | 20 | From 100 total; 9 direct solutions, 10 partial, 1 not applicable |
| **Viewports** | 3 (+ 2 color modes) | Mobile, Tablet, Desktop × Light/Dark = 6 total |
| **Color Pairs** | 40+ | All WCAG AA compliant for contrast |
| **Expected Effort** | 8–10 weeks | Full migration + polish + testing |

---

## References

- **Live Site:** http://localhost:8000/
- **ZooKeeper Repo:** https://github.com/samurai-ka/ZooKeeper
- **Demozoo Issues:** https://github.com/demozoo/demozoo/issues
- **UIKit Docs:** https://getuikit.com/docs/introduction
- **UIKit Source:** https://github.com/uikit/uikit

---

## Contact & Questions

For detailed information on any aspect of this analysis, refer to the specific document:
- **Colors & Typography?** → `01-tokens.md`
- **Layout & Breakpoints?** → `02-layout.md`
- **Which UIKit Component?** → `03-component-mapping.md`
- **Dark Mode?** → `04-theming.md`
- **Which Issues to Fix?** → `06-issues-uikit.md`

---

**Last Updated:** 2026-10-14  
**Next Review:** After Figma MCP authorization or Playwright setup
