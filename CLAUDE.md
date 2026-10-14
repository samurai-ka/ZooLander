# ZooLander – project context for Claude

**ZooLander is a design language project, not a homepage rebuild.** It hosts a new, reusable design language for Demozoo-style sites, built on **UIKit**, that other people and projects can adopt. It derives its colours, fonts and layout character from the existing Demozoo site (ZooKeeper) but expresses them as UIKit-based tokens, theme and components. The deliverable is the design system itself (tokens, light/dark theme, component library, documentation, Figma library); it is **not** a new Demozoo homepage and no site is to be migrated here. Phase so far: analysis + design. No code has been written yet.

Implications of this scope (decided by the user on 2026-10-14):
- The Home screens in Figma are a **demonstration** of the components, not a product page to ship. Do not plan page-by-page site migration.
- Primary consumers are third parties: tokens and the UIKit theme must be self-contained, documented and usable without Demozoo's code base.
- **Distribution decided by the user (2026-10-14):** users consume a **CSS package** (the UIKit-based theme with the tokens as CSS custom properties, light/dark via `prefers-color-scheme`) and a **documentation site** explaining how to use the components. The Figma file is the design source for the maintainers, not a consumer deliverable.
- `plan/07-implementation-roadmap.md` (homepage migration phases, team roles, week estimates) and the "rebuild the homepage" framing in `plan/README.md`, `plan/03` and `plan/06` predate this decision and are **outdated**; reuse only the parts about tokens, theming and components.
- The open Demozoo issues in `plan/06` are useful only as evidence of which patterns the language should cover, not as a work list.

