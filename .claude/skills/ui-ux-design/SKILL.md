---
name: ui-ux-design
description: Generates UI/UX from the approved CLaaS2SaaS design system — approved design tokens, brand palette, logos, favicons, a persistent sidebar shell, mobile-first responsive layout, and a reusable component library, specified as written guidelines with a bundled brand asset pack. Use when designing or generating any screen, page, layout, or component, when the user mentions this design system, or when they name a specific component (buttons, forms, tables, top bar, side nav, toasts, modals, etc.).
---

# UI/UX Design Skill

Generates UI/UX that is **identical in look and feel** across every application, while allowing **domain-appropriate flexibility** in content, entities, and navigation.

These are the **approved rules and values** for the design system. **Always use them, by default.** Deviate only when the developer explicitly asks. The layout, both themes, color combination, fonts, logos, and favicons are already set; only the app name, title-bar name, navigation tree, entities, roles, and domain copy vary per app.

## How to use this skill

1. Read this file for the **design tokens**, **approved brand identity**, **philosophy**, and **global rules** below (they apply to every component).
2. The **complete approved values** live in `reference/design-tokens.md` - the single source of truth for every token (both themes), type, shape, elevation, and shell dimension. Implement those values exactly; when in doubt about a value, that file decides.
3. Identify which component(s) the task needs, then open **only** the matching file(s) from the **Component Index**. Do not load files you don't need.
4. Before finalizing, run the relevant component file's Do/Don't checklist.

Every rule is tagged **ENFORCED** (must follow exactly) or **PRINCIPLED** (sensible default; deviation allowed with written justification).

---

## Design philosophy (ENFORCED principles)

- **Structural DNA** - persistent left sidebar, mobile-first responsive, three-tier layout.
- **Visual language** - 60-30-10 color split, Montserrat headings + Source Sans 3 body.
- **Progressive disclosure** - show only what's needed now; hide advanced behind "show more"/modals.
- **Predictable response** - every action has feedback (hover/active/loading); every async op shows loading; every error shows recovery; every success shows confirmation.
- **Apply the standard exactly, every time** - build a concept the way the reference files prescribe (same archetype for a kind of screen, same primitive for a control, same status-role meaning, same icon for an action); do not reinterpret or invent a variant, and never repurpose a status role. Only the app identity (name, title), nav tree, entities, roles, and domain content vary - **never** the tokens, palette, fonts, logos, or theme.

**60-30-10 rule:** 60% neutral background (canvas + surfaces) · 30% secondary UI (Cadmium Violet / Jelly Bean Blue / Sunray) · 10% primary accent (Midnight Blue).

---

## Approved brand identity (ENFORCED — never ask, never vary)

Approved constants under `assets/` (next to this file): the four per-theme logos and two favicons. Use each asset **as supplied** - never recolour, box, pad, plate, stretch, regenerate, or substitute it.

| Asset | Light theme | Dark theme |
| --- | --- | --- |
| Company logo, full wordmark (expanded sidebar) | `logo-full-light.png` (navy + gold) | `logo-full-dark.png` (white + gold) |
| Company logo, C2S short mark (collapsed rail) | `logo-short-light.png` | `logo-short-dark.png` |
| Favicon (multi-size `.ico`: 16/32/48/256) | `favicon-light.ico` | `favicon-dark.ico` |

- Company name: **CLaaS2SaaS**.
- Fonts: headings **Montserrat** (600/700), body **Source Sans 3** (400/500/600/700), loaded from Google Fonts with the import in `reference/design-tokens.md`.
- The logo and favicon **swap with the effective theme** (light variants on light chrome, dark variants on dark chrome); wire the favicon swap into the theme switcher.
- **Where the asset files go inside the project is the developer's decision.** Each stack differs (`public/`, `wwwroot/`, `static/`, `assets/`, ...), so the destination is recorded in `.claude/PROJECT-CONTEXT.md` as the brand-assets path (`<BRAND_ASSETS_PATH>`). If not recorded, **ask the developer where to put the files before generating UI that references them - never choose the location yourself.** The developer may instead copy the files manually and record the path.
- Still per-app (developer supplies, never invented): **app name**, **browser title-bar name**, **sidebar menu structure**, the **brand-assets path**, entities, roles, and domain copy.

