# Form & Input UI/UX Design Guide

Reference for AI-assisted development of forms, inputs, and validation. Values here (palette, fonts,
tokens) are approved constants, the same in every app; the canonical source is `design-tokens.md` (and
`../assets/`). Responsive is mobile-first; every token is defined in both a light and a dark theme with
a Light/Dark/System switcher.

> **TL;DR for the AI**
> 1. **Two sizing contexts** (§3): page form (roomy) and modal form (compact), plus three structural shapes: page, modal, and multi-step / wizard (§10). Sizing differs; validation states are identical (§6).
> 2. **Validation is ENFORCED** (§6-§7): inline on blur, error message below the field, and the submit button stays enabled - clicking it validates and jumps to the first invalid field.
> 3. **One column by default** (§9). Use 2-3 columns only with justification, and the column count a modal can hold depends on its size (420/600/900).
> 4. Many fields → group by category (§8): section headings / fieldsets, tabs (page or large modal), or steps (wizard, §10).
> 5. Every field needs a visible, associated `<label>`; placeholders are not labels.
> 6. Use the design tokens - never hardcode a hex or invent a shade outside the palette.

---

## Contents

1. [Design tokens (source of truth)](#1-design-tokens-source-of-truth)
2. [Anatomy of a field](#2-anatomy-of-a-field)
3. [Form types — page form vs modal form](#3-form-types--page-form-vs-modal-form)
4. [Field sizes (per form type)](#4-field-sizes-per-form-type)
5. [Input types](#5-input-types)
6. [Validation states (ENFORCED — same for both form types)](#6-validation-states-enforced--same-for-both-form-types)
7. [Validation behavior & timing (ENFORCED)](#7-validation-behavior--timing-enforced)
8. [Grouping fields by category](#8-grouping-fields-by-category)
9. [Layout — columns (PRINCIPLED)](#9-layout--columns-principled)
10. [Multi-step forms (wizards)](#10-multi-step-forms-wizards)
11. [Labels, help text, placeholders & tooltips](#11-labels-help-text-placeholders--tooltips)
12. [Full CSS reference (copy-paste ready)](#12-full-css-reference-copy-paste-ready)
13. [Full JS reference (validation contract)](#13-full-js-reference-validation-contract)
14. [Accessibility checklist](#14-accessibility-checklist)
15. [Do's and Don'ts](#15-dos-and-donts)
16. [Rules for the AI assistant](#16-rules-for-the-ai-assistant)
17. [Quick decision guide](#17-quick-decision-guide)

---

## 1. Design tokens (source of truth)

Complete two-theme set is canonical in `design-tokens.md`; the values below are the approved constants
forms consume. Buttons inside forms follow [buttons.md](buttons.md); forms inside modals follow
[modals-dialogs.md](modals-dialogs.md).

```css
:root {
    /* Brand palette (approved) */
    --color-primary: #193E6B;                 /* Midnight Blue */
    --color-accent: #B3A125;                  /* Green Gold */
    --color-secondary-violet: #7F3F98;        /* Cadmium Violet */
    --color-secondary-blue: #448E9D;          /* Jelly Bean Blue — non-interactive only */
    --color-secondary-sunray: #E9AC53;        /* Sunray */
    --color-success: #5F8025;        /* Avocado Green — success surface fills */
    --color-danger: #991547;         /* Violet-Red — danger surface fills */
    --color-background: #E8DFD0;     /* canvas; dark theme: #1A2E46 */
    --color-surface: #FFFFFF;        /* card; dark theme: #253E5D */
    --color-text: #1E2E42;           /* dark theme: #E9EFF6 */
    --color-text-muted: #4D5E75;     /* dark theme: #A3B2C5 */

    /* Theme-aware readable semantic tokens (--badge-*-fg pattern) — use these
       whenever a semantic hue is text, an icon, or a border on a surface;
       the raw semantic hex is not legible on both themes */
    --badge-danger-fg: #85113E;      /* dark theme: #F5B8CF */
    --badge-success-fg: #3A4E13;     /* dark theme: #CBE79B */

    /* Field tokens (shared) */
    --field-radius: 8px;
    --field-border: color-mix(in srgb, var(--color-text) 25%, transparent);
    --field-border-hover: color-mix(in srgb, var(--color-text) 40%, transparent);
    --field-focus-ring: var(--color-primary);   /* focus ring is always the primary interactive accent, both themes */
    --field-label-weight: 600;

    /* Page-form sizing (roomy) */
    --field-height-page: 32px;
    --field-font-page: 1rem;         /* 16px — avoids iOS zoom */
    --field-pad-page: 0 10px;        /* horizontal only — the 32px height centers content */
    --field-gap-page: 20px;          /* vertical gap between fields */
    --form-col-page: 640px;          /* single-column max-width */

    /* Modal-form sizing (compact) */
    --field-height-modal: 32px;
    --field-font-modal: 0.9375rem;   /* 15px */
    --field-pad-modal: 0 10px;
    --field-gap-modal: 14px;
}
```

---

## 2. Anatomy of a field

A field is **a label** + **the control** + **(optional) help text** + **a validation message slot**.

```
Label *                      ← visible <label for>, * marks required
┌─────────────────────────┐
│  control (input/select) │  ← the input itself
└─────────────────────────┘
Helper text or hint          ← optional, muted, BEFORE the user errs
⚠ Error message              ← appears BELOW the field on invalid (§6)
```

- **Label:** always visible, always associated (`<label for>` or wrapping). Mark required with `*` **and** `aria-required`.
- **Control:** input/select/textarea, sized per form type (§4).
- **Help text:** optional guidance shown up front (`aria-describedby`).
- **Message slot:** reserved space below the control so the layout doesn't jump when the error appears.

---

## 3. Form types — page form vs modal form

Two form contexts. They share validation, labelling, and grouping rules but are **sized differently**
because their containers differ.

| | **Page form** | **Modal form** |
|---|---|---|
| **Lives in** | A full page / route | Inside a [modal dialog](modals-dialogs.md) |
| **Space** | Generous | Constrained — body scrolls |
| **Field size** | **Roomy** (32px, 16px) — §4 | **Compact** (32px, 15px) — §4 |
| **Best for** | Long / important / multi-section forms | Short, focused create/edit (1–6 fields) |
| **Submit location** | Sticky footer or end-of-form action bar | Modal footer (one primary button) |
| **Class hook** | `form-page` | `form-modal` |

**Choosing:** long, central, or multi-category → page form. Short side-task that keeps context visible →
modal form (respect the modal size rules 420/600/900, modal guide §5). A modal form that needs to
exceed ~6-8 fields or multiple categories → move to a page form.

> Validation states, timing, labels, help text, and grouping are **identical** across both - only field sizing changes.

> **A third shape: the multi-step form (wizard).** Page and modal are *sizing* contexts; a wizard is a *structural* pattern hosting a long, sequential form across ordered steps (itself sized as page or modal). See §10.

---

## 4. Field sizes (per form type)

| Property | **Page form** (`form-page`) | **Modal form** (`form-modal`) | Why |
|---|---|---|---|
| Control height | **32px** | **32px** | Compact both contexts; touch raises to ≥44px (§12) |
| Font size | **16px** (`1rem`) | **15px** (`0.9375rem`) | 16px avoids mobile zoom on a standalone page |
| Padding | `0 10px` | `0 10px` | Horizontal only — the 32px height centers content |
| Vertical gap | **20px** | **14px** | Modals are tighter |
| Single-column max-width | **640px** | Inherits modal width (420/600/900) | Readable line length |
| Label size | 12px / 600 | 12px / 600 | One compact label size |

**Rules**
- On touch (aspect-ratio ≤ 1/1), **both** form types raise controls to **≥ 44px** (§12).
- Don't mix sizes within one form.
- Full-width controls within their column by default; constrain width only for a known short length (a 4-digit code, a postcode).

---

## 5. Input types

Use the right control; don't reinvent native ones.

| Data | Control | Notes |
|---|---|---|
| Short text | `<input type="text">` | Set `autocomplete`, `inputmode` where helpful |
| Email / tel / url / number | `type="email|tel|url|number"` | Correct mobile keyboard + native validation |
| Long text | `<textarea>` | Allow vertical resize; cap rows sensibly |
| One of few options | radio group **or** native `<select>` | ≤ 5 visible → radios; more → select |
| One of many options | `<select>` / combobox | Searchable combobox for long lists |
| Multiple options | checkboxes / multi-select | Checkboxes for ≤ ~7; multi-select beyond |
| Boolean | single checkbox or switch | Switch for instant-apply settings |
| Date / time | `type="date|time"` or a date picker | Always show the expected format |
| Password | `type="password"` + show/hide toggle | Toggle needs `aria-label`; never block paste |

> Whatever the control, it still gets a label, the §6 validation states, and the §4 size for its form type.

**When to use which control:**
- **Text vs textarea** - single line for names/codes/short values; `<textarea>` only when multi-line is genuinely expected.
- **Radios vs select** - ≤ 5 exclusive options worth comparing at a glance → radios; more, or tight space → `<select>`; long lists → searchable combobox.
- **Checkboxes vs multi-select** - a handful of independent on/off choices seen together → checkboxes; many → multi-select.
- **Single checkbox vs switch** - checkbox for consent applied **on submit**; switch for a setting that takes effect **instantly**.
- **Number vs stepper vs slider** - free numeric → `type="number"` + `inputmode="numeric"`; small bounded counts → stepper; fuzzy ranges → slider.
- **Native date vs custom picker** - `type="date"` for a single date; a custom picker only for ranges or constrained availability; always surface the expected format.
- **One field vs split fields** - one field unless parts are validated/used separately (card expiry MM / YY, phone country code).

---

## 6. Validation states (ENFORCED — same for both form types)

Identical in page and modal forms; only sizing differs. Every field implements them.

| State | Trigger | Appearance | A11y |
|---|---|---|---|
| **Default / Rest** | — | Neutral border (`--field-border`) | — |
| **Hover** | Pointer over | Border darkens (`--field-border-hover`) | — |
| **Focus** | Keyboard/click focus | **Visible focus ring** (`--field-focus-ring`), 2px | Mandatory — never remove without replacement |
| **Filled** | Has a value | Same as rest, with text color | — |
| **Disabled** | Not editable now | `opacity: 0.5`, `not-allowed`, muted | `disabled` |
| **Read-only** | Shown but not editable | Muted fill, no edit focus ring | `readonly` |
| **Error / Invalid** | Failed validation (§7) | **Danger** border + danger message **below** + `⚠` icon | `aria-invalid="true"`, `aria-describedby` → message id |

**Rules**
- **Validation is error-only.** Show feedback only when a field is invalid - no green border, `✓`, or "Looks good" on valid fields. A passing field returns to rest.
- **Error message goes BELOW the field** (ENFORCED), in danger color, tied via `aria-describedby`.
- **Error text and border use the theme-aware readable danger token** (`--badge-danger-fg`), never raw Violet-Red `#991547`.
- **Never rely on color alone.** Pair the error red with an icon (`⚠`) and text.
- **Reserve the message slot** so the form doesn't jump when an error appears/disappears.

---

## 7. Validation behavior & timing (ENFORCED)

The authoritative form-validation contract for *when* validation runs.

1. **Validate inline on blur.** Show any error immediately below the field. Don't wait for submit.
2. **Error message below the field** (§6).
3. **Submit stays enabled.** Never grey out the primary button. Clicking it runs full validation; if anything is invalid, submit is blocked and the first error is revealed (rule 5).
4. **Re-validate on input *after* a field has errored** so the error clears the moment it's fixed - the field returns to rest (no success styling).
5. **On submit:** if anything is still invalid, focus and scroll to the **first** invalid field and announce it.
6. **Async/server errors** map back to the relevant field (or a form-level summary); never swallow them silently.
7. **Don't validate required-ness on first focus.** Empty-but-untouched is not an error yet - flag only on blur or submit.

> **Why submit stays enabled:** a permanently greyed-out submit with no explanation frustrates users. An enabled submit that validates on click and focuses the first invalid field explains itself. Matches [buttons.md](buttons.md) §6.

### Timeline

```
field focus → (type) → blur ─valid──▶ rest (no success styling)
                          │
                          └─invalid─▶ error below field
                                         │ (user edits)
                                         └─on input─▶ re-validate ─▶ clears to rest when fixed
submit click ─any invalid─▶ focus first invalid field + announce
submit click ─all valid───▶ submits (the button is never disabled by validation)
```

---

## 8. Grouping fields by category

When a form has many fields, group them by category instead of a single long wall of inputs.

**When to group:** more than **~8 fields**, or fields in clear real-world categories → group. Fewer, or
one obvious set → a single ungrouped column.

**How to group:**
- Use a `<fieldset>` with a `<legend>`, or a section with a heading, per category (*Personal details*, *Contact*, *Billing address*, *Preferences*).
- Order by priority and logic: what the user knows first comes first; required before optional.
- Keep each group short; a group that grows large may deserve its own step or page.
- One **primary action** for the whole form, not per group (unless groups are independently saved).
- For very long page forms, consider a stepper/wizard or in-page section nav.

```html
<form class="form form-page">
  <fieldset class="field-group">
    <legend class="field-group-title">Personal details</legend>
    <!-- first name, last name, date of birth … -->
  </fieldset>
  <fieldset class="field-group">
    <legend class="field-group-title">Contact</legend>
    <!-- email, phone … -->
  </fieldset>
  <div class="form-actions">
    <button type="button" class="btn btn-ghost">Cancel</button>
    <button type="submit" class="btn btn-primary">Save</button>
  </div>
</form>
```

> Grouping is independent of form type. In a modal, heavy grouping is a signal you may have outgrown the modal and should move to a page (§3).

### Three grouping mechanisms — headings, tabs, steps

| Mechanism | Use in | Pattern |
|---|---|---|
| **Section heading / `<fieldset>`** | Page forms; short modals | All categories visible at once; user scrolls between them |
| **Tabs** | **Page forms _and_ large modals** | One category per tab; everything stays on one screen / height |
| **Steps (wizard)** | Long or sequential flows (page or short modal) | One category per step; **Back / Next** (§10) |

Pick by length and flow: a few categories visible at once → headings; many to switch between without
scrolling → tabs; a long ordered flow → steps.

**Tabs** work identically in a large modal (e.g. *Basic Info · Contact & Address · Professional · CRM &
Owner · Identity / Compliance · Marketing*) and in a long page-form record (e.g. *Profile · Security ·
Notifications · Billing*), using the same markup:

```html
<!-- Identical markup works in a modal OR a page form -->
<div class="modal-form-tabs" role="tablist">
  <button type="button" class="modal-form-tab active" data-ftab="cBasic">Basic Info</button>
  <button type="button" class="modal-form-tab" data-ftab="cContact">Contact &amp; Address</button>
  <button type="button" class="modal-form-tab" data-ftab="cProfessional">Professional</button>
</div>
<div class="form-grid">
  <div class="modal-form-section active" data-fsection="cBasic">
    <div class="form-section-heading">Personal Details</div>
    <!-- fields … -->
  </div>
  <!-- other category sections … -->
</div>
```

**Rules for tabs:** keep **required fields in the first tab** so they're never hidden behind a click;
order tabs by likelihood of use; still use section headings *inside* a tab that holds many fields; the
form's **one** submit validates across **all** tabs and, on failure, switches to the tab holding the
first invalid field.

---

## 9. Layout — columns (PRINCIPLED)

**Default: single column** - the most accessible and scannable, works on every screen.

- Single column is the default for both form types.
- **Multi-column only with justification:** 10+ related fields, settings pages, or naturally paired fields (First/Last name, City/Postcode). Keep related fields together; never split a single logical field across columns.
- **Never** create a multi-column layout that reorders the tab sequence illogically - tab order follows visual order.
- On touch (aspect-ratio ≤ 1/1), **collapse multi-column to single column** (§12).
- Keep line length readable: single column caps at `--form-col-page` (640px) on a page; in a modal it inherits the modal width.

### Choosing the column count

| Columns | When | Field min-width |
|---|---|---|
| **1 (default)** | Most forms; ≤ ~8 fields; mobile / tall screens | full width |
| **2** | 8+ related fields, settings pages, naturally paired fields | ~220px each |
| **3** | Dense entry of **short** fields (codes, dates, numbers) in a wide page or large modal | ~200px each |

**In a modal, the column count is bound by the modal size** ([modal guide](modals-dialogs.md) §5):

| Modal size | Max columns |
|---|---|
| Small (420px) | 1 |
| Medium (600px) | 1–2 |
| Large (900px) | 2–3 |

```html
<!-- Justified pairing (two-col); a three-col row shares three short fields (code/date/qty) in a wide container -->
<div class="field-row two-col">
  <div class="field"><label for="first">First name</label><input id="first"></div>
  <div class="field"><label for="last">Last name</label><input id="last"></div>
</div>
```

All multi-column rows **collapse to a single column** on touch / tall screens (§12).

---

## 10. Multi-step forms (wizards)

When a form is long and naturally sequential (onboarding, multi-entity create, guided setup), split it
into ordered steps. A wizard is categorization spread across screens: each step is one category.

**Use a wizard when:** steps are dependent or order matters, the flow is long, or you want to reduce
perceived effort. **Don't** when a single categorized page (§8) would do.

### Structure

```
●━━━━━●━━━━━○━━━━━○
Account  Profile  Billing  Review
 (done)  (active)
```

- A **step indicator** showing all steps, which are done, and which is current (`aria-current="step"`).
- **One step's fields visible at a time**, sized for its host (page = roomy, modal = compact, §4).
- **Exactly two navigation buttons: Back and Next.** Back is hidden/disabled on the first step. On the **last step, Next becomes the completion action** (*Create* / *Save* / *Finish*) and submits - the **same primary button changing its label**, never a third button.
- An optional **Review** step summarizing entries before final submit.

```html
<form class="form form-page wizard" novalidate>
  <ol class="wizard-steps" aria-label="Progress">
    <li class="step done">Account</li>
    <li class="step active" aria-current="step">Profile</li>
    <li class="step">Billing</li>
    <li class="step">Review</li>
  </ol>
  <section class="wizard-panel" data-step="profile">
    <!-- this step's fields (one category) -->
  </section>
  <!-- Footer: right-aligned group, one solid button. Back hidden on step 1;
       Next morphs to "Create account" + type=submit on the last step. -->
  <div class="form-actions">
    <button type="button" class="btn btn-outline btn-secondary" data-wiz="back" hidden>Back</button>
    <button type="submit" class="btn btn-primary" data-wiz="next">Next</button>
  </div>
</form>
```

### Rules
- **Validate the current step before advancing** - Next stays enabled; clicking it validates with the same states/timing as §6-§7 and advances only once valid.
- **Never lose entered data on Back** - keep every step's values in state.
- **One category per step**; keep steps short.
- **Two buttons, paired and placed per [buttons.md](buttons.md):** the same right-aligned footer group as any form - low-emphasis **Back** (`btn-outline btn-secondary`, hidden/disabled on step 1) immediately left of the one solid **Next** (`btn-primary`), never split to opposite edges. No separate persistent submit - on the last step Next's label becomes Create / Save / Finish and it submits. **Exactly one solid button per step.**
- A wizard can be a **page** (preferred for long flows) or a **modal** (only for short 2-3 step flows - never stack modals, per [modal guide](modals-dialogs.md)).
- Show progress and let users return to completed steps without redoing them where safe.

```css
.wizard-steps { display: flex; list-style: none; padding: 0; margin: 0 0 28px; counter-reset: wizstep; }
.wizard-steps .step {
  flex: 1; position: relative; display: flex; flex-direction: column; align-items: center; gap: 8px;
  font-size: 0.8rem; font-weight: 600; color: var(--color-text-muted); counter-increment: wizstep;
}
.wizard-steps .step::before {                         /* numbered circle */
  content: counter(wizstep);
  position: relative; z-index: 1;
  width: 32px; height: 32px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  background: var(--color-surface); border: 2px solid var(--field-border);
  color: var(--color-text-muted); font-weight: 700; font-size: 0.85rem;
  transition: background 0.15s, border-color 0.15s, color 0.15s;
}
.wizard-steps .step:not(:first-child)::after {        /* connector to the previous step */
  content: ""; position: absolute; top: 15px; left: -50%; right: 50%;
  height: 2px; background: var(--field-border); z-index: 0;
}
.wizard-steps .step.active { color: var(--color-primary); font-weight: 700; }
.wizard-steps .step.active::before { background: var(--color-primary); border-color: var(--color-primary); color: #fff; }
.wizard-steps .step.active::after { background: var(--color-primary); }
.wizard-steps .step.done { color: var(--badge-success-fg); }   /* readable success text, both themes */
.wizard-steps .step.done::before { background: var(--color-success); border-color: var(--color-success); color: #fff; }
.wizard-steps .step.done::after { background: var(--color-success); }
.wizard-panel { animation: fadeIn 0.15s ease; }
@media (max-aspect-ratio: 1/1) {
  .wizard-steps .step { font-size: 0.7rem; }   /* condensed labels on tall screens */
}
```

### The two-button navigation model

Only **Back** and **Next** appear, in the same right-aligned footer group as any form (Back immediately
left of the primary, not split to edges). The primary button is **one element** throughout; on the last
step its label and behaviour change:

```
Step 1:                                 [ Next → ]          ← Back hidden on step 1
Step 2:                       [ ← Back ] [ Next → ]
Last:                  [ ← Back ] [ Create account ]        ← SAME button, new label, now submits
```

```js
function renderWizardNav(current, lastIndex) {
  back.hidden = current === 0;                              // no Back on step 1
  const onLast = current === lastIndex;
  next.textContent = onLast ? 'Create account' : 'Next';   // or 'Save' / 'Finish'
  next.type = onLast ? 'submit' : 'button';                // becomes the submit on the last step
}

next.addEventListener('click', (e) => {
  if (!validateStep(current)) { e.preventDefault(); return; }      // gate advance AND final submit
  if (current < lastIndex) { e.preventDefault(); current++; renderStep(); renderWizardNav(current, lastIndex); }
  // on the last step Next is type="submit", so the form submits normally
});
```

### Wizard in a modal

Short flows (2-3 steps) only. Same two-button model and all modal behaviour from [modal guide](modals-dialogs.md).

- **Step indicator** at the top of the modal body; **one step's fields** in the body; **Back / Next** in the **modal footer** (Next → *Finish* on the last step).
- Use **compact** field sizing (`form-modal`, §4).
- **ESC / backdrop / ×** still close - but if the user entered data, guard the close with a discard prompt (modal guide §8).
- **Never stack modals** and don't open sub-dialogs from a step. Long flows (4+ steps) belong on a **page**.

```html
<div class="modal-backdrop" id="signupWizard" hidden>
  <div class="modal modal-md" role="dialog" aria-modal="true" aria-labelledby="sw-title">
    <div class="modal-header">
      <h2 class="modal-title" id="sw-title">Create account</h2>
      <button class="btn btn-ghost btn-icon" aria-label="Close" data-close>&times;</button>
    </div>
    <div class="modal-body">
      <ol class="wizard-steps" aria-label="Progress"> <!-- steps --> </ol>
      <form class="form form-modal wizard" id="swForm" novalidate>
        <section class="wizard-panel" data-step="account"> <!-- fields --> </section>
      </form>
    </div>
    <div class="modal-footer">
      <button type="button" class="btn btn-outline btn-secondary" data-wiz="back" hidden>Back</button>
      <button type="submit" form="swForm" class="btn btn-primary" data-wiz="next">Next</button>
    </div>
  </div>
</div>
```

### All form cases at a glance

Sizing follows the host (page = roomy, modal = compact, §4); validation is identical everywhere (§6-§7);
columns collapse to 1 on touch/tall screens; required fields always sit in the first category / tab / step.

| # | Shape | Categorization | Columns | Navigation / submit |
|---|---|---|---|---|
| 1 | **Page form** | None (≤ ~8 fields) | 1 | Single submit (Save / Create) |
| 2 | Page form | Section headings | 1–3 | Single submit |
| 3 | Page form | **Tabs** | 1–3 per tab | Single submit (validates across tabs) |
| 4 | **Modal form** | None (≤ 6–8 fields) | 1 (sm) · 1–2 (md) | Footer submit |
| 5 | Modal form | Section headings | 1–2 | Footer submit |
| 6 | Modal form (large) | **Tabs** | 2–3 per tab | Footer submit (validates across tabs) |
| 7 | **Multi-step — page** | One category **per step** | 1–3 per step | **Back / Next** → Next becomes Create/Save/Finish on the last step |
| 8 | **Multi-step — modal** | One category **per step** (2–3 steps) | 1–2 per step | **Back / Next** in footer → Finish on the last step |

---

## 11. Labels, help text, placeholders & tooltips

### Labels & required state
- **Every field has a visible `<label>`** associated via `for`/`id`. Placeholders are **not** labels.
- **Mark required fields** with `*` in the label and `aria-required="true"`; or mark optional fields "(optional)" - pick one convention per form.
- **Label by outcome** and keep copy consistent with the flow and the submit button.

### Help text
- **Persistent guidance** (format, why you need it, constraints) sits under the label/control as muted **help text** (`.help`) and stays visible.
- **Help text before the error.** On failure the error joins/replaces it in the message slot (§6); reserve the slot.
- Use help text - **not** a placeholder or tooltip - for anything a user *must* know to fill the field correctly.

### Placeholder text
A placeholder is a **faint example of a valid value**, shown only while the field is empty. It vanishes
on input, so it can never carry meaning the user still needs.

**Use it to** show the **expected format** (`name@company.com`, `+1 555 0100`, `DD/MM/YYYY`, `SG-ENT-001`)
or hint **what kind of value** goes in (`Search contacts…`, `0.00`).

**Never** use it to replace the label, hold instructions/constraints ("Must be 8+ characters" is help
text), or state required-ness / validation rules.

**Rules:** keep it short and literal; no trailing punctuation except the ellipsis on search/filter
inputs; make it visibly dimmer than real input but legible (muted); **selects take no placeholder** -
use a disabled, non-selectable first option ("Select a stage…"); many short fields need no placeholder
at all.

| Field | Good placeholder | Why |
|---|---|---|
| Email | `name@company.com` | Shows the format |
| Phone | `+1 555 0100` | Format + country hint |
| Date (typed) | `DD/MM/YYYY` | Clarifies field order |
| Amount | `0.00` | Shows decimals |
| Search | `Search contacts…` | States the action |
| First name | *(none)* | The label is enough |

### Tooltips — only when necessary
Most fields don't need a tooltip; reach for visible help text first. A tooltip carries **secondary,
contextual** detail that would clutter the form if always shown.

**Use a tooltip (info `ⓘ` next to the label) for:** a definition of jargon/term of art; **why** a
non-obvious field is collected, or **where to find** a value; **explaining a disabled control** (pair
the disabled state with a tooltip/text saying what unlocks it, [buttons.md](buttons.md) §6).

**Never** put critical or required information only in a tooltip - hover/focus content is easy to miss and awkward on touch.

**Rules:** the trigger is a **focusable** `<button>` with an `aria-label`; link the content via
`aria-describedby`; it opens on **focus and hover** (not hover-only) and dismisses on `Esc`/blur (touch
users tap to toggle); keep it to a sentence or two.

```html
<div class="field">
  <label for="stage">
    Lifecycle stage
    <button type="button" class="field-info" aria-label="What is a lifecycle stage?"
            aria-describedby="stage-tip">ⓘ</button>
  </label>
  <select id="stage" aria-describedby="stage-tip"> <!-- options --> </select>
  <span class="help" id="stage-tip" role="tooltip">Where this contact sits in your funnel: Prospect → Lead → Customer.</span>
</div>
```

---

## 12. Full CSS reference (copy-paste ready)

Pairs with the button CSS ([buttons.md](buttons.md) §10). The two form types are driven by
`.form-page` / `.form-modal` wrapper classes that swap the size tokens.

```css
/* ===== Form wrappers select the size set ===== */
.form { display: flex; flex-direction: column; }
.form-page  { --fh: var(--field-height-page);  --ff: var(--field-font-page);  --fp: var(--field-pad-page);  --fg: var(--field-gap-page);  max-width: var(--form-col-page); }
.form-modal { --fh: var(--field-height-modal); --ff: var(--field-font-modal); --fp: var(--field-pad-modal); --fg: var(--field-gap-modal); }

/* ===== Field block ===== */
.field { display: flex; flex-direction: column; gap: 6px; margin-bottom: var(--fg); }
.field > label {
  font-weight: var(--field-label-weight);
  font-size: 0.75rem;                     /* 12px — labels, both contexts */
  color: var(--color-text);
}
.field .req { color: var(--badge-danger-fg); margin-left: 2px; }   /* readable danger, both themes */

/* ===== Controls ===== */
.field input,
.field select,
.field textarea {
  width: 100%;
  min-height: var(--fh);
  padding: var(--fp);
  font: inherit;
  font-size: var(--ff);
  color: var(--color-text);
  background: var(--color-surface);
  border: 1px solid var(--field-border);
  border-radius: var(--field-radius);
  transition: border-color 0.15s ease, box-shadow 0.15s ease;
}
.field textarea { min-height: calc(var(--fh) * 2); resize: vertical; }

.field input:hover,
.field select:hover,
.field textarea:hover { border-color: var(--field-border-hover); }

.field input:focus-visible,
.field select:focus-visible,
.field textarea:focus-visible {
  outline: none;
  border-color: var(--field-focus-ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--field-focus-ring) 30%, transparent);
}

/* ===== Disabled / read-only ===== */
.field input:disabled,
.field select:disabled,
.field textarea:disabled { opacity: 0.5; cursor: not-allowed; }
.field input[readonly],
.field textarea[readonly] { background: var(--color-background); }

/* ===== Help + message slot ===== */
.field .help { font-size: 0.8125rem; color: var(--color-text-muted); }
.field .msg  { font-size: 11.5px; min-height: 1.1em; display: flex; align-items: center; gap: 6px; }

/* ===== Tooltip trigger (info icon by the label, §11) ===== */
.field-info {
  border: none; background: none; cursor: help; padding: 0 0 0 4px;
  color: var(--color-text-muted); font-size: 0.85em; line-height: 1;
}
.field-info:focus-visible { outline: 2px solid var(--field-focus-ring); outline-offset: 2px; border-radius: 50%; }

/* ===== Validation state (error-only; valid fields return to rest) =====
   Error text + border use --badge-danger-fg (not raw #991547) so they stay legible in dark mode. */
.field.is-error input,
.field.is-error select,
.field.is-error textarea { border-color: var(--badge-danger-fg); }
.field.is-error input:focus-visible { box-shadow: 0 0 0 3px color-mix(in srgb, var(--badge-danger-fg) 25%, transparent); }
.field.is-error .msg { color: var(--badge-danger-fg); }

/* ===== Grouping ===== */
.field-group { border: none; padding: 0; margin: 0 0 var(--spacing-xl, 32px); }
.field-group-title {
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--color-primary);
  padding-bottom: 8px;
  margin-bottom: 16px;
  border-bottom: 1px solid color-mix(in srgb, var(--color-text) 10%, transparent);
  width: 100%;
}

/* ===== Columns (PRINCIPLED) ===== */
.field-row { display: grid; gap: 16px; }
.field-row.two-col   { grid-template-columns: 1fr 1fr; }
.field-row.three-col { grid-template-columns: 1fr 1fr 1fr; }
.field-row .field { margin-bottom: var(--fg); }
.field-row .span-all { grid-column: 1 / -1; }   /* long field spans the row */

/* ===== Categorization: responsive field grid + section headings + modal tabs ===== */
.form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); column-gap: 16px; }
.modal-form-tabs { display: flex; flex-wrap: wrap; gap: 0; border-bottom: 2px solid var(--field-border); margin-bottom: 20px; }
.modal-form-tab {
  border: none; background: none;          /* strip native <button> chrome */
  padding: 9px 18px; font: inherit; font-size: 0.8rem; font-weight: 600; color: var(--color-text-muted);
  cursor: pointer; border-bottom: 3px solid transparent; margin-bottom: -2px; white-space: nowrap;
  transition: color 0.15s, border-color 0.15s;
}
.modal-form-tab.active { color: var(--color-primary); border-bottom-color: var(--color-primary); }
.modal-form-tab:hover:not(.active) { color: var(--color-text); }
.modal-form-tab:focus-visible { outline: 2px solid var(--field-focus-ring); outline-offset: -2px; }
.modal-form-section { display: none; }
.modal-form-section.active { display: contents; }
.form-section-heading {
  grid-column: 1 / -1;
  font-size: 0.7rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.07em;
  color: var(--color-text-muted); margin: 12px 0 2px; padding-bottom: 6px;
  border-bottom: 1px solid var(--field-border);
}

/* ===== Actions ===== */
.form-actions { display: flex; justify-content: flex-end; gap: 10px; margin-top: 8px; }

/* ===== Responsive (mobile-first; width and/or aspect-ratio breakpoints) ===== */
@media (max-aspect-ratio: 1/1) {
  .form-page, .form-modal { --fh: 44px; }      /* touch target */
  .field-row.two-col { grid-template-columns: 1fr; }  /* collapse to single column */
  .field-row.three-col { grid-template-columns: 1fr; }
  .form-page { max-width: 100%; }
  .form-actions { flex-direction: column-reverse; }
  .form-actions .btn { width: 100%; }
}
```

---

## 13. Full JS reference (validation contract)

Dependency-free implementation of §7 (blur validation, error below field, submit-click validation that
focuses the first invalid field). Frameworks should use their own form library - the contract is identical.

```js
function initForm(form) {
  const fields = [...form.querySelectorAll('.field')];

  function validateField(field, { live = false } = {}) {
    const input = field.querySelector('input, select, textarea');
    const msg = field.querySelector('.msg');
    let error = '';

    if (input.required && !input.value.trim()) error = 'This field is required.';
    else if (input.value && !input.checkValidity()) error = input.validationMessage;

    field.classList.toggle('is-error', !!error);   // error-only — no is-valid state
    input.setAttribute('aria-invalid', error ? 'true' : 'false');
    if (msg) msg.textContent = error ? '⚠ ' + error : '';

    // Once errored, re-validate live so it clears as soon as it's fixed (§7.4)
    if (error && !live) {
      input.addEventListener('input', () => validateField(field, { live: true }), { once: false });
    }
    return !error;
  }

  fields.forEach(field => {
    const input = field.querySelector('input, select, textarea');
    input.addEventListener('blur', () => validateField(field));   // §7.1
  });

  // §7.3 — submit stays enabled; the click runs full validation
  form.addEventListener('submit', (e) => {
    let firstInvalid = null;
    fields.forEach(field => { if (!validateField(field) && !firstInvalid) firstInvalid = field; });
    if (firstInvalid) {                                   // §7.5
      e.preventDefault();
      const input = firstInvalid.querySelector('input, select, textarea');
      input.focus();
      firstInvalid.scrollIntoView({ behavior: 'smooth', block: 'center' });
    }
  });
}

document.querySelectorAll('.form').forEach(initForm);
```

---

## 14. Accessibility checklist

- [ ] Every control has a **visible, associated `<label>`** (`for`/`id`).
- [ ] Required fields marked with `*` **and** `aria-required="true"`.
- [ ] Errors use `aria-invalid="true"` and `aria-describedby` → the message id; message sits **below** the field.
- [ ] Don't signal validity by color alone - pair with icon + text.
- [ ] Visible **`:focus-visible`** ring on every control.
- [ ] Help/placeholder text is not the only source of the field's meaning.
- [ ] Tab order follows visual order (especially in multi-column).
- [ ] Touch targets ≥ 44 × 44px on touch (aspect-ratio ≤ 1/1).
- [ ] Don't block paste (passwords, codes); allow native autofill (`autocomplete`).
- [ ] Error summary on submit focuses/scrolls to the first invalid field.

---

## 15. Do's and Don'ts

| ✅ Do | ❌ Don't | Why |
|---|---|---|
| Use `form-page` sizing on pages, `form-modal` sizing in modals | One fixed field size everywhere | Contexts have different space |
| Keep validation states identical across both form types | Style modal errors differently from page errors | Consistency is how users learn the UI |
| Validate on blur and show the error **below** the field | Wait until submit to reveal the first error | Late errors force rework |
| Re-validate on input once a field has errored | Make users blur again to clear a fixed error | Instant clearing rewards the fix |
| Group many fields into labelled categories | Present 15 inputs as one undivided wall | Grouping makes long forms manageable |
| Keep a visible `<label>` on every field | Use a placeholder as the label | Placeholders vanish on typing; fail a11y |
| Default to a single column | Force multi-column for unrelated fields | Single column is most scannable/accessible |
| Pair error color with icon + text | Signal errors with red border only | Color-blind users miss color-only state |
| Reserve space for the message | Let the layout jump when an error appears | Stable layout is calmer |
| Move an overgrown modal form to a page | Cram 12 fields + categories into a modal | Modals are for short, focused tasks |

---

## 16. Rules for the AI assistant

**ALWAYS**
- Pick the form type by context: `form-page` standalone, `form-modal` inside a [modal](modals-dialogs.md); use a **multi-step wizard** (§10) for long, sequential flows.
- Apply the matching field sizes (§4): page = 32px/16px, modal = 32px/15px - one size set per form.
- Implement the same validation states (§6) and timing (§7) regardless of type: blur validation, error below field, submit enabled - on click validate and focus the first invalid field; in a wizard, validate each step before advancing.
- Give every field a visible, associated label; mark required state accessibly.
- **Group many fields by category** (§8): section headings/fieldsets, tabs (page forms _or_ large modals), or steps (wizard). Default to a single column; use 2-3 columns only with justification and within the modal-size limit (§9).
- In a **multi-step form** use **exactly two buttons** (§10): **Back** (hidden/disabled on step 1) and **Next**; on the last step Next's label becomes Create / Save / Finish and it submits - never add a separate persistent submit. Pair and place them per [buttons.md](buttons.md): a right-aligned footer group with **Back** (`btn-outline btn-secondary`) immediately left of the one solid **Next** (`btn-primary`). Holds for both page and modal wizards.
- **Choose the control by the data** (§5): radios for ≤ 5 exclusive options, select/combobox beyond; checkbox on submit vs switch for instant settings; textarea only for true multi-line.
- Treat placeholders as faint examples (format hints), never labels or instructions; put must-know detail in help text, reserve tooltips for secondary/contextual info (§11).
- Use design tokens; respect mobile-first responsive rules and 44px touch targets.

**NEVER**
- Use a placeholder as the only label, or to carry instructions/constraints.
- Put critical or required information only in a tooltip.
- Signal validity by color alone.
- Reveal the first error only at submit time.
- Cram a long, multi-category form into a modal - move it to a page.
- Remove the focus ring without an equally visible replacement.
- Hardcode colors or invent shades outside the palette.

---

## 17. Quick decision guide

```
Which form shape?
├─ Its own page / route ......................... form-page  (32px fields, 16px, max 640px)
├─ Inside a modal dialog ........................ form-modal (32px fields, 15px, inherits modal width)
│     └─ growing past ~6–8 fields or multiple categories? → tabs, or move it to a page
└─ Long & sequential / dependent steps .......... multi-step wizard (validate each step, §10)

How many fields?
├─ A handful, one obvious set ................... single ungrouped column
└─ Many (~8+) or clear categories .............. group → headings/fieldsets · tabs (page or large modal) · steps (wizard)

Columns?
├─ Default ...................................... 1 column
├─ 8+ related / settings / paired ............... 2 columns (justify)
└─ Dense short fields, wide page / large modal .. 3 columns (justify)
      modal cap: 420px→1 · 600px→1–2 · 900px→2–3 · all collapse to 1 on touch

Validation (same for ALL shapes):
├─ On blur ...................................... validate + show error BELOW the field
├─ After it errors ............................. re-validate on input, clear when fixed
├─ Submit button ............................... stays enabled — clicking it validates the form (or step)
└─ On submit if invalid ........................ focus + scroll to first invalid field

Multi-step nav (page or modal):
└─ Two buttons only ............................ Back (hidden/disabled on step 1) + Next
                                                  last step: Next becomes Create/Save/Finish (submit)

Every field: visible <label> · required marked accessibly · color never the only signal · :focus-visible ring
```
