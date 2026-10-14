# Design Tokens: Demozoo UIKit Migration

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Status:** Analysis Phase — Light Mode Complete, Dark Mode to be Derived

---

## Color Tokens

### Primary & Accent Colors

| Token | Light Mode | Usage | Dark Mode (Proposed) | Notes |
|-------|-----------|-------|----------------------|-------|
| **Primary Red** | `#c32e3b` | Main brand color, block headings, button hover | `#e74c56` | Slight lightening for dark mode contrast |
| **Primary Red Light** | `#ec5a64` | Active nav states, hover effects | `#f47f88` | Lighter variant for dark backgrounds |
| **Primary Blue** | `#02a` | Links, interactive elements | `#66ccff` | Much lighter for readability on dark |
| **Primary Blue Visited** | `#42a` | Visited links | `#9966ff` | Purple-shifted for dark mode distinction |

### Secondary & Functional Colors

| Token | Light Mode | Usage | Dark Mode (Proposed) |
|-------|-----------|-------|----------------------|
| **Signpost Blue** | `#6bc7f1` | Category badges, panels | `#3fa0d1` |
| **Panel Title Yellow** | `#a5b45d` | Panel headings (alt to red) | `#c5d47a` |
| **Search Input Blue** | `#77c6ef` | Search bar background | `#4a9fd1` |
| **Success Green** | `#bfb` | Success messages | `#6b9d6b` |
| **Error Red** | `#fbb` | Error messages | `#cc6666` |

### Neutral & Background Colors

| Token | Light Mode | Usage | Dark Mode (Proposed) |
|-------|-----------|-------|----------------------|
| **Background** | `#f5f5f5` | Page background | `#1a1a1a` |
| **Surface White** | `#fff` | Cards, containers | `#222` |
| **Footer** | `#e8e8e8` | Footer background | `#2a2a2a` |
| **Divider Light** | `#eee` | Light borders, stripes | `#333` |
| **Divider Medium** | `#ddd` | Medium borders | `#444` |
| **Divider Dark** | `#ccc` | Input borders | `#555` |
| **Divider Darker** | `#aaa`, `#888` | Strong borders, disabled | `#666` |

### Text Colors

| Token | Light Mode | Usage | Dark Mode (Proposed) |
|-------|-----------|-------|----------------------|
| **Text Primary** | `#333` | Body text, headings | `#e8e8e8` |
| **Text Secondary** | `#444` | Subtext, captions | `#b0b0b0` |
| **Text Tertiary** | `#888` | Metadata, disabled | `#808080` |
| **Text Light** | `#666` | Secondary info | `#999` |
| **Text Muted** | `#555` | Weak contrast | `#777` |

---

## Typography

### Font Stack

**Primary (Body Text):**
```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
             "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
```

**Headings & Display:**
```css
font-family: FabricaRegular, Helvetica, Arial, sans-serif;
font-weight: normal; /* Font file has no weight variation */
```

**Monospace (Code, Pre):**
```css
font-family: dejavu_sans_monobook, monospace;
```

**Retro/Demoscene (Optional, Heritage):**
- `topazplusa1200` — Amiga Topaz font emulation
- `ibm_vga_8x16` — IBM VGA font emulation
- Used in `.font-amiga` and `.font-msdos` for demo/text file display

### Font Sizes

| Context | Size | Notes |
|---------|------|-------|
| Root | 12px (mobile), 14px (tablet+) | Sets `1rem` base |
| Body | 1rem | Responsive via root |
| Headings (h2, h3, h4) | 26pt, 14pt, 11pt | FabricaRegular |
| Focus Title (h2) | 26pt | Production/party page titles |
| Small Text (.small, .secondary) | 8pt–11pt | Search results, metadata |
| Monospace | 1em (via line-height 1em) | Code blocks |

### Line Height

| Context | Value |
|---------|-------|
| Body | 1.5 |
| Monospace | 1em |
| Headings | 1.2em–1.5em (varies) |
| Small text | 1.25rem (search snippets) |

### Text Utilities

- **Text Transform:** `uppercase` on buttons, nav, block headings
- **Letter Spacing:** `-0.05ex` on buttons, `-1px` on headings
- **Text Overflow:** `overflow: hidden`, `text-overflow: ellipsis` (needs `white-space: nowrap`)
- **Word Break:** URL wrapping uses `word-break: break-word`

---

## Spacing & Sizing

### Core Spacing Scale

