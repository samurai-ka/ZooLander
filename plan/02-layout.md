# Page Layout & Viewport Strategy

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Reference:** http://localhost:8000/ (Demozoo site structure)  
**UIKit Grid System:** 12-column flex-based grid

---

## Viewport Definitions

All viewports use `viewport` meta tag: `<meta name="viewport" content="width=device-width, initial-scale=1">`

### Mobile (< 768px)

| Dimension | Value | Context |
|-----------|-------|---------|
| **Width** | 375px (reference) | iPhone 6/7/8, typical smartphone |
| **Height** | 667px | Portrait orientation |
| **Layout** | Single-column stacked | All elements full-width |
| **Navigation** | Hamburger menu (side-drawer or dropdown) | Header nav hidden, drawer icon visible |
| **Padding** | 16px sides | Reduced gutter for small screens |
| **Max-width** | 100% (no constraint) | Full viewport width minus padding |

**UIKit Breakpoint:** No specific breakpoint; uses mobile-first defaults

**Current Breakpoint Triggers:**
- 501px: Search bar width adjusts
- 601px: Further nav adjustments
- 767px: Last mobile breakpoint before tablet

### Tablet (768px – 1023px)

| Dimension | Value | Context |
|-----------|-------|---------|
| **Width** | 768px (reference) | iPad, standard tablet |
| **Height** | 1024px | Portrait orientation |
| **Layout** | 2-column or 2-up grid | Main content + sidebar or cards in pairs |
| **Navigation** | Full horizontal nav visible | Header expands, hamburger no longer needed |
| **Padding** | 24px sides | Standard gutter |
| **Max-width** | 100% | Still full viewport, but more breathing room |

**UIKit Breakpoint:** `@media (min-width: 768px)` — grid switches to multi-column

**Current Breakpoint Triggers:**
- 768px: Main media query breakpoint for Demozoo
- 1024px: Grid columns widen further (3-column layouts possible)

### Desktop (1024px – 1399px)

| Dimension | Value | Context |
|-----------|-------|---------|
| **Width** | 1280px (reference) | Desktop monitor, standard |
| **Height** | 720px | Landscape orientation |
| **Layout** | 3-column or full-width with sidebar | Maximum content density |
| **Navigation** | Full horizontal nav, user menu in top-right | All navigation visible |
| **Padding** | 4.5% horizontal (64px at 1400px max-width) | UIKit-style percentage-based gutter |
| **Max-width** | 1400px | Site constraint class |

**UIKit Breakpoint:** `@media (min-width: 960px)` and `@media (min-width: 1200px)` — full-width layouts

**Current Breakpoint Triggers:**
- 1024px: Full multi-column grid
- 1200px: Minor typography adjustments
- 1400px: Max-width constraint applied

---

## Core Layout Structure

All pages follow this HTML structure (adapted for UIKit):

```html
<html>
  <body>
    <div class="site">
      <!-- 1. HEADER -->
      <header class="site__header">
        <div class="logo">...</div>
        <nav>...</nav>
        <div class="search">...</div>
        <div class="user-menu">...</div>
      </header>

      <!-- 2. CONTENT (main area) -->
      <main class="site__content uk-container">
        <div class="page__header">...</div>
        <div class="page__body">
          <!-- Grid for multi-column layouts -->
          <div class="uk-grid">
            <div class="uk-width-1-3"><!-- Sidebar --></div>
            <div class="uk-width-2-3"><!-- Main content --></div>
          </div>
        </div>
      </main>

      <!-- 3. FOOTER -->
      <footer class="site__footer">
        <ul class="footer-links">...</ul>
      </footer>
    </div>
  </body>
</html>
```

---

## Header Layout

### Mobile (< 768px)

```
┌─────────────────────────────┐
│ [☰] Demozoo [👤 ▼]         │  Height: ~48px
└─────────────────────────────┘
┌─────────────────────────────┐
│ [Search input] [🔍]         │  Height: ~40px
└─────────────────────────────┘
```

**Components:**
- Logo: Text + icon, left-aligned
- Hamburger menu (☰): Triggers side-drawer or dropdown
- Search: Full-width below logo
- User menu: Top-right dropdown

### Tablet (768px–1023px)

