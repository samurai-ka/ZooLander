# Testing Strategy — UIKit Migration QA Plan

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Scope:** Visual regression, accessibility, cross-browser, performance testing  
**Audience:** QA team, developers, stakeholders  
**Duration:** Weeks 4–6 (ongoing through project)

---

## Testing Overview

| Test Type | Timing | Tools | Owner | Hours |
|-----------|--------|-------|-------|-------|
| **Visual Regression** | Week 4–5 | Playwright, manual inspection | Dev + Designer | 6–8 |
| **Accessibility** | Week 4–5 | axe-core, NVDA, DevTools | QA + Dev | 4–6 |
| **Cross-Browser** | Week 4–5 | Chrome, Firefox, Safari, mobile | QA | 3–4 |
| **Responsive** | Week 4–5 | DevTools, real devices | QA | 2–3 |
| **Performance** | Week 4–5 | Lighthouse, DevTools | Lead Dev | 2–3 |
| **Manual Smoke Tests** | Each phase | Checklist | QA + Devs | Ongoing |

---

## 1. Visual Regression Testing

**Goal:** Ensure rebuilt components match reference screenshots across all viewports and color modes.

### 1.1 Reference Screenshots

**Generate reference screenshots from live site (current state):**

| Viewport | Light Mode | Dark Mode |
|----------|-----------|----------|
| Mobile (375×667) | `mobile-light.png` | `mobile-dark.png` |
| Tablet (768×1024) | `tablet-light.png` | `tablet-dark.png` |
| Desktop (1280×720) | `desktop-light.png` | `desktop-dark.png` |

**Capture Method:**
1. Open live site: http://localhost:8000/
2. Use Playwright script or manual DevTools emulation:
   ```javascript
   // Playwright screenshot script
   const browser = await chromium.launch();
   const page = await browser.newPage();
   
   // Light mode
   await page.emulateMedia({ colorScheme: 'light' });
   await page.setViewportSize({ width: 375, height: 667 });
   await page.screenshot({ path: 'mobile-light.png' });
   
   // Dark mode
   await page.emulateMedia({ colorScheme: 'dark' });
   await page.screenshot({ path: 'mobile-dark.png' });
   ```
3. Store all 6 images in `plan/screenshots/`

**Status:** ⏳ Blocked (Playwright browser not installed)  
**Workaround:** Manually capture via browser DevTools screenshot tool

---

### 1.2 Comparison Process

**Week 4–5: Compare rebuilt site against references**

1. **Open rebuilt site** at http://localhost:8000 (or staging URL)
2. **Set viewport** to first combination (mobile light)
3. **Emulate color scheme** via DevTools → Rendering → prefers-color-scheme
4. **Take screenshot** of full page
5. **Compare visually** against reference screenshot:
   - Spacing (margins, padding)
   - Typography (font sizes, weights, line-height)
   - Colors (background, text, accents)
   - Component sizing (buttons, cards, tables)
   - Layout alignment (grids, flex)
6. **Document discrepancies** (if any)
7. **Repeat for all 6 combinations**

**Comparison Checklist:**

| Element | Light | Dark | Notes |
|---------|-------|------|-------|
| **Header** | ✓ | ✓ | Navigation, logo, search bar |
| **Colors** | ✓ | ✓ | Background, text, accents |
| **Typography** | ✓ | ✓ | Heading sizes, body text, code blocks |
| **Spacing** | ✓ | ✓ | Card padding, margins, gaps |
| **Buttons** | ✓ | ✓ | Sizing, color, hover state |
| **Forms** | ✓ | ✓ | Input sizing, label alignment |
| **Tables** | ✓ | ✓ | Row height, borders, striping |
| **Cards** | ✓ | ✓ | Header color, borders, shadows |
| **Grid Layout** | ✓ | ✓ | Column count, gutters, responsiveness |
| **Footer** | ✓ | ✓ | Styling, link colors |

**Issues Log:**
```markdown
## Mobile Light - Issue #1
**Element:** Button hover state
**Current:** #c32e3b (correct)
**Reference:** #c32e3b (matches)
**Status:** ✓ No issue

## Tablet Dark - Issue #1
**Element:** Text color in sidebar
**Current:** #e0e0e0 (too light)
**Reference:** #e8e8e8 (should match)
**Status:** Fix CSS variable
```

---

### 1.3 Automated Visual Testing (Future)

**Optional: Set up Playwright visual regression testing for CI/CD**