## Hard requirements (from the user)
- **Everything in English** – code, docs, commit messages – even when the user prompts in German.
- All analysis documentation lives in `plan/`.
- The new design MUST support **mobile, tablet and desktop** viewports.
- The new design MUST support **light and dark mode**, including the operating-system setting (`prefers-color-scheme`).
- Do **not** use the GitLab, Atlassian or Datadog MCP servers.
- Do not clone the ZooKeeper repo; read it online (GitHub API / raw URLs; `gh` is not installed, use `curl`).
- **Commit messages follow Conventional Commits** (https://www.conventionalcommits.org, v1.0.0): `type(optional scope): description`, with optional body and footers; breaking changes marked with `!` or a `BREAKING CHANGE:` footer. Use types such as `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`. Messages are in English, imperative mood, lower-case description without trailing period (e.g. `docs(plan): add verified colour palette`, `feat(tokens): add dark theme custom properties`). Commit only when the user asks.
- The user's prompts are often terse; confirm before anything outward-facing (publishing, committing). Nothing has been committed so far; `plan/` and `.claude/` are untracked.

## Sources
| What | Where |
|------|-------|
| Live site (temporary, local) | http://localhost:8000/ (it is Demozoo; CSS at `/static/css/dz.css`) |
| Existing site repo | https://github.com/samurai-ka/ZooKeeper (SCSS in `src/style/`, 8 layers: utility → base → fonts → core → assets → components → pages → shame; BEM-style naming) |
| Upstream issues | https://github.com/demozoo/demozoo/issues |
| UIKit docs / source | https://getuikit.com/docs/introduction · https://github.com/uikit/uikit |
| Figma design file | https://www.figma.com/design/kfGHazLuR6aPKTMw0lIYjn (file key `kfGHazLuR6aPKTMw0lIYjn`, Figma plan "Greenbrain", Starter tier, account samurai.ka@gmail.com) |

## What exists
- `plan/README.md` – index. `plan/01`–`08` – tokens, layout, component mapping, theming, Figma spec, issues vs UIKit, roadmap, testing strategy.
- **`plan/09-figma-file-and-corrections.md` is authoritative.** It holds the verified colour palette with computed contrast ratios, the UIKit breakpoint correction, and a list of known errors in docs 01–08. Docs 01–05, 07, 08 and README carry a correction banner pointing to it.
- Figma file: pages *Foundations* (colour/type/spacing specimen), *Components* (9 component sets, 41 variants, all bound to variables), *Screens* (Home at Mobile 375 / Tablet 768 / Desktop 1280 × Light/Dark). Collections: `Primitives`, `Color Light`, `Color Dark`, `Spacing`, `Radius`, `Layout`.

## Key decisions and facts
- **Figma Starter plan = 1 mode per variable collection**, so Light/Dark is two collections (`Color Light`, `Color Dark`, identical variable names) plus a `Theme=Light|Dark` variant property on colour-dependent components. Mergeable into one two-mode collection on a paid plan.
- **Colour fixes (existing site fails WCAG AA):** `#888` meta text → `#666`; active nav `#ec5a64` → `#c32e3b`; white text on `#6bc7f1` / `#a5b45d` → `#1a1a1a` text; input border `#ddd` → `#767676`. Dark mode: `#ff7a84` only as text colour on dark, button fill stays `#c32e3b`, muted text `#a3a3a3`, visited link `#b399ff`. Always recompute contrast when a token changes (the earlier "all pass" claims were never verified).
- **UIKit breakpoints:** `@s` 640, `@m` 960, `@l` 1200, `@xl` 1600 (earlier docs said 768/1024/1400 – wrong). At 768px UIKit still shows the compact header.
- CSS custom property names follow the Figma code syntax, e.g. `--color-bg-page`, `--color-text-primary`, `--spacing-md`, `--radius-none`; same names in both themes, `prefers-color-scheme` switches the values.
- **Fonts:** real fonts are FabricaRegular (display), system stack (body), DejaVu Sans Mono (code), plus Topaz/IBM VGA for text files; font files are under `/static/font/` on the live site. Figma uses stand-ins: Archivo, Inter, JetBrains Mono.
- Base font 14px on all viewports (existing site: 12px below 768px). Desktop content padding 58px (4.5% of 1280).
- Existing-site CSS facts: container max 1400px, brand red `#c32e3b`, blue `#6bc7f1`, olive `#a5b45d`, link `#02a`, page bg `#f5f5f5`; two-column activity block at ≥768px; old stack uses jQuery 1.7.1 + jQuery UI.
- `plan/06-issues-uikit.md` (20 UI-relevant of 100 open issues; 9 yes / 10 partly / 1 no) and the effort estimates in `plan/07` are **unverified** – agent-generated from issue titles / invented numbers. Check against UIKit docs before relying on them. Claim that UIKit has no built-in dark-mode switch is also unchecked.

## Not done / next steps
1. **Specify the CSS package** (name, how it is built from UIKit and the tokens, file layout, versioning, how consumers load it and switch themes) and **the documentation site** (generator/hosting, page structure: getting started, tokens, theming, one page per component with live examples, markup, UIKit class usage, accessibility notes, light/dark and viewport behaviour). Open: package manager/registry (e.g. npm), docs tooling and hosting. Ask the user before choosing.
2. **Reference screenshots** of the live site (`plan/screenshots/`, 3 viewports × light/dark) to check the language against its source. Blocked: the Playwright MCP could not start (Chrome not installed, `npx playwright install chrome --with-deps` needs sudo). Options: install Chrome/Chromium for the Playwright MCP, or capture manually. Until then the Figma design is derived from CSS/HTML/source only.
3. **Broaden the component library** beyond the nine components (forms, tabs, pagination, modal, alerts, tags, icons, etc.) and document usage rules, so the language is complete enough to adopt.
4. Verify the UIKit claims (dark mode, component fit) against getuikit.com/docs and the UIKit source.
5. Clean up docs 01–08: replace the old dark-palette values and CSS snippets (still contain `#e74c56`, `#808080`, etc.) with the values from `plan/09`, rewrite the homepage-migration parts to match the design-language scope, then remove the correction banners.
6. Build the CSS package: CSS custom properties file with the tokens from `plan/09`, light/dark switching via `prefers-color-scheme` (optional `data-theme` override), UIKit component styling, then the documentation site with an example for every component.
7. Optional: Code Connect mappings. Commit `plan/` and `CLAUDE.md` only after asking the user.

## Working notes for Figma via MCP
- Load the Figma skills before `use_figma` (`get_figma_skill` with `skill://figma/figma-use/SKILL.md`, plus `figma-generate-library` / `figma-generate-design`); pass `skillNames` with the `resource:` prefix.
- `figma.createAutoLayout()` frames have a **default white fill** – set `fills = []` on transparent containers.
- `resize()` resets sizing to FIXED – set `primaryAxisSizingMode = 'AUTO'` again afterwards for hug-height components.
- Use `setTextStyleIdAsync`; wrapping text needs `textAutoResize = 'HEIGHT'` + `layoutSizingHorizontal = 'FILL'`.
- `get_screenshot` returns a URL; download it with `curl` into the scratchpad and `Read` the PNG to view it.
- Component set IDs in the file (may change if rebuilt): Button `6:26`, Input `6:48`, Badge `6:56`, Nav Item `6:70`, Panel Header `6:76`, Production Card `7:16`, Table Row `7:42`, Header `7:112`, Footer `7:130`. Screens `10:14`, `10:117`, `10:234` (Light M/T/D), `10:361`, `10:464`, `10:581` (Dark M/T/D).
- Figma `whoami` plan key: `team::1050112024146622265`.