---

## Design tokens (ENFORCED — single source of truth)

The authoritative, complete token set (including badges, chrome, popovers, tabs, toasts) is written out in `reference/design-tokens.md` (light + dark). Token names may adapt to the stack, values may not. Core values, used in every app:

**Brand palette (CLaaS2SaaS Brand Guidelines, Oct 2025):**

| Role | Color | Hex |
| --- | --- | --- |
| Primary accent (actions, links, focus, active tab) | Midnight Blue | `#193E6B` |
| Secondary accent (active-nav gold treatment) | Green Gold | `#B3A125` |
| Secondary (buttons) | Cadmium Violet | `#7F3F98` |
| Secondary (non-interactive headers) | Jelly Bean Blue | `#448E9D` |
| semantic.warning | Sunray | `#E9AC53` |
| semantic.success | Avocado Green | `#5F8025` |
| semantic.danger | Violet-Red | `#991547` |
| semantic.info | Jelly Bean Blue | `#448E9D` |

**Surfaces and text (per theme - both approved):**

| Token | Light | Dark |
| --- | --- | --- |
| Chrome (sidebar + top bar, ONE color) | White `#FFFFFF` | Ink navy `#080F1A` |
| Canvas (working area, clearly distinct) | Warm platinum `#E8DFD0` | Lifted navy `#1A2E46` |
| Card / surface | White `#FFFFFF` | Raised navy `#253E5D` |
| Text (ink, not gray) | `#1E2E42` | `#E9EFF6` |
| Muted text | `#4D5E75` | `#A3B2C5` |
| Interactive accent | `#193E6B` | `#7FADE1` |
| Nav active (gold) | tint of `#B3A125`, fg `#5C5010`, bar `#B3A125` | tint of `#C9B62F`, fg `#EBDD7E`, bar `#C9B62F` |

Spacing, radius, and shadows are likewise approved: 4px spacing scale (`4/8/16/24/32/48/64`), radii `4/8/12/16` (cards 12, controls 8), and the minimal shadow set in `reference/design-tokens.md` (flat resting cards; soft elevation only for overlays and the chrome edge).

Never hardcode raw values in components; define the tokens once and reference them everywhere. Every text/surface pair is **WCAG AA verified** (body text well above 4.5:1); when a new tint is genuinely needed, derive it from an approved hue and keep that bar. Semantic colors used as **text or icons** go through the theme-aware readable tokens (`--badge-*-fg`), never the raw semantic hex on a surface.

**Color role rules (ENFORCED):** secondary **buttons** use only Cadmium Violet; Jelly Bean Blue and Sunray are non-interactive only. Destructive is always Violet-Red. Green Gold is the active-nav / highlight treatment only - never warnings or deletion.

**Theme switcher (REQUIRED).** Lives in the profile menu's fixed **Appearance** section. System / Dark / Light (that segment order, System default), presented as a connected segmented control (with a leading icon on the Appearance row) inside the profile menu, persisted, with **every token defined in both themes** and `color-scheme` declared per theme. The two themes' chromes are different colors by design (white vs ink navy); never render the same chrome in both modes.

```css
:root { color-scheme: light; }   /* light values - see reference/design-tokens.md */
[data-theme="dark"] { color-scheme: dark; }   /* dark values - same file, complete set */
```

---

## Responsive rule

Design **mobile-first**: author base styles for small screens, then layer up. On small screens content **stacks into a single column**, the **sidebar goes off-canvas** (slide-in with a dimmed backdrop), and interactive **touch targets are >= 44px**. Use **width and/or aspect-ratio breakpoints** as needed.

