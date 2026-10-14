# Implementation Roadmap — UIKit Migration

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Total Duration:** 8–10 weeks  
**Team Size:** 2–3 developers, 1 designer  
**Effort:** ~500 person-hours total

---

## Phase Overview

| Phase | Duration | Focus | Deliverables |
|-------|----------|-------|--------------|
| **Phase 1** | Week 1–2 | Foundation | CSS variables, UIKit setup, build pipeline |
| **Phase 2** | Week 2–4 | Component Migration | Navigation, forms, buttons, cards, grids |
| **Phase 3** | Week 3–4 | Dark Mode | Color overrides, testing, polish |
| **Phase 4** | Week 4–5 | Testing & Polish | Screenshot comparison, accessibility audit |
| **Phase 5** | Week 5+ | Issue Resolution | High-impact GitHub issues, cleanup |

---

## Phase 1: Foundation (Weeks 1–2)

**Goal:** Set up UIKit framework and CSS architecture for migration

### Week 1: Setup & Planning

#### Task 1.1: Install & Configure UIKit
- [ ] Create `src/` directory structure (if not exists)
- [ ] Install UIKit via npm: `npm install uikit`
- [ ] Create `src/style/uikit-custom.scss`:
  ```scss
  // Import UIKit
  @import 'uikit/src/scss/variables-theme';
  @import 'uikit/src/scss/mixins-theme';
  
  // Override UIKit defaults with Demozoo colors
  $global-background: var(--bg-page);
  $global-color: var(--text-primary);
  $global-link-color: var(--color-primary-blue);
  $global-border: var(--border-medium);
  
  // Import rest of UIKit
  @import 'uikit/src/scss/uikit';
  ```
- [ ] Add build script to `package.json`:
  ```json
  {
    "scripts": {
      "build:css": "sass src/style/uikit-custom.scss static_built/css/style.css",
      "watch:css": "sass --watch src/style:static_built/css"
    }
  }
  ```
- [ ] Test build: `npm run build:css`

**Effort:** 4–6 hours  
**Owner:** Lead Developer  

---

#### Task 1.2: Create CSS Custom Properties File
- [ ] Create `src/style/utility/_custom-properties.scss`
- [ ] Define all 40+ color tokens (light mode):
  ```scss
  :root {
    --color-primary: #c32e3b;
    --color-primary-light: #ec5a64;
    --text-primary: #333;
    // ... 37 more tokens (see 01-tokens.md)
  }
  ```
- [ ] Define dark mode overrides:
  ```scss
  @media (prefers-color-scheme: dark) {
    :root {
      --color-primary: #e74c56;
      --text-primary: #e8e8e8;
      // ... dark variants
    }
  }
  ```
- [ ] Import custom properties into main SCSS file
- [ ] Verify CSS variables load in browser DevTools

**Effort:** 3–4 hours  
**Owner:** Lead Developer  

---

#### Task 1.3: Create Demozoo Custom Overrides File
- [ ] Create `src/style/demozoo-overrides.scss`
- [ ] Document all color overrides for UIKit components:
  ```scss
  // Navbar overrides
  .uk-navbar-container {
    background: var(--bg-surface);
    border-bottom: 1px solid var(--border-medium);
  }
  
  .uk-navbar-nav > li > a {
    color: var(--nav-link);
    font-family: FabricaRegular, sans-serif;
    text-transform: uppercase;
  }
  ```
- [ ] Add placeholder comments for phases 2–3 components
- [ ] Test with live site

**Effort:** 2–3 hours  
**Owner:** Lead Developer  

---

### Week 2: Documentation & Team Alignment

#### Task 1.4: Document SCSS Architecture
- [ ] Write internal `CONTRIBUTING.md` for CSS:
  - Layer order (utility → base → fonts → demozoo → uikit)
  - Naming conventions (BEM-inspired: `__element`, `--modifier`)
  - Color access via CSS variables (not hardcoded)
  - Spacing scale (2px, 4px, 8px, 16px, 24px, 32px)
  - Breakpoints (768px, 1024px, 1280px)
- [ ] Create style guide snippets:
  ```scss
  // ✅ DO: Use CSS variables
  .component {
    color: var(--text-primary);
    background: var(--bg-surface);
  }
  
  // ❌ DON'T: Hardcode colors
  .component {
    color: #333;
    background: #fff;
  }
  ```

