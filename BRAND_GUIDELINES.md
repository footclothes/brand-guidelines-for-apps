# FootClothes / Dashboard World — Brand Guidelines for Apps

Reference for building a new product that matches the Dashboard World look and feel.
Source of truth: `dashboardworld/src/app/globals.css`, `tailwind.config.ts`, and `src/app/layout.tsx`.
When this document and `globals.css` disagree, `globals.css` wins.

Design character: dense internal operations tool with light "FootClothes" personality.
Useful under pressure before decorative. Dark mode is primary; light mode is fully supported.
Every visual choice is driven by CSS variables (design tokens), not hard-coded hex. Copy the token
structure, not just the values.

---

## 1. Color

### 1.1 Raw brand palette
These are the base brand hues. They stay constant across themes.

| Token | Hex | Use |
| --- | --- | --- |
| `--color-ink` | `#111111` | Deepest text / black |
| `--color-bone` | `#f7f2ea` | Light "paper" tone, button text on dark |
| `--color-paper` | `#fffdf8` | Brightest surface |
| `--color-warm-gray` | `#d8d1c7` | Muted warm neutral |
| `--color-charcoal` | `#2a2a2a` | Dark neutral |
| `--color-oxblood` | `#741f2b` | Primary action red |
| `--color-plum` | `#4e294f` | Secondary brand purple (avatars) |
| `--color-forest` | `#2f4a3e` | Success green base |
| `--color-gold` | `#c79a35` | Accent / focus / warning / primary nav |
| `--color-pink` | `#e94b8a` | Highlight accent, selection |

Tailwind exposes these as `ink`, `bone`, `paper`, `charcoal`, `warmgray`, `oxblood`, `plum`, `forest`, `gold`, `pink`.

### 1.2 Semantic tokens (theme-aware)
Build UI against these, not the raw palette. Left = dark (default), right = light.

| Token | Dark | Light | Role |
| --- | --- | --- | --- |
| `--surface` | `#171313` | `#f7f2ea` | App base surface |
| `--surface-raised` | `#211b1d` | `#fffdf8` | Panels / cards |
| `--surface-soft` | `#2b2426` | `#eee6da` | Recessed surface |
| `--text-main` | `#fff7ea` | `#171313` | Primary text |
| `--text-muted` | `#cdbfb0` | `#5d554d` | Secondary text |
| `--line` | `rgba(247,242,234,.14)` | `rgba(17,17,17,.14)` | Default border |
| `--line-subtle` | `rgba(255,253,248,.10)` | `rgba(17,17,17,.12)` | Quiet border |
| `--line-strong` | `rgba(255,253,248,.15)` | `rgba(17,17,17,.18)` | Emphasis border |
| `--menu-surface` | `#181415` | `#fffaf2` | Menus / dropdowns |
| `--surface-hover` | `rgba(255,253,248,.05)` | `rgba(116,31,43,.07)` | Hover state |
| `--surface-active` | `rgba(255,253,248,.10)` | `rgba(116,31,43,.10)` | Active/selected |

Page background is a diagonal gradient, not a flat fill:
- Dark: `linear-gradient(135deg, #100d0e 0%, #171313 52%, #17140f 100%)`
- Light: `linear-gradient(135deg, #f7f2ea 0%, #fffdf8 52%, #efe4d5 100%)`

A faint 44px grid overlay sits on the page background at ~24% opacity, masked to fade out toward the
bottom. Selection color is `rgba(233,75,138,.42)` (pink).

### 1.3 Status colors
Four fixed status variants, each with border / surface / text tokens: `--status-{variant}-{border,surface,text}`.

| Variant | Meaning | Dark text | Light text |
| --- | --- | --- | --- |
| `neutral` | default / info | `--text-muted` | `#4f473f` |
| `success` | ok / complete | `#b8e0cb` | `#264f3e` |
| `warning` | caution | `#c79a35` (gold) | `#7a5514` |
| `danger` | error / destructive | `#ffb8c2` | `#741f2b` |