```css
/* Mobile-first base styles, then layer up. Width OR aspect-ratio queries are both fine: */
@media (min-width: 768px)       { /* tablet+: restore sidebar, multi-column where it helps */ }
@media (min-width: 1024px)      { /* desktop: wider containers, multi-column */ }
@media (max-aspect-ratio: 1/1)  { /* portrait: sidebar off-canvas, stack, >=44px touch */ }
@media (min-aspect-ratio: 16/9) { /* wide: wider containers, multi-column */ }
```

Avoid: JS for responsive layout logic; separate mobile/desktop codebases. (Pixel/width breakpoints are allowed.)

---

## Render only functional UI

Do not render UI for not-yet-functional features - **except** not-yet-built **navigation destinations**, which render **disabled with a "Soon" indicator** so the planned structure stays visible. For everything else (controls, panels, unwired filter fields) omit it - hide the trigger **and** its markup: no greyed-out placeholders, no empty panels in the DOM, no dead handlers.

- Unbuilt **nav destination** -> render **disabled + "Soon"** (kept visible).
- **Unauthorized** (role-gated) anything -> **absent** (`render null`), never dimmed.
- Unbuilt **non-nav** control / panel / field -> **absent** (omit the markup).
- A group or cluster with zero visible children must not render its header.

---

## Component Index — open only the file you need

| Component | Reference file | Use when |
|-----------|----------------|----------|
| Design tokens (ALL approved values) | `reference/design-tokens.md` | Any color, surface, font, size, elevation, or dimension value |
| Typography | `reference/typography.md` | Setting font family, weight, or size |
| Top bar & side nav (app shell) | `reference/topbar-sidenav.md` | Building the shell / primary navigation |
| Buttons | `reference/buttons.md` | Any button variant or state |
| Cards | `reference/cards.md` | Card layouts and variants |
| Forms & validation | `reference/forms.md` | Inputs, field sizes, validation states |
| Tables & pagination | `reference/tables.md` | Data tables, sorting, pagination |
| Modals & confirmation dialogs | `reference/modals-dialogs.md` | Modals, delete/discard confirmations, segmented/tabbed form modal |
| Toasts & notifications | `reference/toasts.md` | Success/error feedback messages |
| Search & filter | `reference/search-filter.md` | Search inputs, filter controls |
| Date & time picker | `reference/date-time-picker.md` | Date/time/range selection |
| Empty & loading states | `reference/empty-and-loading-state.md` | Skeletons, button/field loading, no-data / no-results / post-delete |
| Breadcrumbs | `reference/breadcrumbs.md` | Navigation > 3 levels deep |
| Tabs (in-canvas) | `reference/tabs.md` | Depth-overflow nav beyond 3 accordion levels, or a record's facets |
| Icons & logo | `reference/assets.md` | Logo asset rules, the bundled icon registry, asset verification |

Together these files specify the whole system - what to build and how. Follow them exactly the same way every time.

---

## Global anti-patterns (ENFORCED — NEVER)

- Change, re-derive, or "improve" the theme: no new palettes, surfaces, fonts, logos, shadows, or per-app re-skins.
- Move standing navigation to the **top bar**, or replace the **sidebar** with top tabs / a hamburger (except mobile collapse). *(In-canvas content tabs - depth-overflow nav or record facets - are fine; see `reference/tabs.md`.)*
- JS-driven responsive logic, or separate mobile/desktop codebases. (Pixel/width breakpoints are allowed - see Responsive rule.)
- Hardcode colors or use system fonts (use the approved tokens + Montserrat/Source Sans 3).
- Skip loading / empty / error states.
- Vary button colors by domain.
- Use Green Gold for destructive, or Jelly Bean Blue / Sunray for buttons.
- Invent, generate, recolour, or plate a logo; use the bundled per-theme assets as-is.

---

## Deviating from a PRINCIPLED default

State: **Standard pattern** -> **Proposed deviation** -> **Rationale (2-3 sentences)** -> **Domain context** -> **Trade-offs acknowledged**. Get sign-off before generating. Anything tagged ENFORCED - including every token value, brand asset, and theme decision - is not deviable.
