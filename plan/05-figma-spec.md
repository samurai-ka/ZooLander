# Figma Design Specification — UIKit Component Library

> **Correction notice (2026-10-14):** colour contrast claims, dark-mode values and UIKit breakpoints in this document were not fully verified. See [09-figma-file-and-corrections.md](09-figma-file-and-corrections.md) — it takes precedence.

**Last Updated:** 2026-10-14  
**Status:** Built — see https://www.figma.com/design/kfGHazLuR6aPKTMw0lIYjn and 09-figma-file-and-corrections.md  
**Target:** Demozoo UIKit Component Library + Design System

---

## Overview

This document specifies the Figma design file structure for the UIKit-based Demozoo redesign. Once the Figma MCP is authorized in claude.ai connector settings, this specification will be automatically converted into a live Figma file.

---

## File Structure & Pages

### **Page 1: Design System Tokens**

**Purpose:** Store all design tokens as Figma styles and color swatches

#### Colors Section
Create color styles for all 40+ tokens:

| Style Name | Light Value | Dark Value | Category |
|-----------|-------------|-----------|----------|
| `Colors/Primary/Red` | #c32e3b | #e74c56 | Primary accent |
| `Colors/Primary/Red-Light` | #ec5a64 | #f47f88 | Primary hover |
| `Colors/Primary/Blue` | #02a | #66ccff | Links |
| `Colors/Primary/Blue-Visited` | #42a | #9966ff | Visited links |
| `Colors/Secondary/Blue` | #6bc7f1 | #3fa0d1 | Signpost, badges |
| `Colors/Secondary/Yellow` | #a5b45d | #c5d47a | Panel titles |
| `Colors/Background/Page` | #f5f5f5 | #1a1a1a | Page background |
| `Colors/Background/Surface` | #fff | #222 | Cards, containers |
| `Colors/Background/Footer` | #e8e8e8 | #2a2a2a | Footer |
| `Colors/Text/Primary` | #333 | #e8e8e8 | Body text |
| `Colors/Text/Secondary` | #444 | #b0b0b0 | Secondary text |
| `Colors/Text/Tertiary` | #888 | #808080 | Muted text |
| `Colors/Border/Light` | #eee | #333 | Light borders |
| `Colors/Border/Medium` | #ddd | #444 | Medium borders |
| `Colors/Border/Dark` | #ccc | #555 | Dark borders |
| `Colors/Status/Success` | #bfb | #6b9d6b | Success messages |
| `Colors/Status/Error` | #fbb | #cc6666 | Error messages |
| `Colors/Status/Warning` | #ffd700 | #ffb700 | Warning messages |

#### Typography Section
Create text styles for each font size & weight:

| Style Name | Font | Size | Weight | Line Height | Usage |
|-----------|------|------|--------|-------------|-------|
| `Typography/Display/H1` | FabricaRegular | 26pt | Normal | 1.2 | Page titles |
| `Typography/Display/H2` | FabricaRegular | 20pt | Normal | 1.2 | Section headings |
| `Typography/Display/H3` | FabricaRegular | 14pt | Normal | 1.3 | Subsection headings |
| `Typography/Body/Large` | System | 1rem (14px) | 400 | 1.5 | Lead text |
| `Typography/Body/Regular` | System | 1rem (14px) | 400 | 1.5 | Body text |
| `Typography/Body/Small` | System | 0.9rem | 400 | 1.4 | Secondary text |
| `Typography/Meta` | System | 0.8rem | 400 | 1.4 | Metadata, captions |
| `Typography/Code` | DejaVu Mono | 0.9rem | 400 | 1.5 | Code blocks |
| `Typography/Button` | FabricaRegular | 16px | 500 | 1 | Button text (uppercase) |

#### Spacing Section
Create spacing variables:

| Style Name | Value | Usage |
|-----------|-------|-------|
| `Spacing/XS` | 2px | Fine adjustments |
| `Spacing/SM` | 4px | Small gaps |
| `Spacing/MD` | 8px | Standard spacing |
| `Spacing/LG` | 16px | Component padding |
| `Spacing/XL` | 24px | Section margins |
| `Spacing/2XL` | 32px | Large margins |