Accent tones extend color choice without new status names: `accent` (default pink), plus `red`, `blue`,
`green`, `yellow`, `orange`, `purple` via `--status-accent-{tone}-*`. Green/yellow reuse success/warning tokens.

Rule: never rely on color alone for status. Pair with text, icon, or shape.

---

## 2. Typography

### 2.1 Families
| Role | Stack | Notes |
| --- | --- | --- |
| Display / headings | `Oswald` (600, 700) → `Impact`, `Arial Narrow`, sans | Loaded via `next/font/google`, bound to `--font-display-oswald`. Condensed, uppercase-friendly. |
| Body / UI | `Source Sans 3` → `DM Sans`, `Segoe UI`, sans | Default body font. |
| Mono | `IBM Plex Mono` → `SFMono-Regular`, `Consolas`, mono | Code blocks, IDs. |

Tailwind aliases: `font-display`, `font-sans`, `font-mono`.

### 2.2 Type scale
Headings use Oswald, weight 700, `letter-spacing: 0`.

| Class | Size (desktop) | Size (mobile) | Line height |
| --- | --- | --- | --- |
| `.ui-heading-xl` | 2.5rem | 2rem | 1.05 |
| `.ui-heading-lg` | 2rem | 1.62rem | 1.12 |
| `.ui-heading-md` | 1.5rem | 1.32rem | 1.18 |
| `.ui-heading-sm` | 1.05rem | — | 1.3 |
| `.ui-body-text` | 1rem | — | 1.65 |
| `.ui-subtext` | 0.875rem | — | 1.55 |
| `.ui-muted-text` | inherits, `--text-muted` | — | — |

Micro-labels (table headers, eyebrows) use uppercase Oswald/sans at ~0.68–0.7rem with wide tracking
(`letter-spacing: 0.14em`–`0.18em`). Body strong = weight ~780.

### 2.3 Casing rules
- Casing utilities: `.ui-case-title`, `.ui-case-lower`, `.ui-case-upper` — for shell/UI labels only.
- Never case-transform user data: names, emails, IDs, product names, user input stay as entered.

---

## 3. Shape, elevation, motion

### 3.1 Radius
- `--radius-rect: 0.38rem` — the standard corner radius for panels, buttons, controls.
- Badges: `0.38rem`. Compact labels: `0.34rem`. Pills/scroll thumbs: `9999px`.

Aesthetic is rectangular and tight, not heavily rounded.

### 3.2 Shadows
| Token | Value (dark) |
| --- | --- |
| `--shadow-panel` | `0 1px 0 rgba(255,253,248,.06), 0 24px 60px rgba(0,0,0,.22)` |
| `--shadow-button` | `0 10px 22px rgba(5,4,3,.22)` |
| `--shadow-overlay` | `0 25px 75px rgba(0,0,0,.45)` |
| `--shadow-drawer` | `0 20px 80px rgba(0,0,0,.38)` |

Light mode uses warmer, softer versions of the same tokens.

### 3.3 Motion
Quick, subtle, operational. Use the tokens; don't invent durations.

| Token | Value | Use |
| --- | --- | --- |
| `--motion-fast` | 160ms | Hover, color, border, small transforms |
| `--motion-expand` | 180ms | Inline expansion, caret rotation, card reveal |
| `--motion-drawer` | 220ms | Drawer / backdrop entry + exit |
| `--motion-ease-standard` | `ease` | Default easing |
| `--motion-ease-emphasized` | `cubic-bezier(0.16, 1, 0.3, 1)` | Expansion easing |

Keep motion CSS-only by default. Always pair with reduced-motion fallbacks. Animate expand/collapse in
both directions.

### 3.4 Focus
Focus ring: 2px `--color-gold` outline, `outline-offset: 3px`, shown on `:focus-visible` only.

---

## 4. Core components

Build these first; reuse rather than re-style.

