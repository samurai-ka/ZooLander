# Component Mapping: Demozoo → UIKit

**Last Updated:** 2026-10-14  
**Scope:** Mapping existing Demozoo UI elements to UIKit components  
**Reference:** http://localhost:8000/ (live site), UIKit 3.x docs

---

## Navigation & Header

### Logo

**Current:**
- Image-based logo (`/static/images/demozoo_title.png`)
- Fixed size 253×52px
- Red background (#c32e3b)
- Text hidden via `text-indent: -5000px`

**UIKit Mapping:**
- **Component:** Logo or custom branding (UIKit doesn't have a logo component)
- **Implementation:** Plain `<a>` with background image or inline SVG
- **Class:** Custom `.logo` (keep existing styling)
- **Responsive:** Scale on mobile (reduce size, stack differently)

---

### Navigation Menu

**Current:**
- Horizontal list of links (Releases, Groups, Parties, BBSes, Forums)
- Custom active state (background color #ec5a64)
- Hover effect (top border + margin adjustment)
- Mobile: Hamburger menu with drawer/dropdown

**UIKit Mapping:**
- **Component:** `uk-navbar`
- **Sub-components:**
  - `uk-navbar-container` (wrapper, dark background)
  - `uk-navbar-nav` (horizontal menu items)
  - `uk-navbar-toggle` (hamburger icon for mobile)
  - `uk-navbar-dropdown` (mobile menu or user submenu)
- **Implementation:**
  ```html
  <nav class="uk-navbar-container">
    <div class="uk-navbar-left">
      <a href="/" class="uk-navbar-item uk-logo">Demozoo</a>
      <ul class="uk-navbar-nav">
        <li class="uk-active"><a href="/productions/">Releases</a></li>
        <li><a href="/groups/">Groups</a></li>
        <!-- ... -->
      </ul>
    </div>
    <div class="uk-navbar-right">
      <a href="/user/profile/" class="uk-navbar-item">Profile</a>
    </div>
  </nav>
  ```
- **Custom Styling:** Override default navbar colors with Demozoo palette
  - Background: `#fff` (light) or `#1a1a1a` (dark)
  - Link color: `#5e5f5f` with `#ec5a64` on hover
  - Active background: `#ec5a64`

---

### Search Bar

**Current:**
- Input field with blue background (#77c6ef)
- Dropdown for category filter (Everything, Productions, Graphics, Music, Sceners)
- Submit button (dark blue background, white icon)
- Responsive: Width adjusts at 501px, 601px breakpoints

**UIKit Mapping:**
- **Component:** `uk-search` (built-in search component)
- **Sub-components:**
  - `uk-search-input` (text input)
  - `uk-search-icon` (search icon left of input)
  - Custom `<select>` for category (not part of UIKit search)
  - `uk-button` for submit (custom styling)
- **Implementation:**
  ```html
  <form class="uk-search uk-search-default" method="get" action="/search/">
    <span class="uk-search-icon-flip" uk-search-icon></span>
    <input class="uk-search-input" type="search" name="q" placeholder="Search...">
    <select name="category" class="uk-select">
      <option value="">Everything</option>
      <option value="production">Productions</option>
      <!-- ... -->
    </select>
    <button type="submit" class="uk-button uk-button-default">Search</button>
  </form>
  ```
- **Custom Styling:**
  - Background: `#77c6ef` (light) or adjust for dark mode
  - Icon: Swap to SVG or UIKit icon
  - Layout: Flex container with responsive widths

---

### User Menu

**Current:**
- Top-right corner (`.user_menu` class)
- Login status indicator with icon
- Dropdown on click (Login/Profile/Logout)
- Mobile: Integrated into hamburger menu

**UIKit Mapping:**
- **Component:** `uk-navbar-dropdown` or `uk-drop` (popover)
- **Sub-components:**
  - `uk-navbar-nav` item with dropdown trigger
  - `uk-dropdown` wrapper
  - List of menu items
- **Implementation:**
  ```html
  <div class="uk-navbar-right">
    <div class="uk-navbar-item">
      <button class="uk-button uk-button-text">
        <span uk-icon="user"></span>
        <span>Username</span>
        <span uk-icon="chevron-down"></span>
      </button>
      <div uk-dropdown="mode: click; align: right">
        <ul class="uk-nav uk-dropdown-nav">
          <li><a href="/user/profile/">Profile</a></li>
          <li><a href="/settings/">Settings</a></li>
          <li class="uk-nav-divider"></li>
          <li><a href="/logout/">Logout</a></li>
        </ul>
      </div>
    </div>
  </div>
  ```
- **Custom Styling:** Match existing colors and spacing

---

## Content Sections

### Page Headers & Titles

**Current:**
- `.focus_title` class with FabricaRegular font
- `h2` + `h3` combination (title + subtitle)
- Padding and spacing adjustments
- Production type badges (blue signpost style)

**UIKit Mapping:**
- **Component:** Standard `<h1>`, `<h2>`, `<h3>` headings + custom styling
- **Sub-components:** `uk-text-lead` or `uk-text-large` for subtitles
- **Implementation:**
  ```html
  <div class="page-header">
    <h1 class="uk-heading-large">Production Title</h1>
    <p class="uk-text-meta">By Artist Name · Released 2025</p>
    <span class="uk-label uk-label-primary">Demo</span>
    <span class="uk-label uk-label-primary">Production</span>
  </div>
  ```
- **Custom Styling:**
  - Font: FabricaRegular
  - Color: Demozoo primary (#c32e3b accent)
  - Badge styling: Blue signpost background (#6bc7f1)

---

### Content Blocks & Panels

**Current:**
- `.block` class with border-top and red heading
- `.panel` class with background color
- Padding/margin adjustments for flow

**UIKit Mapping:**
- **Component:** `uk-card` (general container)
- **Sub-components:**
  - `uk-card-header` (title area, can use custom background)
  - `uk-card-body` (content area)
  - `uk-card-footer` (bottom section if needed)
- **Implementation:**
  ```html
  <div class="uk-card uk-card-default">
    <div class="uk-card-header" style="background: #c32e3b; color: #fff;">
      <h3 class="uk-margin-remove">Downloads</h3>
    </div>
    <div class="uk-card-body">
      <!-- Content here -->
    </div>
  </div>
  ```
- **Alternative (Simpler):** Use `uk-section` + custom styling if card overhead is excessive
- **Custom Styling:** Red header background, white text, Fabrica font

---

## Forms & Inputs

### Form Fields

**Current:**
- `.entry_form` class with label + input pair
- Labels floated left (150px width)
- Inputs floated right with max-width
- Responsive adjustments at breakpoints

**UIKit Mapping:**
- **Component:** `uk-form-stacked` or `uk-form-horizontal` (depending on layout)
- **Sub-components:**
  - `uk-form-label` (label)
  - `uk-input`, `uk-select`, `uk-textarea` (input elements)
- **Implementation (Horizontal):**
  ```html
  <form class="uk-form-horizontal">
    <div class="uk-margin">
      <label class="uk-form-label" for="id_title">Title</label>
      <div class="uk-form-controls">
        <input class="uk-input" type="text" name="title" id="id_title">
      </div>
    </div>
  </form>
  ```
- **Mobile Responsive:** Switch to stacked layout on small screens
  ```css
  @media (max-width: 767px) {
    .uk-form-horizontal .uk-form-label {
      display: block;
      margin-bottom: 8px;
    }
  }
  ```

---

### Buttons & Action Groups

**Current:**
- `.button` class with custom styling
- `.action_button` class for small icon buttons
- Groups of actions (`.actions`, `.listing_actions`, `.maintenance_actions`)
- Icon-based buttons with background images

**UIKit Mapping:**
- **Component:** `uk-button` (primary button element)
- **Variants:**
  - `uk-button-primary` (colored, active style)
  - `uk-button-secondary` (default style)
  - `uk-button-text` (text-only)
  - `uk-button-small` or `uk-button-large` (sizing)
- **Sub-components:** `uk-button-group` (grouped buttons)
- **Implementation:**
  ```html
  <!-- Single button -->
  <button class="uk-button uk-button-primary">Save</button>
  
  <!-- Grouped buttons -->
  <div class="uk-button-group">
    <button class="uk-button uk-button-secondary">Edit</button>
    <button class="uk-button uk-button-secondary">Delete</button>
    <button class="uk-button uk-button-secondary">Archive</button>
  </div>
  
  <!-- Icon button -->
  <button class="uk-button uk-button-small">
    <span uk-icon="icon: pencil"></span>
  </button>
  ```
- **Custom Styling:** Override colors to match Demozoo palette
  - Default: `#f5f5f5` background, `#888` text
  - Hover: `#c32e3b` background, `#fff` text
  - Small buttons: 16px font, 4px padding (vs. 0.8em default)

---

## Data Display

### Tables

**Current:**
- `.table` class with simple styling
- Row stripes (alternating background)
- Media thumbnails (48×48px)
- Metadata text (secondary color)

**UIKit Mapping:**
- **Component:** `uk-table`
- **Variants:**
  - `uk-table-divider` (borders between rows)
  - `uk-table-striped` (alternating row backgrounds)
  - `uk-table-hover` (highlight row on hover)
  - `uk-table-small` (compact spacing)
- **Implementation:**
  ```html
  <table class="uk-table uk-table-divider uk-table-striped">
    <thead>
      <tr>
        <th>Title</th>
        <th>Artist</th>
        <th>Date</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><img src="thumb.png" alt="" class="uk-preserve-width"></td>
        <td>Production Title</td>
        <td>2025</td>
      </tr>
    </tbody>
  </table>
  ```
- **Custom Styling:** Keep row stripes with Demozoo colors

---

### Lists

**Current:**
- `.list--results` with flex layout
- `.list--screenshots` with gallery grid
- `.list--party-editions` with responsive columns
- Media thumbnails and metadata

**UIKit Mapping:**
- **Component:** `uk-list` (base list styling) + `uk-grid` (for gallery/multi-column)
- **Sub-components:**
  - `uk-list-divider` (borders)
  - `uk-list-striped` (row backgrounds)
- **Grid Implementation (Gallery):**
  ```html
  <ul class="uk-grid uk-grid-small uk-child-width-1-2 uk-child-width-1-3@m uk-child-width-1-4@l" uk-grid>
    <li>
      <div class="uk-card uk-card-small">
        <div class="uk-card-media-top">
          <img src="screenshot.png" alt="">
        </div>
        <div class="uk-card-body">
          <h3>Screenshot Title</h3>
        </div>
      </div>
    </li>
  </ul>
  ```
- **Responsive Columns:**
  - Mobile: 1 column (`uk-child-width-1-1`)
  - Tablet: 2 columns (`uk-child-width-1-2@m`)
  - Desktop: 3–4 columns (`uk-child-width-1-3@l` or `@xl`)

---

## Media & Images

### Thumbnails

**Current:**
- Fixed size (48×48px, 96×96px, or 3em square)
- CSS `object-fit: cover` for consistent aspect ratio
- Border/shadow styling
- Hover effects (opacity change)

**UIKit Mapping:**
- **Component:** `uk-image` (no special component, plain `<img>`)
- **Container:** `.media` or `.media--thumbnail` wrapper
- **Implementation:**
  ```html
  <div class="uk-card uk-card-small">
    <div class="uk-card-media-top">
      <img src="thumb.png" alt="Production" class="uk-width-1-1" style="object-fit: cover;">
    </div>
    <div class="uk-card-body">
      <h4>Title</h4>
    </div>
  </div>
  ```
- **Custom Styling:** `object-fit: cover` for square thumbnails, `box-shadow: inset 0 0 1px rgba(0,0,0,.25)` for depth

---

### Screenshot Carousel

**Current:**
- `.carousel` class with viewport and tray
- Navigation arrows (prev/next)
- Zoom controls
- Responsive sizing

**UIKit Mapping:**
- **Component:** `uk-slideshow` (built-in carousel)
- **Sub-components:**
  - `uk-slideshow-items` (items container)
  - Navigation buttons (`uk-slideshow-nav`, `uk-slidenav-previous`, `uk-slidenav-next`)
  - Thumbnail navigation (`uk-thumbnav`)
- **Implementation:**
  ```html
  <div class="uk-position-relative uk-visible-toggle" tabindex="-1" uk-slideshow>
    <ul class="uk-slideshow-items">
      <li><img src="screenshot1.png" alt=""></li>
      <li><img src="screenshot2.png" alt=""></li>
    </ul>
    <a class="uk-position-center-left uk-position-small uk-hidden-hover" href="#" uk-slidenav-previous uk-slideshow-item="previous"></a>
    <a class="uk-position-center-right uk-position-small uk-hidden-hover" href="#" uk-slidenav-next uk-slideshow-item="next"></a>
  </div>
  ```
- **Custom Styling:** Width 400px (fixed), height 300px, black background

---

## Modals & Overlays

### Lightbox

**Current:**
- `.lightbox` overlay with dark background
- Modal dialog box (620px wide, auto height)
- Close button (top-right)
- Content area (media or text)

**UIKit Mapping:**
- **Component:** `uk-modal`
- **Sub-components:**
  - `uk-modal-container` (modal wrapper)
  - `uk-modal-dialog` (inner content box)
  - `uk-modal-header` / `uk-modal-body` / `uk-modal-footer` (sections)
  - Close button
- **Implementation:**
  ```html
  <div id="lightbox" uk-modal>
    <div class="uk-modal-dialog uk-modal-body">
      <button class="uk-modal-close-default" type="button" uk-close></button>
      <img src="media.png" alt="Content">
    </div>
  </div>
  ```
- **Custom Styling:** 620px max-width, black border, gray background (#eee)

---

## Typography & Text Utilities

### Headings

**Current:**
- FabricaRegular font for h2, h3, h4
- Specific font-sizes (26pt, 14pt, 11pt)
- Letter-spacing adjustments (-1px on h3/h4)
- Color: Varies (usually #333, sometimes custom accent)

**UIKit Mapping:**
- **Component:** Standard `<h1>` through `<h6>` + UIKit text classes
- **Classes:**
  - `uk-heading-xlarge`, `uk-heading-large`, `uk-heading-medium`, `uk-heading-small`
  - `uk-text-lead` (larger intro text)
  - `uk-text-large`, `uk-text-small`, `uk-text-meta`
- **Implementation:**
  ```html
  <h1 class="uk-heading-large">Main Title</h1>
  <h2 class="uk-heading-medium">Section Title</h2>
  <p class="uk-text-lead">Introduction paragraph</p>
  <p class="uk-text-small uk-text-muted">Metadata text</p>
  ```
- **Custom Font:** Override with FabricaRegular via `@font-face` in custom CSS
  ```css
  h1, h2, h3, h4, h5, h6 {
    font-family: FabricaRegular, sans-serif;
    letter-spacing: -0.05ex;
  }
  ```

---

### Text Utilities

**Current:**
- `.meta` (secondary text color #888)
- `.small` (small font size)
- `.text-muted` (muted color)
- Text truncation (`overflow: hidden`, `text-overflow: ellipsis`)
- Word-break for URLs (`word-break: break-word`)

**UIKit Mapping:**
- **Classes:**
  - `uk-text-meta` (metadata text)
  - `uk-text-small` (small size)
  - `uk-text-muted` (muted color)
  - `uk-text-truncate` (single-line ellipsis)
  - `uk-text-center`, `uk-text-left`, `uk-text-right` (alignment)
  - `uk-text-uppercase`, `uk-text-capitalize` (transform)
- **Implementation:**
  ```html
  <p class="uk-text-meta">By Artist · 2025</p>
  <p class="uk-text-small">Supporting text</p>
  <p class="uk-text-truncate">Long URL that wraps or truncates...</p>
  ```
- **Custom Utility:** Add custom CSS for word-break on URLs
  ```css
  a {
    word-break: break-word;
  }
  ```

---

## Icons

### Current Icon System

**Current:**
- PNG-based icon sprites
- Background images in CSS
- 16px × 16px or 20px × 20px sizes
- Inline SVG in some places

**UIKit Mapping:**
- **Component:** `uk-icon` (SVG-based)
- **Usage:**
  - `<span uk-icon="icon: pencil"></span>` (inline)
  - `<span uk-icon="icon: pencil; ratio: 2"></span>` (larger)
  - `<button uk-icon="heart"></button>` (icon button)
- **Icon Library:** UIKit includes ~50 common icons
- **Custom Icons:** Use inline SVG or custom icon font
- **Implementation (Migration):**
  1. Replace PNG icons with UIKit SVG icons where available
  2. For Demozoo-specific icons (disk, palette, music, scener, group):
     - Convert to SVG
     - Create custom UIKit icon library or inline SVG
  3. Color via CSS custom properties (see Theming)

---

## Specific Component Gaps

### Not Directly Available in UIKit

| Demozoo Element | Current Solution | UIKit Alternative | Gap |
|-----------------|------------------|-------------------|-----|
| Tagit (tag input) | Custom jQuery plugin | No built-in tag input | Use `uk-input` + custom JS or external lib |
| Autocomplete | jQuery UI autocomplete | No built-in autocomplete | Use `uk-input` + custom JS or external lib |
| Drag-drop (ranking) | None (broken feature) | No drag-drop component | Requires Sortable.js or custom JS |
| Drag-drop (upload) | Custom implementation | No drag-drop upload component | Use `uk-upload` (if available in UIKit 4+) or custom JS |
| Editable grid | Custom implementation | No editable grid component | Use `uk-table` + custom JS or form overlay |
| Lightbox carousel | Custom implementation | `uk-slideshow` | Good match, minor styling adjustments |
| Comments panel | Custom implementation | `uk-comment` | Good match, add nesting support |

---

## Implementation Priorities

### Phase 1 (High-Impact, Low Effort)

- [ ] Navigation: Replace with `uk-navbar`
- [ ] Search: Use `uk-search`
- [ ] Buttons: Use `uk-button` + `uk-button-group`
- [ ] Typography: Use UIKit text classes
- [ ] Cards: Replace `.block` and `.panel` with `uk-card`

### Phase 2 (Medium Impact, Medium Effort)

- [ ] Forms: Replace `.entry_form` with `uk-form-horizontal` / `uk-form-stacked`
- [ ] Tables: Use `uk-table`
- [ ] Lists/Grids: Use `uk-grid` + `uk-child-width-*`
- [ ] Lightbox: Replace custom modal with `uk-modal`
- [ ] Carousel: Use `uk-slideshow`

### Phase 3 (Lower Priority, Custom Implementation)

- [ ] Tagit: Integrate or replace
- [ ] Autocomplete: Enhance with custom JS
- [ ] Drag-drop: Add Sortable.js if needed
- [ ] Comments: Enhance `uk-comment` nesting

---

## Styling Overrides Template

```css
/* Demozoo-specific overrides for UIKit */

/* Navbar */
.uk-navbar-container {
  background: var(--bg-surface);
  border-bottom: 1px solid var(--border-medium);
}

.uk-navbar-nav > li > a {
  color: var(--nav-link);
  text-transform: uppercase;
  font-family: FabricaRegular, sans-serif;
}

.uk-navbar-nav > li.uk-active > a,
.uk-navbar-nav > li > a:hover {
  background: var(--nav-link-hover);
  color: #fff;
}

/* Buttons */
.uk-button {
  text-transform: uppercase;
  font-family: FabricaRegular, sans-serif;
  letter-spacing: -0.05ex;
}

.uk-button-default {
  background: var(--button-bg);
  color: var(--button-text);
  border-color: var(--button-border);
}

.uk-button-default:hover {
  background: var(--color-primary);
  color: #fff;
}

/* Cards */
.uk-card-header {
  background: var(--bg-surface);
  border-bottom: 1px solid var(--border-medium);
  padding: var(--card-header-padding, 12px 16px);
}

.uk-card-header h3 {
  font-family: FabricaRegular, sans-serif;
  margin: 0;
}

/* Dark Mode */
@media (prefers-color-scheme: dark) {
  .uk-navbar-container {
    background: var(--bg-surface);
  }

  .uk-button-default {
    background: var(--button-bg);
    color: var(--button-text);
  }
}
```

---

## References

- UIKit Components: https://getuikit.com/docs/components
- UIKit Grid: https://getuikit.com/docs/grid
- UIKit Navbar: https://getuikit.com/docs/navbar
- UIKit Buttons: https://getuikit.com/docs/button
- UIKit Forms: https://getuikit.com/docs/form
- UIKit Modal: https://getuikit.com/docs/modal
- UIKit Slideshow: https://getuikit.com/docs/slideshow
- UIKit Icons: https://getuikit.com/docs/icon