#### Shadows Section
Create shadow styles:

| Style Name | Definition | Usage |
|-----------|-----------|-------|
| `Shadows/Subtle` | 0 1px 2px rgba(0,0,0,0.1) | Hover states |
| `Shadows/Inset-Subtle` | inset 0 0 1px rgba(0,0,0,0.25) | Image borders |
| `Shadows/None` | None | Default state |

---

### **Page 2: Components — Navigation**

**Purpose:** Navigation and header components with variants

#### Navbar Component
Main navigation container with variants:

**Variants:**
- `Navbar/Light` — Light mode (white bg, dark text)
- `Navbar/Dark` — Dark mode (#1a1a1a bg, light text)

**Sub-components:**
- `Navbar/Logo` — Demozoo wordmark (253×52px, red background)
- `Navbar/Nav-Item` — Menu link
  - States: default, hover (top border #ec5a64), active (background #ec5a64, white text)
- `Navbar/Search-Bar` — Global search
  - Input field (#77c6ef or dark equivalent)
  - Category dropdown
  - Submit button (dark background, search icon)
- `Navbar/User-Menu` — Top-right profile/login menu
  - Closed state (login button or profile icon)
  - Open state (dropdown with login/profile/logout)

**Responsive Variants:**
- Mobile (< 768px): Collapsed nav, hamburger icon, full-width search
- Tablet (768px+): Expanded nav, inline search

---

### **Page 3: Components — Buttons & Forms**

**Purpose:** Button system, form controls, and input patterns

#### Button Variants

**Primary Button**
- States: default, hover, active, disabled, loading
- Sizes: default, small, large
- Modifiers: with icon, icon-only, full-width
- Colors: default (#c32e3b hover), secondary (blue), ghost (outline)

**Action Button**
- Small icon buttons (add, edit, delete, etc.)
- Background: #eee → #ccc on hover
- Supports PNG icon sprites

**Icon Button**
- SVG icons (20×20px)
- No background
- Hover: opacity change, slight fill color change

#### Form Controls

**Input Fields**
- Text input (`<input type="text">`)
- Search input (`<input type="search">`)
- Number input (`<input type="number">`)
- Date input (`<input type="date">`)
- Email input (`<input type="email">`)
- URL input (`<input type="url">`)
- Password input (`<input type="password">`)
- States: default, focus (border color: #77c6ef), disabled, error
- Sizes: small, regular, large

**Textarea**
- Regular textarea (420px × 300px, responsive)
- Short notes variant (50px height)
- States: default, focus, disabled, error

**Select Dropdown**
- Multi-option select
- States: default, focus, disabled
- Sizes: regular, responsive
- Options: single select, multiple select

**Checkbox & Radio**
- Checkbox (label + input)
- Radio button (label + input)
- Grouped checkboxes
- Grouped radio buttons
- States: unchecked, checked, indeterminate, disabled

**Button Group**
- Horizontal button arrangement
- Multiple buttons (e.g., filter buttons, action buttons)
- Active state highlighting

**Help Text & Labels**
- Form label (`<label>`)
- Required indicator (asterisk)
- Help text (small, gray)
- Error message (red text)
- Hover-help icon with tooltip

---

### **Page 4: Components — Cards & Panels**

**Purpose:** Container components with headers, footers, and content areas

#### Card Component

**Variants:**
- `Card/Default` — Standard card with shadow
- `Card/Bordered` — Card with border only
- `Card/Flat` — No shadow or border

**Sections:**
- `Card-Header` — Title bar with optional background color
- `Card-Body` — Main content area
- `Card-Footer` — Bottom section (often right-aligned actions)
- `Card-Media` — Image/media section

**Sizes:**
- Small (compact spacing)
- Medium (default)
- Large (expanded padding)

#### Panel Component

**Variants:**
- `Panel/Red-Header` — Red title bar (primary color)
- `Panel/Yellow-Header` — Yellow title bar (tertiary color)
- `Panel/Simple` — No colored header

**Elements:**
- Title area (colored background, white text, FabricaRegular font)
- Content area (white background, light padding)
- Footer (right-aligned text/buttons)

#### Block Component

**Variants:**
- `Block/Standard` — Content block with border-top separator
- `Block/Colored-Header` — Overlapping colored header

---

### **Page 5: Components — Data Display**

**Purpose:** Tables, lists, grids for displaying data

#### Table Component

**Variants:**
- `Table/Default` — Basic table styling
- `Table/Striped` — Alternating row backgrounds (#f8f8f8)
- `Table/Bordered` — Visible cell borders
- `Table/Hover` — Row highlight on hover
- `Table/Compact` — Reduced padding

**Sub-components:**
- Table header (dark background, white text)
- Table row (standard height 36px)
- Table cell (left-aligned content)
- Thumbnail cell (48×48px, object-fit: cover)
- Metadata cell (secondary text color)

**Sorting Indicators:**
- Sort ascending icon
- Sort descending icon
- Sortable column hover state

#### List Component

**Variants:**
- `List/Default` — Unstyled list
- `List/Divider` — Borders between items
- `List/Striped` — Alternating backgrounds
- `List/Hover` — Highlight on hover

#### Grid (Gallery)

**Responsive Grid Layouts:**
- Mobile: 1 column
- Tablet: 2 columns
- Desktop: 3–4 columns
- Configurable gap (8px, 16px, 24px)

**Grid Item (Card):**
- Image placeholder (responsive aspect ratio)
- Title overlay
- Metadata
- Hover state (slight zoom, shadow)

---

### **Page 6: Components — Modals & Overlays**

**Purpose:** Modal dialogs, lightboxes, and overlay patterns

#### Modal Dialog

**Variants:**
- `Modal/Default` — Standard modal with gray background
- `Modal/Fullscreen` — Full-screen modal (mobile optimization)
- `Modal/Compact` — Small modal (confirmation dialogs)

**Sections:**
- Modal header (title, close button)
- Modal body (content area)
- Modal footer (action buttons, right-aligned)

**Sizes:**
- Small (400px)
- Medium (620px)
- Large (800px)
- Fullscreen

#### Lightbox

**Variants:**
- `Lightbox/Image` — Image viewer
- `Lightbox/Gallery` — Image carousel with prev/next
- `Lightbox/Content` — Generic content display

**Elements:**
- Background overlay (dark, semi-transparent)
- Content container (white background)
- Close button (top-right, SVG icon)
- Navigation arrows (prev/next for galleries)
- Zoom controls (optional)

#### Dropdown / Popover

**Variants:**
- `Dropdown/Menu` — Menu items list
- `Dropdown/Info` — Information panel
- `Dropdown/Form` — Form fields in dropdown

**States:** closed, open, hover

---

### **Page 7: Components — Typography & Text**

**Purpose:** Text utilities and typography patterns

#### Heading Styles
- H1, H2, H3, H4, H5, H6
- Each with FabricaRegular font, specific sizes, and margins

#### Text Utilities
- `.uk-text-lead` — Larger intro text
- `.uk-text-large` — Larger text
- `.uk-text-small` — Smaller text
- `.uk-text-meta` — Metadata/secondary text
- `.uk-text-muted` — Muted/disabled text
- `.uk-text-truncate` — Single-line ellipsis
- `.uk-text-uppercase` — Uppercase transform
- `.uk-text-center`, `.uk-text-left`, `.uk-text-right` — Alignment

#### Link States
- Default link (#02a)
- Hover link (underline, color)
- Visited link (#42a)
- Active link (darker)

#### Code & Pre
- Inline code (background, border, monospace font)
- Code block (background, border, 1rem padding, scroll)

---

### **Page 8: Components — Icons**

**Purpose:** Icon system and SVG icon library

#### Icon Sizes
- Icon/XS — 16px (inline)
- Icon/SM — 20px (buttons)
- Icon/MD — 24px (standard)
- Icon/LG — 32px (prominent)
- Icon/XL — 48px (hero)

#### Icon Variants
- Monochrome (fill color from CSS variable)
- Colored (brand red, secondary blue, etc.)
- Outlined (stroke-based)
- Filled (solid)

#### Common Icons (UIKit + Custom)
- Navigation: menu, chevron-up, chevron-down, arrow-left, arrow-right, close, menu-dots
- Actions: edit, delete, add, save, cancel, download, upload, search
- Status: check, warning, error, info, question
- Media: play, pause, volume, camera, image
- Social: share, link, download, print
- Demozoo-specific: disk, palette, music, scener, group, party, bbs

---

### **Page 9: Layout Templates**

**Purpose:** Full-page layout templates for different viewport sizes

#### Mobile Layout (375px)
- Single-column layout
- Full-width content (16px padding)
- Stacked components
- Header: hamburger + logo + search (stacked)
- Footer: single-column list

#### Tablet Layout (768px)
- Two-column layouts possible
- Sidebar + content (1/3 + 2/3)
- Header: full nav visible
- Grid: 2-column cards
- Footer: inline list

#### Desktop Layout (1280px+)
- Three-column layouts (1/3 + 1/3 + 1/3)
- Sidebar + content + related
- Full header with inline search
- Grid: 3–4 column cards
- Max-width: 1400px container
- Footer: full-width inline list

---

### **Page 10: Light Mode Mockups**

**Purpose:** Full-page mockups in light mode across viewports

#### Homepage Light
- Hero banner (responsive image, 40% padding-bottom)
- Latest activity sections (2-column grid on tablet+)
- Featured content cards
- Footer

#### Production Page Light
- Header with banner
- Metadata sidebar (33% width on desktop)
- Main content (67% width on desktop)
- Screenshots carousel (3-column grid on desktop)
- Download links
- Comments section
- Related productions (3-column)

#### Search Results Light
- Query input bar
- Result items (list format)
- Pagination controls

#### Forum Light
- Thread list (table format)
- Pagination

---

### **Page 11: Dark Mode Mockups**

**Purpose:** Same templates as Page 10, but in dark mode

#### Homepage Dark
- Same layout as light mode
- All colors overridden to dark mode palette
- Text: #e8e8e8 on #1a1a1a
- Borders: #333 instead of #eee
- Accents: lightened (#e74c56, #66ccff)

#### Production Page Dark
- Same structure as light
- Dark mode color overrides applied

#### Search Results Dark
- Dark mode styling
- Maintained contrast ratios

#### Forum Dark
- Dark mode styling

---

### **Page 12: Component States & Interactions**

**Purpose:** Document interactive states and animations

#### Button States
- Resting: #f5f5f5 background, #888 text
- Hover: #c32e3b background, #fff text, 0.2s ease transition
- Active/Pressed: darker background, inset shadow effect
- Disabled: #ccc background, #999 text, cursor: not-allowed
- Loading: animated spinner overlay

#### Form Input States
- Default: white background, #ddd border
- Focus: white background, #77c6ef border, outline: none
- Error: pink background, red border, error message displayed
- Disabled: #f5f5f5 background, #ccc text
- Filled: as default, but with value visible

#### Link States
- Default: #02a color, no underline
- Hover: #02a color, underline, cursor: pointer
- Visited: #42a color
- Active: darker, possibly different background
- Focus: underline, outline for accessibility

#### Card/Panel Hover
- Slight shadow increase
- Background color slightly lighter/darker
- No transform (avoid layout shift)
- 0.2s ease transition

#### Modal/Overlay
- Fade in (opacity: 0 → 1, 0.2s)
- Overlay background (semi-transparent black)
- Escape key closes (keyboard interaction)
- Focus trap (tab navigation within modal)

#### Notification States
- Success: green background (#bfb or #6b9d6b), white text
- Error: red background (#fbb or #cc6666), white text
- Warning: yellow background, dark text
- Info: blue background (#77c6ef or #4a9fd1), white text
- Auto-dismiss after 5 seconds (optional)

---

## Accessibility Features in Design

### Color Contrast
- All text on background: WCAG AA 4.5:1 minimum
- Large text (18pt+): WCAG AA 3:1 minimum
- All contrasts validated for both light and dark modes

### Interactive Elements
- Minimum touch target size: 44×44px
- Minimum spacing between targets: 8px
- Focus states visible (underline, outline, background change)
- Hover states in addition to focus states

### Semantic HTML
- Proper heading hierarchy (H1 → H2 → H3)
- Form labels associated with inputs (`<label for="id">`)
- ARIA attributes where needed (role, aria-label, aria-expanded)
- Keyboard navigation support (Tab order, Enter/Space to activate)

### Motion & Animation
- Transitions: 0.2s ease (subtle, not distracting)
- No auto-playing animations
- Respect `prefers-reduced-motion` media query
- No flashing content (> 3 flashes/sec)

---

## Export & Implementation

### Component Code
Each component in Figma should include:
- **Figma Documentation:** Component description and usage guidelines
- **Dev Handoff:** Responsive breakdown, color tokens used, spacing values
- **CSS Classes:** Corresponding UIKit classes or custom classes
- **States:** All interactive states documented

### Responsive Variants
- Figma components auto-layout for responsive behavior
- Breakpoint notes for developer reference (768px, 1024px, 1280px)
- Mobile/tablet/desktop variants clearly labeled

### Design Tokens
- All colors link to color styles (updatable globally)
- Typography uses text styles (maintainable)
- Spacing uses consistent units (8px base)
- Shadows use shadow styles

### Dark Mode Implementation
- Each component has light/dark variant
- Tokens automatically swap in dark mode variant
- Visual contrast verified for both modes

---

## Integration with UIKit

### Component Mapping
| Figma Component | UIKit Component | Notes |
|-----------------|-----------------|-------|
| Card | `uk-card` | With header/body/footer sections |
| Button | `uk-button` | Primary, secondary, ghost variants |
| Form Input | `uk-input` | With focus/error states |
| Select | `uk-select` | With dropdown styling |
| Navbar | `uk-navbar` | With logo, nav items, search, user menu |
| Modal | `uk-modal` | With close button and sections |
| Grid | `uk-grid` | With responsive column widths |
| Table | `uk-table` | With striped/hover variants |
| Dropdown | `uk-drop` / `uk-navbar-dropdown` | For menus and popovers |
| Icon | `uk-icon` | SVG-based, colored via CSS |

---

## File Sharing & Collaboration

### Access Control
- **Viewers:** Design team, product managers (view-only)
- **Editors:** Lead designer, developers (can modify, comment)
- **Admin:** Project lead (full control)

### Commenting Protocol
- Tag issues with @mention when requesting changes
- Developers link to Figma pages in GitHub PRs
- Version history tracked (revert if needed)

### Handoff Process
1. Designer finalizes components
2. Developer extracts CSS/spacing values from Figma
3. Developer implements in HTML/UIKit
4. QA compares built version against Figma mockups
5. Figma updated with final implementation details

---

## Maintenance & Updates

### Component Updates
- Changes to components propagated to all instances
- Variant updates reflected across designs
- Color updates across all light/dark modes simultaneously

### Breakpoint Changes
- If breakpoint changes (e.g., 768px → 740px), update all templates
- Grid column counts updated if needed
- Responsive variants re-verified

### Token Updates
- New colors added to color styles page
- Typography scale adjusted if sizing changes
- Spacing scale modified if base unit changes

---

## File Readiness Checklist

- [ ] All 40+ design tokens created as styles
- [ ] 20+ UIKit components designed with variants
- [ ] Light & dark mode mockups complete
- [ ] All interactive states documented
- [ ] Responsive layouts for 3 viewports
- [ ] Accessibility contrast validated
- [ ] Component documentation written
- [ ] Figma libraries published
- [ ] Shared with team
- [ ] Version history initialized

---

## Next Steps (Once Figma MCP Authorized)

1. **Create Figma file** from this specification
2. **Invite team members** (designers, developers, stakeholders)
3. **Share design system** for team reference
4. **Version control** (Figma versioning + GitHub sync)
5. **Implement components** in HTML/CSS/UIKit
6. **QA against mockups** (visual regression testing)
7. **Update Figma** with final implementation details

---

## References

- UIKit Components: https://getuikit.com/docs/components
- Demozoo Issues: [plan/06-issues-uikit.md](plan/06-issues-uikit.md)
- Design Tokens: [plan/01-tokens.md](plan/01-tokens.md)
- Component Mapping: [plan/03-component-mapping.md](plan/03-component-mapping.md)
- Theming Strategy: [plan/04-theming.md](plan/04-theming.md)