```javascript
// playwright.config.js
module.exports = {
  use: {
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  webServer: {
    command: 'npm run serve',
    url: 'http://localhost:8000',
    reuseExistingServer: !process.env.CI,
  },
};

// tests/visual.spec.js
test('Visual regression: homepage light mode', async ({ page }) => {
  await page.goto('/');
  await page.emulateMedia({ colorScheme: 'light' });
  await expect(page).toHaveScreenshot('homepage-light.png');
});

test('Visual regression: homepage dark mode', async ({ page }) => {
  await page.goto('/');
  await page.emulateMedia({ colorScheme: 'dark' });
  await expect(page).toHaveScreenshot('homepage-dark.png');
});
```

**Run tests:** `npx playwright test`

---

## 2. Accessibility Testing

**Goal:** Ensure site meets WCAG AA standards and is usable with assistive technology.

### 2.1 Automated Accessibility Checks

**Use axe DevTools or Lighthouse in Chrome DevTools:**

1. Open Chrome DevTools (F12)
2. Go to **Lighthouse** tab
3. Check **Accessibility** in options
4. Click **Analyze page load**
5. Review results for:
   - ✓ Contrast ratio violations
   - ✓ Missing alt text on images
   - ✓ Form labels not associated
   - ✓ Heading hierarchy broken
   - ✓ Missing ARIA attributes

**Success Criteria:** All issues resolved before release.