```
┌────────────────────────────────────┐
│ [Logo] [Nav items] [Search] [👤▼] │  Height: ~64px
└────────────────────────────────────┘
```

**Components:**
- Logo: Left-aligned, visible
- Navigation: Horizontal menu items (Releases, Groups, Parties, BBSes, Forums)
- Search: Centered or right-aligned, inline
- User menu: Top-right

### Desktop (1024px+)

```
┌─────────────────────────────────────────────────┐
│ [Logo] [────── Nav items ──────] [User menu ▼] │  Height: ~72px
│                              [Search bar]       │
└─────────────────────────────────────────────────┘
```

**Components:** Same as tablet, but with more breathing room and larger touch targets.

---

## Content Area Layouts

### Single Column (Default for Mobile)

**Mobile Full-Width:**
```
┌─────────────────────────┐
│ Padding: 16px           │
│ ┌─────────────────────┐ │
│ │ Content area 100%   │ │
│ │ (no grid)           │ │
│ └─────────────────────┘ │
└─────────────────────────┘
```

### Two-Column (Tablet & Desktop)

**Sidebar + Content:**
```
┌────────────────────────────────────┐
│ Padding: 24px (tablet) or 4.5% (desktop)
│ ┌──────────┐ ┌──────────────────┐ │
│ │Sidebar   │ │Main content      │ │
│ │30–33%    │ │67–70%            │ │
│ └──────────┘ └──────────────────┘ │
└────────────────────────────────────┘
```

**UIKit Classes:**
```html
<div class="uk-container">
  <div class="uk-grid uk-grid-divider">
    <div class="uk-width-1-4@m"><!-- Sidebar --></div>
    <div class="uk-width-3-4@m"><!-- Content --></div>
  </div>
</div>
```

### Three-Column (Desktop Only)

**Sidebar + Content + Right Panel:**
```
┌────────────────────────────────────────────┐
│ ┌────────┐ ┌──────────────┐ ┌────────────┐ │
│ │Left    │ │Main content  │ │Right panel │ │
│ │Sidebar │ │              │ │Related     │ │
│ │33%     │ │34%           │ │33%         │ │
│ └────────┘ └──────────────┘ └────────────┘ │
└────────────────────────────────────────────┘
```

**UIKit Classes:**
```html
<div class="uk-grid">
  <div class="uk-width-1-3@l"><!-- Left --></div>
  <div class="uk-width-1-3@l"><!-- Content --></div>
  <div class="uk-width-1-3@l"><!-- Right --></div>
</div>
```

---

## Footer Layout

**Responsive:** Single-line list of links that stacks on mobile.

```
Light Mode (Mobile):
┌─────────────────────────┐
│ • Link1                 │
│ • Link2                 │
│ • Link3                 │
└─────────────────────────┘

Light Mode (Tablet+):
┌────────────────────────────────────┐
│ • Link1 | Link2 | Link3 | Link4   │
└────────────────────────────────────┘
```

**Current Structure:**
- Background: `#e8e8e8` (light gray)
- Border: 10px white border on top
- Padding: Responsive (16px mobile, 20px tablet+)
- Text alignment: Center
- Font size: 0.8em, reduced from body

---

## Grid System Details

### Mobile (No Grid — Single Column)

All content flows vertically, 100% width minus padding.

### Tablet & Desktop (UIKit Grid)

**UIKit 12-column Grid:**
- Mobile-first: No grid class for mobile
- Tablet up (`@m`): 2-column layouts
- Large tablet up (`@l`): 3-column layouts
- Desktop up (`@xl`): Full-width optimization

**Common Ratios:**
- `uk-width-1-2@m` — 50% width on tablet+
- `uk-width-1-3@m` — 33% width on tablet+
- `uk-width-2-3@m` — 67% width on tablet+
- `uk-width-1-4@l` — 25% width on large desktop+

---

## Spacing & Gutter

### Horizontal Padding (Body Margin)

| Viewport | Padding | Total Width | Content Width |
|----------|---------|-------------|---------------|
| Mobile | 16px | 375px | 343px |
| Tablet | 24px | 768px | 720px |
| Desktop | 4.5% | 1400px max | 1337px |

**UIKit Gutter (Grid Gaps):**
- Default: 20px (10px each side)
- Can be adjusted with `uk-grid-small` (10px) or `uk-grid-large` (40px)

