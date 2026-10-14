# Dark Mode & Theming Strategy for UIKit Migration

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Scope:** Light/Dark mode implementation with OS preference support  
**UIKit Version Target:** UIKit 3.x (current stable) with CSS custom properties for future UIKit 5+ compatibility

---

## Executive Summary

The current Demozoo site has **no dark mode**. The new UIKit-based rebuild will introduce:

1. **OS Preference Detection** via `prefers-color-scheme` media query
2. **Manual Toggle** (optional, for user override)
3. **CSS Custom Properties** (recommended over UIKit's limited inverse classes)
4. **Comprehensive Color Overrides** for all Demozoo-specific colors

---

## Current State Analysis

- **Light mode:** Fully defined in `dz.css` with 40+ color tokens
- **Dark mode:** Not implemented
- **UIKit Inverse:** UIKit has `uk-light` and `uk-dark` classes, but they only style UIKit components. Demozoo custom colors are ignored.

---

## Recommended Approach: CSS Custom Properties

### Why CSS Custom Properties?

| Approach | Pros | Cons | Recommendation |
|----------|------|------|-----------------|
| **CSS Custom Properties** | Full control, maintainable, future-proof | Requires CSS refactor | **✅ RECOMMENDED** |
| **UIKit Inverse Classes** | Built-in UIKit support | Limited to UIKit components, doesn't style Demozoo colors | Only for UIKit components |
| **SCSS/LESS Mixins** | Familiar to CSS preprocessor users | Harder to toggle at runtime | No, CSS variables are simpler |
| **JavaScript Toggle** | Runtime flexibility | No performance benefit, more code | No, use CSS variables |

---

## Implementation: CSS Custom Properties

### Step 1: Define Light Mode Tokens

Create a new file `static/css/variables.css` (or embed in main CSS):

```css
/* Light mode (default) */
:root {
  /* Primary colors */
  --color-primary: #c32e3b;
  --color-primary-light: #ec5a64;
  --color-primary-blue: #02a;
  --color-primary-blue-visited: #42a;
  
  /* Secondary colors */
  --color-signpost: #6bc7f1;
  --color-panel-title: #a5b45d;
  --color-search: #77c6ef;
  --color-success: #bfb;
  --color-error: #fbb;
  
  /* Backgrounds */
  --bg-page: #f5f5f5;
  --bg-surface: #fff;
  --bg-footer: #e8e8e8;
  --bg-input: #fff;
  
  /* Borders */
  --border-light: #eee;
  --border-medium: #ddd;
  --border-dark: #ccc;
  --border-darker: #aaa;
  
  /* Text */
  --text-primary: #333;
  --text-secondary: #444;
  --text-tertiary: #888;
  --text-muted: #666;
  
  /* Components */
  --button-bg: #f5f5f5;
  --button-text: #888;
  --button-border: #ddd;
  --button-hover-bg: #c32e3b;
  --button-hover-text: #fff;
  
  --nav-link: #5e5f5f;
  --nav-link-hover: #ec5a64;
  
  --form-text: #333;
  --form-placeholder: #888;
  --form-disabled: #888;
  
  --input-border: #ddd;
  --input-focus-border: #77c6ef;
}
```

### Step 2: Define Dark Mode Overrides

```css
/* Dark mode (auto-detect or manual toggle) */
@media (prefers-color-scheme: dark) {
  :root {
    /* Primary colors — lightened for dark background contrast */
    --color-primary: #e74c56;
    --color-primary-light: #f47f88;
    --color-primary-blue: #66ccff;
    --color-primary-blue-visited: #9966ff;
    
    /* Secondary colors */
    --color-signpost: #3fa0d1;
    --color-panel-title: #c5d47a;
    --color-search: #4a9fd1;
    --color-success: #6b9d6b;
    --color-error: #cc6666;
    
    /* Backgrounds */
    --bg-page: #1a1a1a;
    --bg-surface: #222;
    --bg-footer: #2a2a2a;
    --bg-input: #222;
    
    /* Borders */
    --border-light: #333;
    --border-medium: #444;
    --border-dark: #555;
    --border-darker: #666;
    
    /* Text */
    --text-primary: #e8e8e8;
    --text-secondary: #b0b0b0;
    --text-tertiary: #808080;
    --text-muted: #999;
    
    /* Components */
    --button-bg: #333;
    --button-text: #ccc;
    --button-border: #555;
    --button-hover-bg: #e74c56;
    --button-hover-text: #fff;
    
    --nav-link: #b0b0b0;
    --nav-link-hover: #e74c56;
    
    --form-text: #e8e8e8;
    --form-placeholder: #808080;
    --form-disabled: #666;
    
    --input-border: #555;
    --input-focus-border: #66ccff;
  }
}
```

### Step 3: Refactor Existing CSS to Use Variables

**Before:**
```css
a {
  color: #02a;
}
a:visited {
  color: #42a;
}
```

**After:**
```css
a {
  color: var(--color-primary-blue);
}
a:visited {
  color: var(--color-primary-blue-visited);
}
```

---

## Manual Theme Toggle (Optional)

If users want manual override of OS preference:

```html
<!-- Toggle button in header -->
<button id="theme-toggle" aria-label="Toggle theme">
  <svg class="icon icon--sun">...</svg>
  <svg class="icon icon--moon">...</svg>
</button>
```

```javascript
// JavaScript to detect, store, and apply preference
const html = document.documentElement;
const preference = localStorage.getItem('theme') || 'auto';

function setTheme(theme) {
  if (theme === 'dark') {
    html.setAttribute('data-theme', 'dark');
  } else if (theme === 'light') {
    html.setAttribute('data-theme', 'light');
  } else {
    // 'auto' — remove attribute, use prefers-color-scheme
    html.removeAttribute('data-theme');
  }
  localStorage.setItem('theme', theme);
}

// CSS to support manual override
@media (prefers-color-scheme: light) {
  :root:not([data-theme="dark"]) {
    /* Already light in light mode */
  }
}

@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* Dark mode colors */
  }
}

[data-theme="dark"] {
  /* Force dark mode */
  color-scheme: dark;
  --bg-page: #1a1a1a;
  /* ... all dark tokens */
}

[data-theme="light"] {
  /* Force light mode */
  color-scheme: light;
  --bg-page: #f5f5f5;
  /* ... all light tokens */
}
```

---

## UIKit Integration with Dark Mode

### UIKit Components & Color Scheme

UIKit 3.x provides limited dark mode support:
- `uk-light` class — inverts text for light backgrounds
- `uk-dark` class — inverts text for dark backgrounds
- No built-in background color switching

**Solution:** Apply UIKit's inverse classes + CSS custom property overrides:

```html
<!-- Light mode (default) -->
<div class="uk-container">
  <!-- UIKit components, no class needed -->
</div>

<!-- Dark-specific overrides -->
@media (prefers-color-scheme: dark) {
  .uk-container {
    background: var(--bg-surface);
    color: var(--text-primary);
  }
  
  .uk-button {
    background: var(--button-bg);
    color: var(--button-text);
    border-color: var(--button-border);
  }
  
  .uk-input,
  .uk-select,
  .uk-textarea {
    background: var(--bg-input);
    color: var(--form-text);
    border-color: var(--input-border);
  }
}
```

### UIKit Inverse Classes (Selective Use)

Use `uk-light` or `uk-dark` for component-specific overrides only:

```html
<!-- Navbar (typically stays dark) -->
<nav class="uk-navbar-container uk-dark">
  <!-- UIKit will invert text to light -->
</nav>

<!-- Light content area -->
<div class="uk-container uk-light">
  <!-- Content here respects light text -->
</div>
```

---

## Breakpoint-Specific Dark Mode

Dark mode respects the same breakpoints as light mode. No special handling needed — CSS custom properties cascade to all breakpoints.

```css
/* Mobile dark mode */
@media (max-width: 767px) and (prefers-color-scheme: dark) {
  :root {
    --text-primary: #e8e8e8;
    /* same colors as desktop dark mode */
  }
}

/* Desktop dark mode */
@media (min-width: 1200px) and (prefers-color-scheme: dark) {
  :root {
    /* same colors, unchanged */
  }
}
```

---

## WCAG Accessibility Compliance

### Color Contrast Ratios (AA Standard: 4.5:1)

The table that was here claimed all pairs passed. They were not computed, and several failed (`#888` meta text 3.25:1 on `#f5f5f5`, `#808080` on `#1a1a1a` 4.41:1, white on `#e74c56` 3.77:1). The verified, computed palette and its ratios are in [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) §2. Use those values, and re-run a contrast check (script or WebAIM) whenever a token changes.

---

## Testing Dark Mode

### Browser Testing

1. **Manual OS Preference Test:**
   ```bash
   # Linux/Mac
   open http://localhost:8000  # Respects system dark mode
   
   # Windows 11
   # Settings > Personalization > Colors > Dark
   ```

2. **DevTools Emulation (Chrome/Firefox):**
   - Right-click → Inspect → three dots → Rendering
   - Find "Emulate CSS media feature prefers-color-scheme"
   - Toggle between "light" and "dark"

3. **Force Reload:**
   - Ctrl+Shift+R (or Cmd+Shift+R) to bypass cache

### Automated Testing

```css
/* Test dark mode in screenshots */
@media (prefers-color-scheme: dark) {
  /* All dark mode colors should be in effect */
  body {
    background: var(--bg-page);
  }
}
```

### Screenshot Comparison

The `plan/screenshots/` folder contains 6 reference images:
- Mobile (375px): light, dark
- Tablet (768px): light, dark
- Desktop (1280px): light, dark

Use Playwright to verify rebuild matches all 6 scenarios.

---

## Implementation Timeline

1. **Phase 1 (Foundation):** Create `variables.css` with all light/dark tokens (~4 hours)
2. **Phase 2 (Refactor):** Replace hardcoded colors in `dz.css` with `var(--color-*)` (~8 hours)
3. **Phase 3 (UIKit Integration):** Map UIKit component colors to custom properties (~6 hours)
4. **Phase 4 (Testing):** Validate dark mode across breakpoints and browsers (~4 hours)
5. **Phase 5 (Optional):** Add manual theme toggle UI (~3 hours)

**Total:** 25–29 hours for full dark mode implementation.

---

## References

- MDN: `prefers-color-scheme`: https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme
- UIKit Dark Mode (3.x): https://getuikit.com/docs/introduction (check for inverse classes)
- WCAG Color Contrast: https://www.tpgi.com/color-contrast-checker/
- Current CSS File: `http://localhost:8000/static/css/dz.css`