| Component / class | Definition |
| --- | --- |
| `.ui-panel` | Standard content surface: `1px` subtle border, `--radius-rect`, `--surface-raised` bg, `--shadow-panel`, `1rem` padding. `.ui-panel-flush` removes padding. |
| `.ui-button` | Base: transparent, `1px` border, `--radius-rect`, weight 700, `min-height 2.3rem`, padding `0.42rem 0.72rem`. `.ui-button-sm` for compact. |
| `.ui-button-primary` | Oxblood `#741f2b` bg, bone text; hover `#8a2937`. |
| `.ui-button-secondary` | Strong border, soft hover surface, main text. |
| `.ui-button-accent` | Gold border/text on faint gold surface. |
| `.ui-button-destructive` | Oxblood-tinted border/surface, danger text. |
| `.ui-badge` / `.ui-badge-{neutral,success,warning,danger,accent}` | Inline status label, `0.7rem` weight 800, `white-space: nowrap`. |
| `.ui-compact-label` | Quieter than a badge; same status variants. |
| `.ui-control`, `.ui-select`, `.ui-textarea` | Form inputs, `min-height 2.35rem`, gold focus border. |

Class families in `globals.css`: surfaces (`ui-panel`, `ui-data-row`, `ui-empty-state`, `ui-code-block`),
buttons/controls, labels/type, tables (`ui-advanced-table-*`), progressive disclosure/layout
(`ui-collapsible-card-*`, `ui-page-tabs`, `ui-drawer-*`).

---

## 5. Layout & page structure

Every working screen includes, top to bottom:
1. **Breadcrumbs** near the top of main content, matching the sidebar path.
2. **PageHeader** — eyebrow (optional context), title (canonical H1), description, optional badge, optional actions. Do not repeat the same label across breadcrumb, eyebrow, and title.
3. Content built from shared panels/components, not one-off wrappers.

Shell:
- Left sidebar with sections (top-level groupings), page groups (expandable, may have own route), and pages.
- Sidebar pins/unpins; unpinned rests as an icon rail and peeks on hover.
- Nav icons are semantically colored by section (primary gold vs alternate red).
- Mobile: fixed top strip + animated side drawer reusing the same nav tree.

Density philosophy:
- Dense, quiet, operational, scannable.
- Prefer plain text, small dots, compact labels for normal state. Reserve badges for warnings, restrictions, environment state, exceptions.
- Cards live at page surface level; do not nest decorative cards inside cards.
- Keep sections unframed unless they are real repeated items, modals, drawers, tables, forms, or panels.
- Use `lucide-react` icons for buttons and nav when an icon exists.
- Do not scale font size with viewport width; use stable responsive layout constraints.

Interaction primitives to reuse: `AdvancedTable` (dense sortable/searchable/paginated, 50/page default),
`AnimatedDrawer` (side drawers, shared persisted width), `CollapsibleCard` (card-level disclosure),
`PageTabs` (local sections on one route), `SearchableSelect` (long option lists).

---

## 6. Voice & copy

- Playful language only in low-risk places: empty states, helper text, benign labels.
- Direct, serious copy for: errors, destructive actions, permission failures, failed jobs, production-affecting actions.

---

## 7. Accessibility

- Semantic HTML; keyboard navigable.
- High contrast; check both dark and light themes for text, borders, focus, badge/control contrast.
- Never rely on color alone for status.
- Dense tables stay horizontally scrollable on narrow screens; prefer stacked card lists on 360–430px when actions would clip.
- Honor reduced-motion.

---

## 8. Starting a new app fast

1. Copy the token blocks from `globals.css` (`:root`/`[data-theme="dark"]` and `[data-theme="light"]`) into the new project's global stylesheet.
2. Copy the `tailwind.config.ts` color/font/shadow extends.
3. Load Oswald via `next/font/google` → `--font-display-oswald`; set body font to `Source Sans 3` stack.
4. Set `data-theme="dark"` on `<html>` as the default; wire a theme toggle that switches to `light`.
5. Reuse the `ui-*` class families rather than authoring new one-off Tailwind stacks.
6. Enforce the page contract: Breadcrumbs + PageHeader on every working screen.

Anchor the new product to these tokens. Keep the token names identical so future changes port cleanly
between products.