### Vertical Spacing

| Component | Spacing | Notes |
|-----------|---------|-------|
| **Sections** | 20–30px | Between major content blocks |
| **Cards** | 15px | Between cards in a grid |
| **Form fields** | 12px | Between form inputs |
| **Lists** | 8px | Between list items |
| **Headings** | 16px above, 8px below | Typography spacing |

---

## Specific Page Layouts

### Homepage

**Mobile:**
- Hero banner (full-width)
- Latest activity sections (single column, stacked)

**Tablet:**
- Hero banner (full-width)
- Latest activity: 2 columns

**Desktop:**
- Hero banner (full-width)
- Latest activity: 2 columns side-by-side
- Sidebar with stats/quick links

### Production/Party Page

**Mobile:**
- Full-width content
- Banner (responsive image)
- Metadata (title, artist, date)
- Screenshots (single-column carousel)
- Download links (full-width list)

**Tablet:**
- 2-column: sidebar (metadata) + main content
- Screenshots in 2-column grid
- Downloads: side-by-side buttons

**Desktop:**
- 3-column: narrow sidebar + main + related
- Screenshots in 3-column grid or carousel
- Expanded metadata panel

### Search Results

**All Viewports:**
- List of results with:
  - Thumbnail (small square)
  - Title
  - Metadata (artist, date, type)
  - Snippet (if available)

**Responsive Grid:**
- Mobile: 1 column, thumbnail on left
- Tablet: 1 column, larger thumbnails
- Desktop: Could expand to 2-column if needed

---

## Responsive Media & Images

### Images

- **Thumbnails:** Fixed size (48–96px) with `object-fit: cover` for consistent aspect ratio
- **Banners:** 100% width, constrained height (40% padding-bottom for aspect ratio)
- **Screenshots:** Max-width 100%, height auto
- **Lazy Loading:** Consider `loading="lazy"` on off-screen images

### Videos & Embeds

- **Responsive Embed:** Wrap in container with `position: relative`, embed has `position: absolute` + `top/left/right/bottom: 0`
- **Max-width:** Respect container constraints, typically 100%

---

## Accessibility & Mobile Considerations

### Touch Targets (Mobile)

- **Minimum size:** 44px × 44px
- **Spacing:** At least 8px between adjacent targets
- **Nav links:** 25–32px tall (current meets this)
- **Buttons:** 32px minimum height

### Text Sizing & Readability

- **Mobile:** Base 12px, scales up to 14px at 768px
- **Line-height:** 1.5 for body text (plenty of breathing room)
- **Line-length:** Constrained by viewport width; typically 60–80 characters max on desktop

### Orientation

- Portrait: Primary focus (375px wide)
- Landscape: Tablet-like behavior (vertical space reduced)

---

## Testing Layout Across Breakpoints

### Browser DevTools

1. **Chrome:** Device Toolbar (Ctrl+Shift+M or Cmd+Shift+M)
2. **Firefox:** Responsive Design Mode (Ctrl+Shift+M or Cmd+Option+M)
3. **Set custom dimensions:** 375 × 667 (mobile), 768 × 1024 (tablet), 1280 × 720 (desktop)

### Reference Screenshots

See `plan/screenshots/` folder:
- `mobile-light.png`, `mobile-dark.png`
- `tablet-light.png`, `tablet-dark.png`
- `desktop-light.png`, `desktop-dark.png`

Compare UIKit rebuild screenshots against these references at each breakpoint and color mode.

---

## UIKit Migration Notes

1. **Replace custom grid with UIKit Grid:** Use `uk-grid`, `uk-width-*@breakpoint` classes
2. **Use UIKit breakpoints:** `@s` (640px), `@m` (960px), `@l` (1200px), `@xl` (1600px)
3. **Leverage UIKit flex utilities:** Replace custom float/flex rules with UIKit classes
4. **Maintain existing spacing:** UIKit gutter (20px default) matches Demozoo card spacing
5. **Responsive images:** UIKit has no special image handling; use standard HTML + CSS

---

## References

- UIKit Grid Documentation: https://getuikit.com/docs/grid
- UIKit Breakpoints: https://getuikit.com/docs/introduction#breakpoints
- Current Layout CSS: `http://localhost:8000/static/css/dz.css` (search for `.site`, `.container`, `@media`)