**Effort:** 2–3 hours  
**Owner:** Lead Developer  

---

#### Task 1.5: Team Kickoff & Review
- [ ] Walkthrough `plan/` documentation with team
- [ ] Review component mapping (03-component-mapping.md)
- [ ] Review theming strategy (04-theming.md)
- [ ] Clarify UIKit vs. custom code decisions
- [ ] Assign Phase 2 component owners (nav, forms, buttons, etc.)
- [ ] Schedule daily standups

**Effort:** 1–2 hours  
**Owner:** Project Lead  

---

## Phase 2: Component Migration (Weeks 2–4)

**Goal:** Migrate all Demozoo UI elements to UIKit components

### Week 2: Navigation & Basic Components

#### Task 2.1: Navbar & Navigation
**Component:** `uk-navbar`

- [ ] Create navbar template in `src/templates/navbar.html`
- [ ] Implement structure:
  ```html
  <nav class="uk-navbar-container">
    <div class="uk-navbar-left">
      <a href="/" class="uk-navbar-item uk-logo">Demozoo</a>
      <ul class="uk-navbar-nav">
        <li><a href="/productions/">Releases</a></li>
        <li><a href="/groups/">Groups</a></li>
        <!-- ... -->
      </ul>
    </div>
    <div class="uk-navbar-right">
      <a href="/user/" class="uk-navbar-item">Profile</a>
    </div>
  </nav>
  ```
- [ ] Add Demozoo color overrides in `demozoo-overrides.scss`
- [ ] Test on 3 viewports (mobile, tablet, desktop)
- [ ] Test active state highlighting
- [ ] Test mobile hamburger menu (UIKit handles this automatically)

**QA Checklist:**
- [ ] Navigation text uppercase, FabricaRegular font
- [ ] Hover: 4px top border + `#ec5a64` color
- [ ] Active: `#ec5a64` background, white text
- [ ] Mobile: hamburger icon visible, menu functional
- [ ] Responsive: works at 375px, 768px, 1280px

**Effort:** 6–8 hours  
**Owner:** Developer 1  
**Deadline:** Mid Week 2  

---

#### Task 2.2: Search Bar
**Component:** `uk-search` + custom category select

- [ ] Create search template in `src/templates/search.html`
- [ ] Implement with UIKit search component and custom select
- [ ] Style search input background `#77c6ef` (light) or dark equivalent
- [ ] Add category dropdown
- [ ] Implement responsive adjustments (width changes at 501px, 601px)

**QA Checklist:**
- [ ] Input background: correct blue color
- [ ] Icon visible (search icon left-aligned)
- [ ] Category dropdown functional
- [ ] Submit button visible (dark background, search icon)
- [ ] Mobile: search bar full-width, below logo

**Effort:** 4–5 hours  
**Owner:** Developer 2  
**Deadline:** Mid Week 2  

---

#### Task 2.3: Buttons
**Component:** `uk-button` + custom sizing

- [ ] Create button system in `demozoo-overrides.scss`
- [ ] Implement variants:
  - Default button (light gray, dark text)
  - Primary button (red on hover)
  - Small button (action buttons)
  - Icon button (SVG-based)
- [ ] Add states: hover, active, disabled, focus
- [ ] Ensure uppercase text, FabricaRegular font, 0.2s transition
- [ ] Create button component library (Figma-ready)

**QA Checklist:**
- [ ] Default state: #f5f5f5 background, #888 text, #ddd border
- [ ] Hover state: #c32e3b background, #fff text
- [ ] Disabled: #ccc background, #999 text
- [ ] All states tested on light & dark modes
- [ ] Touch target size: minimum 44×44px

**Effort:** 5–6 hours  
**Owner:** Developer 1  
**Deadline:** End Week 2  

---

### Week 3: Forms & Complex Components

#### Task 2.4: Forms
**Component:** `uk-form-*` (form-label, form-controls, etc.)

- [ ] Create form template in `src/templates/forms.html`
- [ ] Implement horizontal layout (labels 150px, inputs 450px on desktop)
- [ ] Make responsive (stacked on mobile)
- [ ] Implement input variants:
  - Text inputs
  - Textareas
  - Selects
  - Date inputs
  - Checkboxes & radios
- [ ] Add help text styling
- [ ] Add error/focus states

