# Button UI/UX Design Guide

> **Theming:** values here (palette, fonts, tokens) are approved CLaaS2SaaS constants, never per-app;
> the canonical source of truth is `design-tokens.md` (brand assets in `../assets/`). Every token is
> defined in **both** themes, with a System/Dark/Light switcher. Contrast tables below are computed
> against the light theme; dark pairs are verified in design-tokens.md. Semantic colors used as **text
> or icons** (outline/ghost variants) go through the theme-aware readable tokens (`--badge-*-fg`),
> never the raw semantic hex on a surface.

Reference for AI-assisted development: follow these rules so buttons stay consistent, accessible, and predictable.

> **TL;DR for the AI**
> 1. Pick the variant by **meaning**, not color preference (§3).
> 2. Use the **design tokens** (`var(--color-…)`); never hardcode a new hex or invent a shade.
> 3. Exactly **one** solid/primary button per action group; everything else is outline or ghost.
> 4. Always ship **hover, active, focus-visible, disabled, and loading** states.
> 5. **Dark text** on light buttons (Warning, Accent); **white text** on the rest (§9).
> 6. During async work: **disable + spinner + guard against double-submit**. On success, the toast
>    confirms it — return the button to rest.

---

## Contents

1. [Design tokens (source of truth)](#1-design-tokens-source-of-truth)
2. [The button anatomy](#2-the-button-anatomy)
3. [Variants — meaning drives color](#3-variants--meaning-drives-color)
4. [Emphasis levels (solid / outline / ghost)](#4-emphasis-levels-solid--outline--ghost)
5. [Sizes — driven by usage, not whim](#5-sizes--driven-by-usage-not-whim)
6. [States — how a button should behave](#6-states--how-a-button-should-behave)
7. [Loading & saving behavior](#7-loading--saving-behavior)
8. [Button copy / labels](#8-button-copy--labels)
9. [Contrast reference (WCAG)](#9-contrast-reference-wcag)
10. [Full CSS reference (copy-paste ready)](#10-full-css-reference-copy-paste-ready)
11. [Accessibility checklist](#11-accessibility-checklist)
12. [Do's and Don'ts](#12-dos-and-donts)
13. [Rules for the AI assistant](#13-rules-for-the-ai-assistant)
14. [Quick decision guide](#14-quick-decision-guide)

---

## 1. Design tokens (source of truth)

Buttons consume the approved CLaaS2SaaS palette; no colors outside it. The authoritative token set
(both themes) is `design-tokens.md`; the light values:

```css
:root {
    /* Primary (10%) */
    --color-primary: #193E6B;           /* Midnight Blue */
    --color-accent: #B3A125;            /* Green Gold */
    /* Secondary (30%) */
    --color-secondary-violet: #7F3F98;  /* Cadmium Violet */
    --color-secondary-blue: #448E9D;    /* Jelly Bean Blue — non-interactive only */
    --color-secondary-sunray: #E9AC53;  /* Sunray */
    /* Tertiary & Neutral (60%) */
    --color-success: #5F8025;           /* Avocado Green */
    --color-danger: #991547;            /* Violet-Red */
    --color-background: #E8DFD0;        /* canvas (dark: #1A2E46) */
    --color-surface: #FFFFFF;           /* card (dark: #253E5D) */
    --color-text: #1E2E42;              /* ink (dark: #E9EFF6) */
    --color-text-muted: #4D5E75;        /* (dark: #A3B2C5) */
}
```

---

## 2. The button anatomy

A button is **one semantic meaning** + **one emphasis level** + **one size** + **its current state**.

```
[ variant ] × [ emphasis ] × [ size ] × [ state ]
  primary       solid          md         idle
  danger        outline        sm         loading
  …             ghost          lg         disabled …
```

Class composition example: `class="btn btn-danger btn-outline btn-sm"`.

---

## 3. Variants — meaning drives color

Choose the variant by **what the action does to the user's data or flow**, then apply the mapped color.

| Variant | Token | Hex | Text color | Use for | Do **not** use for |
|---|---|---|---|---|---|
| **Primary** | `--color-primary` | Midnight Blue `#193E6B` | white | The single main action: **Submit, Save, Create, Confirm, Continue** | More than one action per section |
| **Secondary** | `--color-secondary-violet` | Cadmium Violet `#7F3F98` | white | A real alternative beside primary (**Save as draft** next to Publish) | Cancel / Back (use Ghost, §4) |
| **Danger** | `--color-danger` | Violet-Red `#991547` | white | **Destructive / irreversible**: Delete, Remove, Discard, Revoke, Reset | Routine cancels or "no-op" exits |
| **Warning** | `--color-secondary-sunray` | Sunray `#E9AC53` | **dark ink `#1E2E42`** | **Caution, not destruction**: "Proceed anyway", override, leave-with-unsaved-changes | The primary CTA |
| **Success** | `--color-success` | Avocado Green `#5F8025` | white | Confirming a **positive completion**: Approve, Mark complete, Publish | A generic form submit — that's **Primary** |
| **Accent** | `--color-accent` | Green Gold `#B3A125` | **dark ink `#1E2E42`** | **Highlight / upsell CTA**: Upgrade, Try Pro, feature spotlight | Standard form actions |

**Rationale**
- **Destructive actions use `--color-danger` (Violet-Red `#991547`), not the accent.** An accent
  color doesn't read as "dangerous" — reusing it for Delete risks accidental deletion.
- **Green Gold (`--color-accent`) is reserved for Accent / highlight** (Upgrade, spotlight); never warnings or deletion.
- **Warning** uses `--color-secondary-sunray` (Sunray `#E9AC53`), separate from true danger.
- **Cancel / Back** are **not** a filled color — low-emphasis Ghost or Outline (§4).

**Use cases**

| Situation / scenario | Example label | Variant |
|---|---|---|
| The one main action completing a form or view | "Save changes", "Create project", "Continue" | **Primary** |
| A genuine alternative beside the primary | "Save as draft" beside "Publish" | **Secondary** |
| Permanently deleting or removing data | "Delete account", "Remove member", "Revoke access" | **Danger** |
| Same destructive action, deliberate pause wanted | "Delete account" in a danger dialog | **Danger + outline** |
| Proceeding despite a non-destructive caution | "Proceed anyway", "Leave without saving", "Override" | **Warning** (dark text) |
| Approving or finalizing something positive | "Approve", "Mark complete", "Publish" | **Success** |
| Promoting an upsell, trial, or feature highlight | "Upgrade to Pro", "Try premium", "Start free trial" | **Accent** (dark text) |
| Dismissing, going back, or any low-priority exit | "Cancel", "Back", "Skip", "Dismiss" | **Ghost** |
| A secondary/optional action in a toolbar or table row | "Edit", "Duplicate", "Filter" | **Ghost / Outline** |

> **Gut-check:** a routine form submit is **Primary**, not Success. A cancel is **Ghost**, not a filled
> color. A delete is **Danger**, not Accent. When two rules seem to apply, the one tied to the action's
> consequence (destructive, positive completion, upsell) wins over generic "main action".

---

## 4. Emphasis levels (solid / outline / ghost)

Color says *what kind*; emphasis says *how important*. In any group, the eye should land on one.

| Emphasis | Class | Look | Use for |
|---|---|---|---|
| **Solid** (filled) | *(default)* | Filled background | The one primary action in a group |
| **Outline** | `btn-outline` | Colored border + colored text, transparent fill | Medium emphasis: a secondary choice, or a **Danger** action to pause on |
| **Ghost** | `btn-ghost` | No border, no fill until hover | Low emphasis: Cancel, Back, toolbar/table-row actions, tertiary links |

**Pairing rule:** one solid button per group; the rest are outline/ghost.

```html
<!-- Form footer -->
<button class="btn btn-ghost">Cancel</button>
<button class="btn btn-primary">Save changes</button>

<!-- Delete confirmation dialog -->
<button class="btn btn-ghost">Cancel</button>
<button class="btn btn-danger">Delete account</button>

<!-- Wizard -->
<button class="btn btn-outline btn-secondary">Back</button>
<button class="btn btn-primary">Next</button>
```

---

## 5. Sizes — driven by usage, not whim

| Size | Class | Height | Padding | Font | Use for |
|---|---|---|---|---|---|
| **Small** | `btn-sm` | 27px | `0 10px` | 12px | Dense UIs: tables, toolbars, inline/compact actions, filter chips |
| **Medium** *(default)* | `btn` / `btn-md` | 32px | `0 13px` | 15px | The default for forms and most of the app |
| **Large** | `btn-lg` | 48px | `0 24px` | 16px | Prominent CTAs, hero/landing, empty-state actions, mobile primary actions |

**Touch-target rule:** interactive targets should be **≥ 44 × 44px** on touch devices. `btn-sm` and
the 32px default are below that — only use them for mouse/trackpad input or where the element has
extra surrounding hit area. For a mobile primary action, use `btn-lg`.

**Modifiers**

| Modifier | Class | Use for |
|---|---|---|
| Full width | `btn-block` | Mobile forms, narrow cards, single stacked CTA |
| Icon-only | `btn-icon` | Square button with just an icon — **requires `aria-label`** |

---

## 6. States — how a button should behave

Every button must implement all of these. The first five are the interaction contract; loading is §7.

| State | Trigger | Behavior |
|---|---|---|
| **Default / Rest** | — | The variant's base appearance |
| **Hover** | Pointer over (pointer devices) | Lifts slightly (`translateY(-1px)`) and darkens (`brightness 0.94`) |
| **Active / Pressed** | Mouse-down / touch | Presses down (`translateY(1px)`), darkens further (`brightness 0.90`) |
| **Focus (keyboard)** | Tab / keyboard focus | **Visible focus ring** via `:focus-visible` — mandatory; never remove without an equally visible replacement |
| **Disabled** | Action unavailable | `opacity: 0.5`, `cursor: not-allowed`, no hover/active transforms. Don't use disabled to *silently* signal a validation error — explain why nearby |
| **Loading / Saving** | Async work in progress | See §7 |

The enforced interaction CSS is preserved verbatim and extended (focus ring, hover/active darkening) in §10.

> **Disabled vs. "looks done but blocked":** if a primary action is blocked by an incomplete form,
> prefer keeping it enabled and showing the validation message on click, rather than a permanently
> greyed-out button users can't diagnose. If you do disable it, pair it with visible text explaining
> what's missing.

---

## 7. Loading & saving behavior

The button is the user's proof their click registered and work is happening — and the primary guard
against duplicate submissions.

1. **On click of an async action, immediately enter loading:** disable interaction, hide the label
   (`color: transparent`), show a spinner. Prevents double-submission.
2. **Keep the width stable.** The label is hidden, not removed, and the spinner is absolutely centered,
   so the button doesn't collapse. (Add `min-width` if your label changes length.)
3. **Pick the affordance for the duration:** short (< ~1s) spinner only is fine; longer / important /
   destructive → swap the label to a verb-ing form ("Saving…", "Deleting…", "Uploading…").
4. **On success:** return to **rest state**. Success is announced by the toast, so the button doesn't
   confirm anything. (Match the toast to the action: *Save changes* → toast *"Changes saved."*) Never
   leave a button spinning forever.
5. **On error:** return to rest, **re-enable**, surface the error elsewhere (inline / toast). Don't trap
   the user in loading.
6. **Announce to assistive tech:** set `aria-busy="true"` while loading and disable via the `disabled`
   attribute (or `aria-disabled` + a handler guard) so keyboard/screen-reader users can't re-trigger.
7. **Never rely on the spinner alone** for important or destructive operations — pair with text.

```
idle ──click──▶ loading ──success──▶ idle   (+ toast: "Changes saved.")
                   │
                   └────error─────▶ idle   (+ toast / inline error message)
```

### Pattern (framework-free; frameworks mirror this contract)

```js
async function runAction(btn, action) {
  if (btn.classList.contains('loading')) return;   // guard double-submit
  btn.classList.add('loading');
  btn.disabled = true;
  btn.setAttribute('aria-busy', 'true');
  try {
    await action();
    showToast('Changes saved.');   // success is handled by the toast notification system
  } catch (err) {
    throw err;                     // surface the error elsewhere (toast / inline); do not trap in loading
  } finally {
    btn.classList.remove('loading');   // back to rest state
    btn.disabled = false;
    btn.removeAttribute('aria-busy');
  }
}
```

---

## 8. Button copy / labels

- **Say what happens.** Prefer "Save changes" / "Create project" over generic "Submit" or "OK".
- **Keep the verb consistent through the flow.** A "Publish" button should produce a "Published." toast.
- **Sentence case, active voice, no filler.** "Delete file", not "Click here to delete this file."
- **Icon-only buttons still need a name** via `aria-label` (e.g. `aria-label="Delete"`).

---

## 9. Contrast reference (WCAG)

Ratios are computed for the approved palette against a white surface; ✓ = passes WCAG AA for normal
text (≥ 4.5:1). Dark-theme text/surface pairs are verified in `design-tokens.md`; let these results
drive the text color — do not restyle per app.

| Variant background | vs. white text | vs. dark ink `#1E2E42` | Use text color |
|---|---|---|---|
| Midnight Blue `#193E6B` Primary | **10.8 : 1** ✓ AAA | — | **white** |
| Violet-Red `#991547` Danger | **8.2 : 1** ✓ AAA | — | **white** |
| Cadmium Violet `#7F3F98` Secondary | **6.8 : 1** ✓ AA | — | **white** |
| Avocado Green `#5F8025` Success | **4.6 : 1** ✓ AA¹ | — | **white** (use ≥ 15px / 600 weight) |
| Green Gold `#B3A125` Accent | 2.6 : 1 ✗ | **5.3 : 1** ✓ AA | **dark ink `#1E2E42`** |
| Sunray `#E9AC53` Warning | 2.0 : 1 ✗ | **6.9 : 1** ✓ AA | **dark ink `#1E2E42`** |

¹ Borderline; comfortably passes for bold/large labels. Keep Success labels at the default weight (600) or higher.

> **Rule of thumb:** the two **light** buttons (Accent, Warning) use **dark** text. White text on them fails contrast.

---

## 10. Full CSS reference (copy-paste ready)

Enforced interaction rules are preserved verbatim and marked. Uses `color-mix()` (current evergreen browsers).

```css
:root {
  /* Palette from §1 (canonical: design-tokens.md). Button tokens: */
  --btn-radius: 8px;
  --btn-font-weight: 600;
  --btn-focus-ring: var(--color-primary);   /* focus ring is always the primary interactive accent, both themes */
}

/* ===== Base (defaults to medium size) ===== */
.btn {
  display: inline-flex; align-items: center; justify-content: center; gap: 0.5em;
  border: 1px solid transparent; border-radius: var(--btn-radius);
  font-family: inherit; font-weight: var(--btn-font-weight); line-height: 1;
  text-decoration: none; cursor: pointer; user-select: none; white-space: nowrap;
  transition: all 0.2s ease;          /* ENFORCED */
  padding: 0 13px;                    /* md */
  font-size: 0.9375rem;               /* 15px, md */
  min-height: 32px;                   /* md */
  --btn-spinner: #fff;                /* spinner color; overridden on light variants */
}

/* ===== Interaction states (ENFORCED core preserved) ===== */
.btn:hover  { transform: translateY(-1px); filter: brightness(0.94); }  /* ENFORCED transform */
.btn:active { transform: translateY(1px);  filter: brightness(0.90); }  /* ENFORCED transform */
.btn:focus-visible { outline: 2px solid var(--btn-focus-ring); outline-offset: 2px; }
.btn:disabled, .btn[aria-disabled="true"] {
  opacity: 0.5;                       /* ENFORCED */
  cursor: not-allowed;                /* ENFORCED */
  transform: none; filter: none;
}

/* ===== Loading ===== */
.btn.loading {
  position: relative;                 /* ENFORCED */
  color: transparent;                 /* ENFORCED */
  pointer-events: none;
}
.btn.loading::after {
  content: ""; position: absolute; top: 50%; left: 50%;
  width: 1em; height: 1em; margin: -0.5em 0 0 -0.5em;
  border: 2px solid color-mix(in srgb, var(--btn-spinner) 35%, transparent);
  border-top-color: var(--btn-spinner); border-radius: 50%;
  animation: btn-spin 0.6s linear infinite;
}
@keyframes btn-spin { to { transform: rotate(360deg); } }

/* ===== Variants (solid) ===== */
.btn-primary   { background: var(--color-primary);          color: #fff; }
.btn-secondary { background: var(--color-secondary-violet); color: #fff; }
.btn-danger    { background: var(--color-danger);           color: #fff; }
.btn-success   { background: var(--color-success);          color: #fff; }
.btn-warning   { background: var(--color-secondary-sunray); color: var(--color-text); --btn-spinner: var(--color-text); }
.btn-accent    { background: var(--color-accent);           color: var(--color-text); --btn-spinner: var(--color-text); }

/* ===== Emphasis: outline ===== */
.btn-outline               { background: transparent; }
.btn-outline.btn-primary   { color: var(--color-primary);          border-color: var(--color-primary);          background: transparent; }
.btn-outline.btn-secondary { color: var(--color-secondary-violet); border-color: var(--color-secondary-violet); background: transparent; }
.btn-outline.btn-danger    { color: var(--color-danger);           border-color: var(--color-danger);           background: transparent; }
.btn-outline.btn-success   { color: var(--color-success);          border-color: var(--color-success);          background: transparent; }
.btn-outline:hover         { background: color-mix(in srgb, currentColor 10%, transparent); filter: none; }

/* ===== Emphasis: ghost ===== */
.btn-ghost        { background: transparent; border-color: transparent; color: var(--color-text); }
.btn-ghost:hover  { background: color-mix(in srgb, var(--color-text) 8%, transparent); filter: none; }

/* ===== Sizes ===== */
.btn-sm { padding: 0 10px; font-size: 0.75rem;   min-height: 27px; }   /* 12px */
.btn-md { padding: 0 13px; font-size: 0.9375rem; min-height: 32px; }   /* 15px (default) */
.btn-lg { padding: 0 24px; font-size: 1rem;      min-height: 48px; }   /* 16px */

/* ===== Modifiers ===== */
.btn-block       { display: flex; width: 100%; }
.btn-icon        { padding: 0; width: 32px; min-height: 32px; }  /* needs aria-label */
.btn-icon.btn-sm { width: 27px; min-height: 27px; }
.btn-icon.btn-lg { width: 48px; min-height: 48px; }

/* ===== Reduced motion (quality floor) ===== */
@media (prefers-reduced-motion: reduce) {
  .btn { transition: none; }
  .btn:hover, .btn:active { transform: none; }
  .btn.loading::after { animation-duration: 1.2s; }
}
```

---

## 11. Accessibility checklist

- [ ] Use a real `<button>` (or `<a>` for navigation) — **never** a `<div>`.
- [ ] Set `type="button"` unless it is a genuine form submit (`type="submit"`).
- [ ] Visible **`:focus-visible`** ring; never strip `outline` without a replacement.
- [ ] Every icon-only button has an **`aria-label`**.
- [ ] Don't convey state with **color alone** — pair with text/icon (especially Danger and Disabled).
- [ ] Meet **WCAG AA** contrast (§9). Light buttons (Warning, Accent) use **dark** text.
- [ ] Loading sets **`aria-busy`** and blocks re-trigger.
- [ ] Touch targets **≥ 44 × 44px**.
- [ ] **`prefers-reduced-motion`** respected.

---

## 12. Do's and Don'ts

| ✅ Do | ❌ Don't | Why |
|---|---|---|
| Use `btn-danger` (Violet-Red `#991547`) for Delete / Remove | Use `btn-accent` for destructive actions | Green Gold reads as a highlight, not a warning |
| Keep one solid button per group; make Cancel a `btn-ghost` | Put two filled buttons side by side | Competing emphasis hides the real action |
| Disable the button and show the spinner on submit | Leave it clickable while the request runs | Prevents double-submits |
| Let a **toast** confirm success and return the button to rest | Relabel the button to "Saved ✓" after saving | The toast already confirms it |
| Put **dark** text on Warning / Accent (light) buttons | Use white text on Sunray `#E9AC53` or Green Gold `#B3A125` | White fails WCAG contrast (2.0:1 / 2.6:1) |
| Label by outcome: "Save changes", "Delete file" | Use vague labels: "Submit", "OK", "Click here" | Specific labels tell users what will happen |
| Keep wording consistent: "Publish" → "Published" toast | Mix vocab: "Publish" → "Saved" toast | Consistent wording is how people learn the UI |
| Ship a visible `:focus-visible` ring | Set `outline: none` with no replacement | Keyboard users lose track of where they are |
| Use a real `<button>` / `<a>` | Attach `onClick` to a `<div>` / `<span>` | Non-interactive elements break keyboard + screen-reader use |
| Size for context: `btn-sm` in tables, `btn-lg` for mobile CTAs | Use `btn-sm` as the only tap target on mobile | Below the 44 × 44px touch minimum |
| Convey state with text/icon **and** color | Rely on color alone (e.g. red = danger) | Color-blind users can't read color-only meaning |
| On failure, reset to rest and show the error | Leave the button spinning after an error | Traps the user with no feedback or retry |

---

## 13. Rules for the AI assistant

**ALWAYS**
- Use design tokens (`var(--color-…)`); never hardcode a new hex or invent a shade.
- Choose the variant by **semantic meaning** (§3), not appearance.
- Keep exactly **one** solid/primary button per action group; the rest outline or ghost.
- Ship **hover, active, focus-visible, disabled, and loading** states on every button.
- Use **dark text** on Warning/Accent; **white text** on Primary/Secondary/Danger/Success.
- During async work: **disable + spinner + guard against double-submit**; surface errors elsewhere and reset on completion.
- Name buttons by what they do, and keep the verb consistent through the flow.

**NEVER**
- Use **Danger red** for a non-destructive action, or **Success green** for a generic submit (that's Primary).
- Use a filled color for **Cancel / Back** (use Ghost).
- Remove the focus outline without an equally visible replacement.
- Use a `<div>` (or click-handler on a non-interactive element) as a button.
- Rely on color alone to communicate meaning.
- Leave a button spinning with no success/error resolution.

---

## 14. Quick decision guide

```
What does this button do?
├─ Submits / saves / creates the main thing on screen ........ btn-primary (solid)
├─ A real alternative to the primary action ................. btn-secondary
├─ Deletes / removes / cannot be undone ..................... btn-danger
│     └─ want users to pause first? ......................... btn-danger btn-outline
├─ Caution, but not destructive (override / proceed anyway) . btn-warning  (dark text)
├─ Confirms a positive completion (approve / publish) ....... btn-success
├─ Upsell / highlight / spotlight ........................... btn-accent   (dark text)
└─ Cancel / Back / dismiss / tertiary ....................... btn-ghost

How big?
├─ Table row, toolbar, dense area ........................... btn-sm  (avoid as sole mobile tap target)
├─ Default form / most UI ................................... btn-md
└─ Hero, landing, empty state, mobile primary ............... btn-lg
```