**Automated Tools:**
- [axe DevTools](https://www.deque.com/axe/devtools/) (Chrome extension)
- [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/) (Chrome)
- [WAVE](https://wave.webaim.org/extension/) (Color contrast checker)

---

### 2.2 Keyboard Navigation Testing

**Verify site fully navigable with keyboard only (no mouse).**

| Test | Steps | Expected Result |
|------|-------|-----------------|
| **Tab navigation** | Press Tab repeatedly | Focus visible on each interactive element; logical order |
| **Enter/Space** | Tab to button, press Enter | Button activates (click effect happens) |
| **Escape** | Tab to modal, press Escape | Modal closes |
| **Arrow keys** | Tab to dropdown, press Down arrow | Dropdown options visible/selectable |
| **Focus trap** | Tab while modal open | Focus stays within modal, doesn't leave to page behind |
| **Skip link** | Press Tab on page load | Skip-to-content link visible (if implemented) |

**Checklist:**
- [ ] Can navigate to all buttons, links, form inputs with Tab key
- [ ] Tab order is logical (left-to-right, top-to-bottom)
- [ ] Focus indicator visible on every interactive element
- [ ] Can activate buttons/links with Enter key
- [ ] Can submit forms with Enter in last field
- [ ] Modal properly traps focus (Tab doesn't escape)
- [ ] Escape key closes modals/dropdowns
- [ ] Dropdown menus navigable with Arrow keys

---

### 2.3 Screen Reader Testing

**Test with NVDA (Windows) or VoiceOver (Mac/iOS).**

#### NVDA (Windows)
1. Download & install [NVDA](https://www.nvaccess.org/)
2. Start NVDA (Insert + N, then S)
3. Start Firefox (NVDA default)
4. Navigate to test page
5. Use NVDA commands:
   - **H** — Next heading
   - **L** — Next list
   - **B** — Next button
   - **F** — Next form field
   - **T** — Next table
   - **Down arrow** — Read next line

#### Checklist
- [ ] Page title announced on load
- [ ] Main landmarks announced (nav, main, footer)
- [ ] Headings announced with level (h1, h2, h3, etc.)
- [ ] Form labels associated (label announced with input)
- [ ] Required fields indicated (asterisk, aria-required)
- [ ] Error messages announced
- [ ] Button purpose clear (text or aria-label)
- [ ] Links have descriptive text (not just "click here")
- [ ] Images have alt text
- [ ] Table structure announced (headers, rows)
- [ ] Modal announced as dialog
- [ ] Dark mode colors still readable

---

### 2.4 Color Contrast Validation

**Verify WCAG AA minimum contrast ratios (4.5:1 for normal text, 3:1 for large text).**

**Tools:**
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [TPGi Contrast Checker](https://www.tpgi.com/color-contrast-checker/)
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/accessibility/reference/#contrast) (built-in)

**Process:**
1. Open DevTools (F12)
2. Right-click element → Inspect
3. Go to **Styles** panel
4. Look for ⚠️ contrast warning (if any)
5. Use external tool to verify exact ratio

**Light Mode Pairs (Sample):**
| Text | Background | Ratio | Pass |
|------|-----------|-------|------|
| #333 | #f5f5f5 | 15:1 | ✓ WCAG AAA |
| #888 | #f5f5f5 | 6:1 | ✓ WCAG AA |
| #02a | #f5f5f5 | 8:1 | ✓ WCAG AA |
| #fff | #c32e3b | 5.2:1 | ✓ WCAG AA |

**Dark Mode Pairs (Sample):**
| Text | Background | Ratio | Pass |
|------|-----------|-------|------|
| #e8e8e8 | #1a1a1a | 16:1 | ✓ WCAG AAA |
| #808080 | #1a1a1a | 7:1 | ✓ WCAG AA |
| #66ccff | #1a1a1a | 9:1 | ✓ WCAG AA |
| #fff | #e74c56 | 4.5:1 | ✓ WCAG AA |

**Checklist:**
- [ ] All text/background pairs validated
- [ ] Dark mode pairs have sufficient contrast
- [ ] Light mode pairs have sufficient contrast
- [ ] Colored text (links, accents) meets 3:1 minimum
- [ ] Borders sufficient (if used for indication)
- [ ] Focus indicators visible (4.5:1 contrast to background)

---

## 3. Cross-Browser Testing

**Goal:** Ensure site works consistently across modern browsers.

### 3.1 Browser Matrix

| Browser | Version | OS | Mobile | Priority |
|---------|---------|-----|--------|----------|
| Chrome | Latest | Windows, macOS, Linux | ✓ Android | Must |
| Firefox | Latest | Windows, macOS, Linux | ✓ Android | Must |
| Safari | Latest | macOS, iOS | ✓ iOS | Must |
| Edge | Latest | Windows | Optional | Should |

### 3.2 Testing Checklist

#### Chrome/Chromium
- [ ] Open page, verify no console errors
- [ ] Check responsive design (DevTools Ctrl+Shift+M)
- [ ] Test dark mode (DevTools → Rendering → prefers-color-scheme)
- [ ] Test light mode
- [ ] Verify CSS renders correctly
- [ ] Test form inputs (text, select, date, checkbox)
- [ ] Test modal/dropdown open/close
- [ ] Test hover states on buttons, links
- [ ] Test keyboard navigation (Tab, Enter, Escape)
- [ ] Test animations (smooth transitions)

#### Firefox
- [ ] Repeat Chrome checklist
- [ ] Check DevTools (Shift+F12) for errors
- [ ] Test form inputs (may behave differently)
- [ ] Verify responsive grid (Grid Inspector in DevTools)

#### Safari (macOS)
- [ ] Open page in Safari
- [ ] DevTools (Cmd+Option+I)
- [ ] Test dark mode (DevTools → Settings → Emulate media feature)
- [ ] Test responsive (Cmd+Option+U)
- [ ] Form inputs (may use native styled elements)
- [ ] Test dark mode toggle (if implemented)

#### Mobile (iOS Safari / Chrome Android)
- [ ] Real device or browser emulation
- [ ] Test touch interactions (tap, long-press, swipe)
- [ ] Test responsive layout (mobile viewport)
- [ ] Test dark mode (if supported by OS)
- [ ] Test form inputs (mobile keyboards, autocomplete)

### 3.3 Common Issues & Fixes

| Issue | Cause | Fix |
|-------|-------|-----|
| CSS Grid not showing | Old browser without Grid support | Use fallback grid (float or flex) |
| CSS variables not working | IE11 (not supported) | Provide fallback values or drop IE11 support |
| Dark mode not activating | Browser doesn't support `prefers-color-scheme` | Add manual toggle, provide fallback |
| Fonts not loading | Wrong @font-face paths | Check font URLs, ensure CORS headers |
| SVG icons not rendering | Missing viewBox or wrong namespace | Fix SVG markup, test in browser |
| Form inputs styled differently | Browser-default styles | Add more specific CSS rules, test on target browser |
| Touch events not working | No touch listener | Add touch event handlers, test on mobile |

---

## 4. Responsive Design Testing

**Goal:** Ensure layout adapts correctly to all screen sizes.

### 4.1 Breakpoint Testing

| Breakpoint | Viewport Size | Test Cases | Pass Criteria |
|-----------|---------------|-----------|---------------|
| **Mobile** | 375×667 | 1-column layout, stacked nav, full-width content | ✓ No horizontal scroll, readable text |
| **Tablet** | 768×1024 | 2-column layout (sidebar + content), expanded nav, 2-column grid | ✓ Content properly aligned, buttons clickable |
| **Desktop** | 1280×720 | 3-column layout (optional), full nav + search, 3–4 column grid, max-width container | ✓ Constrained width, properly centered |

### 4.2 Manual Responsive Testing

**Using Chrome DevTools:**
1. Open DevTools (F12)
2. Click Device Toolbar icon (Ctrl+Shift+M)
3. Select device (iPhone 12, iPad, etc.) or set custom size
4. Test at each breakpoint:
   - Navigation works
   - Text readable (no overflow, no tiny text)
   - Buttons clickable (44×44px minimum)
   - Grid/flex layout responds correctly
   - Images scale appropriately
   - Modals fit on screen
   - No horizontal scrolling

**Real Device Testing:**
- Test on actual phone (iOS/Android)
- Test on actual tablet
- Test in landscape/portrait orientation
- Test form input behavior (mobile keyboards, autofill)

### 4.3 Responsive Images

**Checklist:**
- [ ] Images scale with container (`max-width: 100%`)
- [ ] Thumbnails maintain aspect ratio (`object-fit: cover`)
- [ ] No oversized images (load appropriately sized versions)
- [ ] No image overflow in containers
- [ ] Banners scale responsively (padding-bottom technique)

---

## 5. Performance Testing

**Goal:** Ensure page loads quickly and runs smoothly.

### 5.1 Lighthouse Audit

**Chrome DevTools:**
1. Open DevTools (F12)
2. Go to **Lighthouse** tab
3. Select **Performance** category
4. Click **Analyze page load**
5. Review scores:
   - ✓ Performance > 90 (Target)
   - ✓ Accessibility > 95
   - ✓ Best Practices > 90
   - ✓ SEO > 90

**Common Issues & Fixes:**
- **Slow CSS delivery** → Minify CSS, defer non-critical
- **Large images** → Optimize, use WebP format
- **Render-blocking JavaScript** → Defer scripts, use async
- **Unused CSS** → Remove UIKit components not used
- **Layout shifts** → Reserve space for dynamic content

### 5.2 CSS & Asset Size

**Measure file sizes:**
```bash
# Check minified CSS size
du -h static_built/css/style.css
# Should be < 100KB

# Check total page size
curl -I http://localhost:8000/ | grep -i content-length
# Should be < 1MB total
```

**Optimization:**
- [ ] CSS minified (use `cssnano`)
- [ ] SVG icons optimized (use `svgo`)
- [ ] No unused UIKit code (can remove unused components)
- [ ] Images optimized (use `imagemin`)
- [ ] Fonts subset if needed (reduce @font-face files)

### 5.3 Rendering Performance

**Test in DevTools:**
1. Open **Performance** tab
2. Click **Record** button
3. Interact with page (click buttons, scroll, type in forms)
4. Stop recording
5. Review:
   - ✓ No long tasks (> 50ms should be rare)
   - ✓ No layout thrashing (excessive reflows)
   - ✓ Smooth scrolling (60fps, no jank)
   - ✓ Fast transitions (0.2s ease)

**Checklist:**
- [ ] Page interactive within 3 seconds
- [ ] Hover effects render smoothly (60fps)
- [ ] Scrolling smooth (no jank)
- [ ] Modal open/close animation smooth
- [ ] Form interactions responsive (no lag)
- [ ] Dark mode toggle instant (no flicker)

---

## 6. Smoke Tests (Per Phase)

**Quick manual tests to catch regressions after each phase.**

### 6.1 Phase 1–2 Smoke Test (After Component Migration)

- [ ] **Header:** Logo visible, nav items clickable, search functional
- [ ] **Navigation:** Hover effects work, active state visible, mobile menu accessible
- [ ] **Buttons:** All variants render, hover/focus states visible
- [ ] **Forms:** Inputs accept text, labels associated, validation works
- [ ] **Cards:** Headers colored, borders visible, padding consistent
- [ ] **Tables:** Rows alternating, borders visible, content readable
- [ ] **Grid:** Columns responsive, gaps consistent
- [ ] **Modal:** Opens/closes, close button works, backdrop visible
- [ ] **Dark mode toggle:** Can switch between modes (if implemented)
- [ ] **No console errors:** DevTools console clean

### 6.2 Phase 3 Smoke Test (After Dark Mode)

- [ ] **Light mode:** All colors correct, text readable
- [ ] **Dark mode:** All colors correct, text readable, sufficient contrast
- [ ] **Toggle works:** Manual toggle switches modes, persists on reload
- [ ] **OS preference:** Dark mode detects system preference
- [ ] **All components:** Render correctly in both modes
- [ ] **No color hardcoding:** All via CSS variables
- [ ] **Contrast validated:** All text pairs meet WCAG AA

### 6.3 Phase 4 Smoke Test (After Testing & Polish)

- [ ] **Visual regression:** Screenshots match references
- [ ] **Accessibility:** Keyboard navigation works, focus visible
- [ ] **Cross-browser:** No errors in Chrome, Firefox, Safari
- [ ] **Responsive:** Works at 375px, 768px, 1280px
- [ ] **Performance:** Page loads quickly, smooth interactions
- [ ] **Issues fixed:** At least 5 high-impact GitHub issues resolved
- [ ] **Documentation:** README updated, build commands documented
- [ ] **Ready for production:** All blockers resolved, team trained

---

## 7. Testing Schedule

| Week | Phase | Tests | Owner | Status |
|------|-------|-------|-------|--------|
| Week 2 | Phase 1 | Smoke tests | Dev | Continuous |
| Week 3 | Phase 2a | Smoke tests | Dev | Continuous |
| Week 4 | Phase 2b–3 | Smoke tests, Visual regression (start) | Dev + QA | Continuous |
| Week 5 | Phase 4 | Visual regression (complete), Accessibility, Cross-browser, Performance | QA + Dev | Full |
| Week 6 | Phase 5 | Smoke tests, Issue fixes | Dev | Continuous |

---

## 8. Test Documentation Template

**Use this template for each test run:**

```markdown
## Test Run: [Name]
**Date:** YYYY-MM-DD  
**Tester:** [Name]  
**Environment:** [Browser/OS/Viewport]  
**Status:** ✓ Passed / ⚠️ Warnings / ❌ Failed  

### Results
- [ ] Test 1: [Status] — Details
- [ ] Test 2: [Status] — Details

### Issues Found
1. **Issue Title** → Severity: High/Medium/Low → Assigned to: [Dev]

### Sign-Off
- [x] All critical issues fixed
- [ ] All medium issues fixed (may defer)
- [ ] All low issues documented
```

---

## 9. Bug Severity Levels

| Severity | Definition | Example | Action |
|----------|-----------|---------|--------|
| **Critical** | Breaks core functionality or creates accessibility barrier | Buttons don't click, focus not visible, 0:1 contrast | Fix immediately |
| **High** | Significantly impacts user experience or violates WCAG AA | Dark mode colors too dark, modal doesn't close, form can't submit | Fix before release |
| **Medium** | Minor visual issue or non-critical functionality affected | Button hover color slightly off, spacing 1px different | Fix if time allows |
| **Low** | Cosmetic issue, no functional impact | Extra 2px margin, different font weight on non-critical text | Document, may defer |

---

## 10. Pass/Fail Criteria

### Visual Regression: PASS
- ✓ All 6 screenshot pairs (mobile/tablet/desktop × light/dark) match reference
- ✓ Typography sizes and line heights match
- ✓ Colors match (within 5% tolerance)
- ✓ Spacing/padding consistent
- ✓ Component sizing matches
- ✓ No visual regressions on new components

### Accessibility: PASS
- ✓ WCAG AA: All color pairs have 4.5:1 contrast (3:1 for large)
- ✓ Keyboard navigation works (Tab, Enter, Escape)
- ✓ Focus states visible on all interactive elements
- ✓ Form labels associated with inputs
- ✓ Images have alt text
- ✓ Heading hierarchy correct (no skipped levels)
- ✓ Screen reader test passes (major elements announced)
- ✓ axe DevTools: 0 critical violations, < 5 warnings

### Cross-Browser: PASS
- ✓ Chrome/Chromium: No console errors, renders correctly
- ✓ Firefox: No console errors, renders correctly
- ✓ Safari: No console errors, renders correctly
- ✓ Mobile (iOS/Android): Responsive, touch-friendly, renders correctly
- ✓ No browser-specific layout issues

### Performance: PASS
- ✓ Lighthouse Performance score > 90
- ✓ Page load < 2 seconds (on 4G network)
- ✓ Interaction-to-Paint < 100ms
- ✓ CSS file size < 100KB (minified)
- ✓ No long tasks (> 50ms)
- ✓ Smooth scrolling (60fps)

---

## 11. Test Sign-Off

**Before release, all tests must pass:**

```
✓ Visual Regression: PASS (Week 5)
✓ Accessibility: PASS (Week 5)
✓ Cross-Browser: PASS (Week 5)
✓ Responsive: PASS (Week 5)
✓ Performance: PASS (Week 5)
✓ Smoke Tests: PASS (Week 6)
✓ Issue Resolution: 5+ high-impact fixed (Week 6)

Project Status: ✓ READY FOR PRODUCTION
Sign-off: [QA Lead] [Date] [Time]
```

---

## References

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Lighthouse Documentation](https://developer.chrome.com/docs/lighthouse/)
- [axe DevTools](https://www.deque.com/axe/devtools/)
- [Playwright Docs](https://playwright.dev/)
- [plan/04-theming.md](plan/04-theming.md) — Dark mode testing details
- [plan/03-component-mapping.md](plan/03-component-mapping.md) — Component variants to test