**QA Checklist:**
- [ ] Label alignment correct (float-based or flex)
- [ ] Input sizing responsive (450px max on desktop, 90vw on mobile)
- [ ] Textarea height correct (300px, 50px for short)
- [ ] Focus state: `#77c6ef` border
- [ ] Error state: pink background + red border + error message
- [ ] All fields tested light/dark mode

**Effort:** 8–10 hours  
**Owner:** Developer 2  
**Deadline:** Mid Week 3  

---

#### Task 2.5: Cards & Panels
**Component:** `uk-card` + custom header styling

- [ ] Create card template with colored header option
- [ ] Implement panel alternative (simple cards without UIKit overhead)
- [ ] Style headers:
  - Red header (primary color)
  - Yellow header (tertiary color)
  - Optional header
- [ ] Add footer area (right-aligned buttons/text)
- [ ] Create responsive variants

**QA Checklist:**
- [ ] Card header background: correct color
- [ ] Header text: white, uppercase, FabricaRegular
- [ ] Header padding & border-right styling correct
- [ ] Card body padding: consistent spacing
- [ ] Footer area right-aligned
- [ ] Cards responsive on all viewports

**Effort:** 5–6 hours  
**Owner:** Developer 1  
**Deadline:** Mid Week 3  

---

#### Task 2.6: Tables & Data Display
**Component:** `uk-table` + custom styling