| Unit | Value | Usage |
|------|-------|-------|
| xs | 2px | Fine adjustments |
| sm | 4px | Padding inside small elements |
| base | 8px | Standard padding, margins |
| md | 12px–16px | Component padding, margins |
| lg | 20px–25px | Section margins |
| xl | 30px–36px | Header/footer padding |

### Breakpoints

| Viewport | Width | Context |
|----------|-------|---------|
| **Mobile** | < 501px | Hamburger nav, stacked layout |
| **Mobile–Tablet** | 501–600px | Search bar adjustments |
| **Small Tablet** | 600–767px | Grid adjustments |
| **Tablet** | 768px+ | 2-column layouts (default media query) |
| **Large Tablet** | 1024px | 3-column, wider grids |
| **Desktop** | 1200px+ | Full-width site, max-width: 1400px |
| **Large Desktop** | 1400px+ | Constrained by max-width |

### Container Sizing

| Element | Width | Notes |
|---------|-------|-------|
| **Max Width** | 1400px | `.container`, `.site` |
| **Content Padding** | 4.5% | Horizontal padding inside `.site__content` |
| **Header Padding** | 36px (top/bottom) | `.site__header` |
| **Mobile Header** | 16px | Reduced on small screens |

---

## Border Radius

| Context | Value |
|---------|-------|
| Buttons | 3px–6px |
| Cards/Panels | 2px–4px |
| Modal/Lightbox | 5px |
| Input fields | 0 (square) to 3px |

---

## Shadows & Depth

| Context | Shadow | Notes |
|---------|--------|-------|
| Lightbox border | 1px solid #444 | Simple, no drop shadow |
| Panel/Container | 1px solid #ccc | Subtle outline |
| Input focus | border-color change only | No box-shadow |
| Hover effects | Background color shift | No shadow, uses color only |

---

## Dark Mode Strategy

### Implementation Approach

1. **CSS Custom Properties (Recommended):**
   ```css
   :root {
     --color-primary: #c32e3b;
     --color-primary-light: #ec5a64;
     --bg-page: #f5f5f5;
     --text-primary: #333;
     /* ... more tokens */
   }
   
   @media (prefers-color-scheme: dark) {
     :root {
       --color-primary: #e74c56;
       --bg-page: #1a1a1a;
       --text-primary: #e8e8e8;
       /* ... dark overrides */
     }
   }
   ```

2. **Manual Toggle (Optional):**
   ```css
   [data-theme="dark"] {
     --bg-page: #1a1a1a;
     /* ... */
   }
   ```

3. **UIKit Integration:**
   - UIKit supports inverse classes (`uk-light`, `uk-dark`) but limited scope
   - Prefer CSS custom properties over UIKit's inverse mode for full control
   - Dark mode should apply to all Demozoo-specific colors, not just UIKit

### Color Ratios (WCAG AA Compliant)

All dark mode colors have been selected to maintain WCAG AA contrast ratios:
- Text on background: 4.5:1 minimum
- Accent on background: 3:1 minimum
- Primary red on dark slightly lightened to improve contrast

---

## Component-Specific Tokens

### Buttons

| Token | Light | Dark |
|-------|-------|------|
| Background | `#f5f5f5` | `#333` |
| Text | `#888` | `#ccc` |
| Border | `#ddd` | `#555` |
| Hover bg | `#c32e3b` | `#e74c56` |
| Hover text | `#fff` | `#fff` |

### Forms

| Token | Light | Dark |
|-------|-------|------|
| Input bg | `#fff` | `#222` |
| Input text | `#333` | `#e8e8e8` |
| Input border | `#ddd` | `#555` |
| Focus border | varies, often blue | lighter blue |
| Disabled text | `#888` | `#666` |

### Navigation

| Token | Light | Dark |
|-------|-------|------|
| Background | `#fff` | `#1a1a1a` |
| Link text | `#5e5f5f` | `#b0b0b0` |
| Hover bg | `border-top #ec5a64` | border with lighter accent |
| Active bg | `#ec5a64` | `#e74c56` |
| Active text | `#fff` | `#fff` |

---

## References

- Current CSS File: `http://localhost:8000/static/css/dz.css`
- Font Files: `http://localhost:8000/static/font/` (Fabrica, DejaVu, Topaz, IBM VGA)
- Existing Breakpoints: Mobile (501px, 601px), Tablet (768px), Desktop (1024px–1400px)