- [ ] Create table component with variants:
  - Default table
  - Striped (alternating rows: #f8f8f8)
  - Bordered (visible cell borders)
  - Hover (highlight on hover)
- [ ] Implement thumbnail cells (48×48px, object-fit: cover)
- [ ] Add metadata cell styling (secondary text color)
- [ ] Test with sample data

**QA Checklist:**
- [ ] Row height: 36px
- [ ] Cell padding: 0.5rem
- [ ] Striped rows: correct background color (#f8f8f8)
- [ ] Hover state: subtle background change
- [ ] Thumbnails: correct size, aspect ratio maintained
- [ ] All variants responsive

**Effort:** 4–5 hours  
**Owner:** Developer 2  
**Deadline:** Mid Week 3  

---

### Week 4: Grids, Modals & Carousels

#### Task 2.7: Responsive Grids
**Component:** `uk-grid` + responsive width classes

- [ ] Create grid template with responsive column layouts
- [ ] Implement viewport-specific breakpoints:
  - Mobile: 1 column (`uk-child-width-1-1`)
  - Tablet: 2 columns (`uk-child-width-1-2@m`)
  - Desktop: 3–4 columns (`uk-child-width-1-3@l`)
- [ ] Test with card components inside grid
- [ ] Verify gutter spacing (20px default)

**QA Checklist:**
- [ ] Mobile: single column full-width
- [ ] Tablet: 2-column layout
- [ ] Desktop: 3–4 column layout
- [ ] Gutter spacing consistent
- [ ] Cards responsive within grid cells
- [ ] All viewports tested

**Effort:** 3–4 hours  
**Owner:** Developer 1  
**Deadline:** Early Week 4  

---

#### Task 2.8: Modal & Lightbox
**Component:** `uk-modal` + custom styling

- [ ] Create modal template
- [ ] Implement structure:
  - Header (title, close button)
  - Body (content area)
  - Footer (action buttons)
- [ ] Create lightbox variant for image viewing
- [ ] Add responsive sizing (400–620px on desktop, fullscreen on mobile)
- [ ] Test close button (X icon, top-right)
- [ ] Test keyboard interaction (Escape to close)

**QA Checklist:**
- [ ] Modal background: dark overlay (#000 with transparency)
- [ ] Modal box: white background, border
- [ ] Close button: visible, clickable, correct position
- [ ] Escape key closes modal
- [ ] Modal responsive on mobile (fullscreen or near-fullscreen)
- [ ] Lightbox: image displays correctly

**Effort:** 4–5 hours  
**Owner:** Developer 2  
**Deadline:** Early Week 4  

---

#### Task 2.9: Carousel/Slideshow
**Component:** `uk-slideshow` or similar

- [ ] Create slideshow template for screenshots/images
- [ ] Implement navigation (prev/next buttons)
- [ ] Add thumbnail navigation (optional)
- [ ] Test with sample images
- [ ] Ensure responsive sizing (400×300px on desktop, full-width on mobile)

**QA Checklist:**
- [ ] Slideshow displays first image on load
- [ ] Prev/next buttons navigate slides
- [ ] Width/height responsive
- [ ] Navigation arrows hover effect visible
- [ ] Auto-play disabled (manual nav only)

**Effort:** 3–4 hours  
**Owner:** Developer 1  
**Deadline:** End Week 4  

---

#### Task 2.10: Typography & Text Utilities
**Component:** Native + UIKit text classes

- [ ] Create typography scale in CSS:
  - Headings (h1–h6) with FabricaRegular font
  - Text sizes (large, regular, small, meta)
  - Text utilities (truncate, uppercase, center, etc.)
- [ ] Implement all classes from [plan/01-tokens.md](plan/01-tokens.md)
- [ ] Test readability at each breakpoint

**QA Checklist:**
- [ ] Headings use FabricaRegular font
- [ ] Font sizes match spec (26pt h1, 20pt h2, 14pt h3, etc.)
- [ ] Line height: 1.5 for body
- [ ] Text utilities work (truncate, center, small, meta)
- [ ] All text readable (sufficient contrast)

**Effort:** 3–4 hours  
**Owner:** Developer 2  
**Deadline:** End Week 4  

---

**Phase 2 Summary:**
- [ ] All major components migrated to UIKit
- [ ] Demozoo color palette applied throughout
- [ ] Dark mode CSS variables in place (not activated yet)
- [ ] All components tested at 3 viewports
- [ ] Ready for Phase 3 (dark mode activation)

---

## Phase 3: Dark Mode (Weeks 3–4)

**Goal:** Activate and test dark mode across all components

### Parallel with Phase 2, Week 3+

#### Task 3.1: Apply Dark Mode Overrides
- [ ] Ensure all dark mode color tokens defined in CSS variables
- [ ] Test `@media (prefers-color-scheme: dark)` activation
- [ ] Verify all components render correctly in dark mode
- [ ] Check contrast ratios for accessibility

**QA Checklist:**
- [ ] Background: #1a1a1a (dark), #f5f5f5 (light)
- [ ] Text: #e8e8e8 (dark), #333 (light)
- [ ] Borders: #333 (dark), #eee (light)
- [ ] All contrast ratios: WCAG AA (4.5:1 minimum)
- [ ] No hardcoded colors visible (all via CSS variables)

**Effort:** 2–3 hours  
**Owner:** Lead Developer  

---

#### Task 3.2: Manual Toggle (Optional)
- [ ] Implement theme toggle button (optional feature)
- [ ] Add JavaScript to detect/store user preference
- [ ] Create `data-theme` attribute logic
- [ ] Test override of OS preference

**QA Checklist:**
- [ ] Toggle button visible in header/settings
- [ ] Toggle switches between light/dark
- [ ] Preference persisted in localStorage
- [ ] Manual choice overrides OS preference
- [ ] All components render correctly with toggle

**Effort:** 2–3 hours  
**Owner:** Developer 1 or 2  
**Deadline:** Week 4  

---

#### Task 3.3: Dark Mode Screenshots
- [ ] Capture screenshots of all 6 combinations:
  - Mobile light/dark
  - Tablet light/dark
  - Desktop light/dark
- [ ] Save to `plan/screenshots/`
- [ ] Compare against live site if available

**Effort:** 1–2 hours  
**Owner:** Whoever captured light mode screenshots  
**Deadline:** Week 4  

---

## Phase 4: Testing & Polish (Weeks 4–5)

**Goal:** Verify rebuild matches reference, fix bugs, ensure accessibility

### Week 4–5: Visual & Accessibility Testing

#### Task 4.1: Visual Regression Testing
- [ ] Compare rebuild screenshots with reference screenshots
- [ ] Use Playwright or manual visual inspection
- [ ] Document any discrepancies
- [ ] Fix rendering issues (sizing, spacing, colors)

**QA Checklist:**
- [ ] All 6 screenshot pairs compared (mobile/tablet/desktop × light/dark)
- [ ] Spacing matches (margins, padding, gaps)
- [ ] Colors accurate (light/dark modes)
- [ ] Text sizing and line height match
- [ ] Images/icons display correctly
- [ ] No layout shifts or oversized elements

**Effort:** 6–8 hours  
**Owner:** Developer 1 + Designer  

---

#### Task 4.2: Accessibility Audit
- [ ] Test keyboard navigation (Tab, Enter, Escape)
- [ ] Verify focus states visible (outlines, underlines)
- [ ] Check color contrast with automated tool
- [ ] Test with screen reader (NVDA, JAWS, or VoiceOver)
- [ ] Verify semantic HTML (headings, labels, landmarks)
- [ ] Check ARIA attributes where needed

**QA Checklist:**
- [ ] Tab order logical and useful
- [ ] Focus indicators visible on all interactive elements
- [ ] All color pairs: WCAG AA contrast (4.5:1 for normal text)
- [ ] Form labels associated with inputs
- [ ] Images have alt text
- [ ] Page structure semantic (landmarks, headings)
- [ ] Modals: focus trap, close button accessible

**Effort:** 4–6 hours  
**Owner:** Developer 2 + QA  

---

#### Task 4.3: Cross-Browser Testing
- [ ] Test on Chrome, Firefox, Safari (latest versions)
- [ ] Test mobile browsers (Chrome Android, Safari iOS)
- [ ] Check CSS compatibility (vendor prefixes via Autoprefixer)
- [ ] Verify SVG icons render correctly
- [ ] Test dark mode in each browser

**QA Checklist:**
- [ ] No console errors or warnings
- [ ] CSS renders consistently across browsers
- [ ] Images/fonts load correctly
- [ ] Responsive behavior works (mobile view works)
- [ ] Dark mode toggle functional
- [ ] No visual regressions in any browser

**Effort:** 3–4 hours  
**Owner:** QA Lead  

---

#### Task 4.4: Performance Optimization
- [ ] Minify CSS output
- [ ] Optimize images (SVGO for SVGs)
- [ ] Check page load time (target: < 2s)
- [ ] Verify no unused CSS (if using UIKit, some will be unused)
- [ ] Test on slow network (DevTools throttle)

**QA Checklist:**
- [ ] CSS file size reasonable (< 100KB minified)
- [ ] No render-blocking resources
- [ ] Images optimized
- [ ] Page loads quickly on fast/slow networks
- [ ] No layout shifts (CLS acceptable)

**Effort:** 2–3 hours  
**Owner:** Lead Developer  

---

## Phase 5: Issue Resolution (Week 5+)

**Goal:** Address high-priority GitHub issues using UIKit

See [plan/06-issues-uikit.md](plan/06-issues-uikit.md) for issue prioritization.

### High-Impact Issues (Quick Wins)

#### Issue #608: User Menu Profile Link
**Component:** `uk-navbar-dropdown`  
**Effort:** 1–2 hours  
**Priority:** High  

- [ ] Add profile link to user menu dropdown
- [ ] Test dropdown opens/closes
- [ ] Verify on all viewports

---

#### Issue #249: SVG Icon Migration
**Component:** `uk-icon` system  
**Effort:** 4–6 hours  
**Priority:** High  

- [ ] Convert PNG icons to SVG
- [ ] Create custom icon font or UIKit icon library
- [ ] Update icon usage throughout site
- [ ] Test icon coloring via CSS

---

#### Issue #468: Multi-Column BBS Grid
**Component:** `uk-grid`  
**Effort:** 2–3 hours  
**Priority:** High  

- [ ] Use UIKit Grid for multi-column layout
- [ ] Implement responsive columns (1 mobile, 2 tablet, 3+ desktop)
- [ ] Test word wrapping and overflow

---

#### Issue #305: Consolidate Buttons
**Component:** `uk-button-group`  
**Effort:** 1–2 hours  
**Priority:** High  

- [ ] Use UIKit button groups for multi-action buttons
- [ ] Group "Update" / "Done" buttons on compo page
- [ ] Test button group styling and spacing

---

#### Issue #600: Truncate Alias Lists
**Component:** Text utilities + overflow  
**Effort:** 1–2 hours  
**Priority:** High  

- [ ] Use UIKit text-truncate utility
- [ ] Limit alias list visually with ellipsis
- [ ] Test on all viewports

---

### Medium-Impact Issues (Phase 2)

#### Issue #662: Tag Tooltips
**Component:** `uk-tooltip` or custom popover  
**Effort:** 3–4 hours  
**Priority:** Medium  

- [ ] Show tag description on hover/focus
- [ ] Implement tooltip positioning
- [ ] Test on all viewports

---

#### Issue #537: Editable Comments
**Component:** Custom form overlay + `uk-modal`  
**Effort:** 4–5 hours  
**Priority:** Medium  

- [ ] Add edit button to comments
- [ ] Open modal with textarea on edit
- [ ] Submit changes via API
- [ ] Test on all viewports

---

#### Task 5.1: High-Impact Issues (Week 5)
- [ ] Complete issues #608, #249, #468, #305, #600
- [ ] Test each issue fix
- [ ] Update GitHub issues with "Fixed" status

**Effort:** 10–15 hours  
**Owner:** Developer 1  
**Deadline:** Week 5  

---

#### Task 5.2: Medium-Impact Issues (Week 6)
- [ ] Complete issues #662, #537, #278, #390
- [ ] Some require backend integration
- [ ] Coordinate with backend team if needed

**Effort:** 12–16 hours  
**Owner:** Developer 2  
**Deadline:** Week 6  

---

#### Task 5.3: Cleanup & Documentation
- [ ] Remove unused CSS/JavaScript
- [ ] Document custom components
- [ ] Update README with UIKit version, build commands
- [ ] Archive old CSS files (don't delete—keep for reference)

**Effort:** 4–5 hours  
**Owner:** Lead Developer  
**Deadline:** Week 6  

---

## Milestone Summary

| Milestone | Week | Deliverable |
|-----------|------|-------------|
| **Phase 1 Complete** | Week 2 | UIKit installed, CSS variables defined, team aligned |
| **Phase 2 Complete** | Week 4 | All components migrated, light mode complete |
| **Phase 3 Complete** | Week 4 | Dark mode activated, tested across viewports |
| **Phase 4 Complete** | Week 5 | Visual regression testing, accessibility audit passed |
| **Phase 5a Complete** | Week 5 | High-impact issues resolved (#608, #249, #468, #305, #600) |
| **Phase 5b Complete** | Week 6 | Medium-impact issues resolved (#662, #537, etc.) |
| **Project Complete** | Week 6 | Cleanup, documentation, handoff to team |

---

## Resource Allocation

### Team
- **Lead Developer:** 15–20 hours/week (setup, oversight, CSS variables, performance)
- **Developer 1:** 20–25 hours/week (navigation, buttons, cards, grids, issues)
- **Developer 2:** 20–25 hours/week (search, forms, tables, modals, issues)
- **Designer:** 8–10 hours/week (visual review, Figma updates, QA)
- **QA:** 10–12 hours/week (testing, accessibility, cross-browser)
- **Project Lead:** 5–8 hours/week (planning, standups, issue resolution)

**Total Team Effort:** ~500 person-hours over 8–10 weeks

---

## Risk Mitigation

### Risk: UIKit Doesn't Match Demozoo Aesthetic
**Mitigation:**
- Use CSS variable overrides liberally
- Custom CSS for Demozoo-specific styling
- Keep "shame.scss" for temporary fixes

### Risk: Dark Mode Contrast Issues
**Mitigation:**
- Validate all color pairs before Phase 3
- Use automated contrast checker (WebAIM, TPGi)
- Test with screen reader on dark mode

### Risk: Performance Degradation
**Mitigation:**
- Profile CSS file size early (Phase 1)
- Remove unused UIKit code if needed
- Test page load on slow network (Week 4)

### Risk: Schedule Slippage
**Mitigation:**
- Daily standups to catch blockers
- Prioritize high-impact issues first
- Push lower-priority tasks to Phase 5b or later

---

## Success Criteria

- [ ] All components migrated to UIKit
- [ ] Light & dark modes fully functional
- [ ] All 6 screenshot combinations match reference
- [ ] Accessibility audit: WCAG AA compliance
- [ ] Cross-browser testing: 0 critical bugs
- [ ] Performance: page load < 2s
- [ ] At least 5 high-impact GitHub issues resolved
- [ ] Team trained on new codebase
- [ ] Documentation complete

---

## References

- [plan/01-tokens.md](plan/01-tokens.md) — Design tokens
- [plan/03-component-mapping.md](plan/03-component-mapping.md) — UIKit component mappings
- [plan/06-issues-uikit.md](plan/06-issues-uikit.md) — GitHub issues prioritization
- UIKit Docs: https://getuikit.com/docs/introduction
- WCAG Accessibility: https://www.w3.org/WAI/WCAG21/quickref/
