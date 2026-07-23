# Modal & Dialog UI/UX Design Guide

Part of the `ui-ux-design` skill. Values here (palette, fonts, tokens) are approved CLaaS2SaaS
constants, not per-project choices; the canonical source is `design-tokens.md` and `../assets/`.
Responsive is mobile-first; every token is defined in both the light and dark theme with a
Light/Dark/System switcher.

> **TL;DR for the AI**
> 1. A modal **interrupts** - use it only when the task must be handled now or kept in context. Otherwise use a page, drawer, or inline UI.
> 2. **Behavior is ENFORCED** (§6): ESC closes, backdrop click closes, focus is **trapped**, and focus **returns to the trigger** on close.
> 3. **Pick size by content** (§5): Small 420px (confirmations), Medium 600px (standard forms), Large 900px (complex data entry).
> 4. **Destructive confirmations** (§7) name the exact thing, use a **Danger** button, and never default-focus the destructive action.
> 5. **Discard confirmations** (§8) only appear when there are **unsaved changes**.
> 6. Always ship **labelled** dialogs (`role="dialog"` + `aria-modal="true"` + `aria-labelledby`), a scroll lock, and a visible close affordance.
> 7. **Optional (§16):** a long form *may* be split into segments with an **in-header tab strip** - build it only if the developer wants it (default: a plain form modal, or a page). When used: only the body scrolls, required fields go in the **first** tab, and a failed save jumps to the first tab with an error.

---

## Contents

1. [Design tokens (source of truth)](#1-design-tokens-source-of-truth)
2. [When to use a modal (and when not to)](#2-when-to-use-a-modal-and-when-not-to)
3. [Anatomy of a modal](#3-anatomy-of-a-modal)
4. [Modal types](#4-modal-types)
5. [Sizes — principled, driven by content](#5-sizes--principled-driven-by-content)
6. [Behavior rules (ENFORCED)](#6-behavior-rules-enforced)
7. [Delete / destructive confirmations](#7-delete--destructive-confirmations)
8. [Discard / unsaved-changes confirmations](#8-discard--unsaved-changes-confirmations)
9. [Focus management & accessibility](#9-focus-management--accessibility)
10. [Copy / labels](#10-copy--labels)
11. [Full CSS reference (copy-paste ready)](#11-full-css-reference-copy-paste-ready)
12. [Full JS reference (behavior contract)](#12-full-js-reference-behavior-contract)
13. [Accessibility checklist](#13-accessibility-checklist)
14. [Do's and Don'ts](#14-dos-and-donts)
15. [Rules for the AI assistant](#15-rules-for-the-ai-assistant)
16. [Segmented / tabbed (multi-section) form modal](#16-segmented--tabbed-multi-section-form-modal)
17. [Quick decision guide](#17-quick-decision-guide)

---

## 1. Design tokens (source of truth)

Modals consume the brand palette and spacing scale; introduce no colors outside it. The complete
two-theme set is canonical in `design-tokens.md` (dark-theme values in the comments below). Buttons
inside a modal follow [buttons.md](buttons.md).

```css
:root {
    /* Brand palette (approved) */
    --color-primary: #193E6B;           /* Midnight Blue */
    --color-accent: #B3A125;            /* Green Gold */
    --color-secondary-violet: #7F3F98;  /* Cadmium Violet */
    --color-secondary-blue: #448E9D;    /* Jelly Bean Blue */
    --color-secondary-sunray: #E9AC53;  /* Sunray */
    --color-success: #5F8025;           /* Avocado Green */
    --color-danger: #991547;            /* Violet-Red */
    --color-background: #E8DFD0;        /* canvas — dark theme: #1A2E46 */
    --color-surface: #FFFFFF;           /* card — dark theme: #253E5D */
    --color-text: #1E2E42;              /* dark theme: #E9EFF6 */
    --color-text-muted: #4D5E75;        /* dark theme: #A3B2C5 */

    /* Modal tokens */
    --modal-backdrop: rgba(25, 30, 40, 0.55);
    --modal-backdrop-blur: 4px;         /* the backdrop dims AND blurs the page behind */
    --modal-radius: 14px;
    --modal-shadow: 0 8px 24px rgba(15, 25, 45, 0.14);  /* overlay shadow — dark theme: 0 10px 28px rgba(0,0,0,0.45) */
    --modal-padding: 24px;
    --modal-gap: 16px;

    /* Sizes (max-width) */
    --modal-sm: 420px;   /* confirmations, simple forms (strict minimum) */
    --modal-md: 600px;   /* standard forms */
    --modal-lg: 900px;   /* complex data entry */
    --modal-xl: min(1200px, 95vw); /* by exception: content that needs > 900px */
    --modal-z: 1000;     /* toasts stack above at 1100 (toasts.md) */
}
```

---

## 2. When to use a modal (and when not to)

A modal **interrupts the user and blocks the page behind it** - the whole point and the whole risk.
Use it sparingly.

| Use a modal when… | Use something else when… |
|---|---|
| The action needs a **deliberate decision now** (delete, discard, confirm payment) | The content is long or browsable → use a **page / route** |
| A short, focused task should keep the **surrounding context** visible | The task is a multi-step wizard → consider a **page** or stepper |
| You must **block progress** until the user responds | It's a non-blocking notification → use a **toast** (button guide §7) |
| A small form (1–6 fields) supports the current view | You'd stack a modal **on top of another modal** → rethink the flow |

**Rule:** never open a modal from within a modal. If a flow needs that, it belongs on a page.

---

## 3. Anatomy of a modal

A modal is **a backdrop** + **a labelled dialog surface** + **a header** + **a body** + **an action footer**.

```
┌───────────────────────────────────────────────┐  ← backdrop (click closes)
│   ┌─────────────────────────────────────┐     │
│   │  Title (aria-labelledby)     [ × ]  │ ← title left · close right (same row)
│   ├─────────────────────────────────────┤     │
│   │  Body — message / form / content    │ ← body (scrolls if tall)
│   ├─────────────────────────────────────┤     │
│   │            [ Cancel ]  [ Confirm ]   │ ← footer: one solid action
│   └─────────────────────────────────────┘     │
└───────────────────────────────────────────────┘
```

- **Backdrop:** dims and blocks the page; clicking it closes the modal (§6).
- **Dialog surface:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby` → the title id.
- **Header:** the **title on the left** and a close (`×`) button on the **right**, on the **same row** and vertically aligned; the close button has `aria-label="Close"`.
- **Body:** the message or form. Scrolls internally when content exceeds the viewport - the header/footer stay put.
- **Footer:** action buttons. Exactly **one** solid/primary button; Cancel is a ghost (button guide §4). A long, segmented form adds a **tab strip in the header** and a **helper note on the left** of the footer - see §16.

---

## 4. Modal types

| Type | Purpose | Default size | Primary button |
|---|---|---|---|
| **Confirmation** | A yes/no decision before a single action | Small (420px) | Matches the action (Primary or **Danger**) |
| **Destructive confirmation** | Confirm an **irreversible** action (delete, remove, revoke) | Small (420px) | **Danger** |
| **Discard confirmation** | "You have unsaved changes" guard on exit | Small (420px) | **Danger** ("Discard") |
| **Form modal** | A focused create/edit form | Medium (600px) | Primary ("Save", "Create") |
| **Complex / data-entry** | Multi-section forms, detail editors | Large (900px) | Primary |
| **Segmented / tabbed form** *(optional, on request)* | A long form split into tabbed segments (§16) | Large (900px) | Primary ("Save") |

> Confirmation and discard modals are the two most common and the easiest to get wrong - §7 and §8 cover them in full.

---

## 5. Sizes — principled, driven by content

**PRINCIPLED:** pick the size from what's inside, not from how important it feels.

| Size | Token | Max-width | Use for |
|---|---|---|---|
| **Small** | `modal-sm` | **420px** | Confirmations, discard prompts, simple 1–3 field forms |
| **Medium** *(default form)* | `modal-md` | **600px** | Standard forms (create/edit a record) |
| **Large** | `modal-lg` | **900px** | Complex data entry, multi-column or multi-section forms |
| **Extra-large** *(by exception)* | `modal-xl` | **> 900px** | Content that genuinely needs more room (wide tables, side-by-side editors, dashboards-in-a-modal) |

**Rules**
- **420px is a strict minimum.** Never make a modal narrower than `modal-sm` - even a one-line confirmation uses 420px.
- **For content that fits within 900px:** 420px (confirmations / simple), 600px (standard forms), 900px (complex). Pick the smallest size that comfortably holds the content.
- **900px is not a hard ceiling.** If content legitimately needs more (a wide table, side-by-side panels, a rich editor), the modal **may exceed 900px** - still capped so it never becomes full-bleed on large screens (e.g. `max-width: min(1200px, 95vw)`).
- The width is a **max-width**; on narrow screens the modal becomes near-full-width with a safe margin (§11).
- Height is **content-driven** with a cap (`max-height: 90vh`); the **body scrolls**, never the whole dialog - keep header and footer pinned.
- If you want to exceed 900px for a *form*, first ask whether it belongs on a **page** (§2).

```css
/* Exceeding 900px when content requires it — still capped, never full-bleed */
.modal-xl { max-width: min(1200px, 95vw); }
```

---

## 6. Behavior rules (ENFORCED)

Non-negotiable. Every modal implements **all four**.

| Rule | Behavior | Why |
|---|---|---|
| **ESC key closes** | Pressing `Escape` dismisses the modal (treated as Cancel) | Universal, expected escape hatch |
| **Backdrop click closes** | Clicking the dimmed area outside the dialog dismisses it | Predictable, fast dismissal |
| **Focus trapped** | `Tab`/`Shift+Tab` cycle **only** through focusable elements inside the modal | Keyboard users can't get lost behind the modal |
| **Return focus on close** | On close, focus returns to the **element that opened it** | No "lost focus" jump to the top of the page |

**Supporting requirements that make the four work:**
- **Move focus in on open** - to the first focusable control, the dialog, or (for destructive modals) the **safe** action, *never* the destructive one.
- **Scroll-lock the background** - the page behind must not scroll while the modal is open.
- **Inert background** - set `aria-hidden`/`inert` on the rest of the page so AT and Tab don't reach it.

> **Exception — destructive & dirty-form modals:** ESC and backdrop-click still close, but treat them as **Cancel**, never confirm. If closing would lose typed data, see §8 - a backdrop/ESC dismiss may need a discard guard, or should be disabled in favor of explicit buttons.

### Close-intent matrix

| Trigger | Confirmation modal | Form with unsaved changes |
|---|---|---|
| **Cancel button** | Close, no action | Prompt discard (§8) |
| **× / ESC / backdrop** | Close, no action | Prompt discard (§8) |
| **Confirm / Save** | Run action, then close | Save, then close |

---

## 7. Delete / destructive confirmations

The most safety-critical modal. Make an **irreversible** action **deliberate** without being annoying.

### Rules
1. **Name the exact target.** "Delete **project "Q3 Launch"**?" - not "Delete this item?".
2. **State the consequence and whether it's recoverable.** "This permanently deletes the project and its 14 tasks. **This can't be undone.**"
3. **Use a Danger button** (`btn-danger`, Violet-Red `#991547`) labelled with the **verb** - "Delete project", not "OK" or "Yes".
4. **Cancel is the safe default.** Ghost button, left of the destructive action, **initial focus on Cancel** (or the dialog), never on Delete.
5. **Don't rely on color alone.** Pair Danger red with a clear verb and (optionally) a warning icon.
6. **High-stakes deletes get friction.** For bulk or unrecoverable destruction, require typing the name or checking "I understand" before enabling the Danger button.
7. **Resolve the outcome with a toast.** On success, close and show a toast ("Project deleted."); on error, keep the modal open or reopen with an inline error. Don't relabel buttons (button guide §7).

### Markup pattern

```html
<div class="modal-backdrop" data-modal>
  <div class="modal modal-sm" role="alertdialog" aria-modal="true"
       aria-labelledby="del-title" aria-describedby="del-desc">
    <div class="modal-header">
      <h2 class="modal-title" id="del-title">Delete project “Q3 Launch”?</h2>
      <button class="btn btn-ghost btn-icon" aria-label="Close" data-close>×</button>
    </div>
    <div class="modal-body">
      <p id="del-desc">
        This permanently deletes the project and its 14 tasks.
        <strong>This can’t be undone.</strong>
      </p>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" data-close autofocus>Cancel</button>
      <button class="btn btn-danger" data-confirm>Delete project</button>
    </div>
  </div>
</div>
```

> Use `role="alertdialog"` (not plain `dialog`) for destructive/critical confirmations so screen readers announce them with urgency.

### High-stakes "type to confirm" variant

```html
<div class="modal-body">
  <p id="del-desc">This deletes <strong>all 1,204 customer records</strong> permanently.
     Type <code>DELETE</code> to confirm.</p>
  <input class="modal-input" type="text" aria-label="Type DELETE to confirm"
         data-confirm-text="DELETE" autocomplete="off">
</div>
<div class="modal-footer">
  <button class="btn btn-ghost" data-close>Cancel</button>
  <button class="btn btn-danger" data-confirm disabled>Delete everything</button>
</div>
```

---

## 8. Discard / unsaved-changes confirmations

Protects work-in-progress; appears **only when there are actually unsaved changes**.

### Rules
1. **Track "dirty" state.** Only intercept close/cancel/navigation when the form has **unsaved edits**. A clean form closes immediately - never nag.
2. **Be honest about what's lost.** "You have unsaved changes. If you leave now, they'll be lost."
3. **The destructive choice is "Discard".** Use `btn-danger` for **Discard changes**; keep **Keep editing** as the safe ghost default with initial focus.
4. **Offer a way to save when it makes sense.** Three-way: *Keep editing* (ghost) · *Discard* (danger) · *Save* (primary) - only if saving is valid right now.
5. **Guard every exit path.** Cancel button, ×, ESC, backdrop click, *and* in-app navigation away from a dirty form all route through this guard.
6. **For full-page navigation,** also wire the browser's `beforeunload`.

### Markup pattern

```html
<div class="modal modal-sm" role="alertdialog" aria-modal="true"
     aria-labelledby="discard-title" aria-describedby="discard-desc">
  <div class="modal-header">
    <h2 class="modal-title" id="discard-title">Discard unsaved changes?</h2>
  </div>
  <div class="modal-body">
    <p id="discard-desc">You’ve made changes that haven’t been saved.
       If you leave now, they’ll be lost.</p>
  </div>
  <div class="modal-footer modal-footer-3">
    <button class="btn btn-ghost" data-close autofocus>Keep editing</button>
    <button class="btn btn-danger" data-discard>Discard changes</button>
    <button class="btn btn-primary" data-save>Save</button>
  </div>
</div>
```

---

## 9. Focus management & accessibility

Modals live or die on focus handling - this makes the §6 contract real.

- **On open:** save a reference to the trigger element, then move focus into the modal (first field / safe action / the dialog itself).
- **Trap focus:** intercept `Tab` and `Shift+Tab` so focus wraps within the modal's focusable elements only.
- **On close:** restore focus to the saved trigger element.
- **Roles:** `role="dialog"` for standard modals, `role="alertdialog"` for destructive/critical ones. Always `aria-modal="true"`.
- **Labelling:** `aria-labelledby` → the title; `aria-describedby` → the body message.
- **Background:** apply `inert` (or `aria-hidden="true"`) to everything outside the modal and **lock page scroll**.
- **Close button:** a real `<button>` with `aria-label="Close"`.
- **Motion:** respect `prefers-reduced-motion` - fade only, no large transforms.

---

## 10. Copy / labels

- **Title asks the question; button gives the answer.** Title "Delete project?" → button "Delete project". Keep the verb consistent.
- **Name the object** in the title or body ("…project "Q3 Launch"").
- **Buttons say the outcome,** never "OK" / "Yes" / "No". "Delete project", "Discard changes", "Keep editing".
- **State recoverability** for destructive actions: "This can't be undone."
- **Sentence case, active voice, no filler.**

| Context | Title | Safe (ghost) | Destructive / primary |
|---|---|---|---|
| Delete record | Delete invoice #1042? | Cancel | **Delete invoice** (danger) |
| Remove member | Remove Alex from team? | Cancel | **Remove member** (danger) |
| Unsaved changes | Discard unsaved changes? | Keep editing | **Discard changes** (danger) |
| Confirm submit | Submit application? | Cancel | **Submit** (primary) |

---

## 11. Full CSS reference (copy-paste ready)

Pairs with the button CSS from [buttons.md](buttons.md) §10. Uses `color-mix()`.

```css
/* ===== Backdrop ===== */
.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: var(--modal-z);
  display: grid;
  place-items: center;
  padding: 24px;
  background: var(--modal-backdrop);
  backdrop-filter: blur(var(--modal-backdrop-blur));          /* dim AND blur the page behind */
  -webkit-backdrop-filter: blur(var(--modal-backdrop-blur));  /* Safari */
  opacity: 0;
  transition: opacity 0.2s ease;
}
.modal-backdrop.open { opacity: 1; }

/* JS shows/hides via `hidden` (§12). Because .modal-backdrop sets display:grid, you MUST re-assert
   the hidden state — an author display rule overrides the UA [hidden]{display:none}, otherwise the
   transparent backdrop stays in layout and swallows clicks. */
.modal-backdrop[hidden] { display: none; }

/* Lock page scroll when a modal is open (add to <body>) */
body.modal-open { overflow: hidden; }

/* ===== Dialog surface ===== */
.modal {
  width: 100%;
  max-width: var(--modal-md);          /* default: standard form */
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  background: var(--color-surface);
  color: var(--color-text);
  border-radius: var(--modal-radius);
  box-shadow: var(--modal-shadow);
  transform: translateY(8px) scale(0.98);
  opacity: 0;
  transition: transform 0.2s ease, opacity 0.2s ease;
}
.modal-backdrop.open .modal { transform: none; opacity: 1; }

/* ===== Sizes ===== */
.modal-sm { max-width: var(--modal-sm); }  /* 420px — confirmations / simple forms (strict min) */
.modal-md { max-width: var(--modal-md); }  /* 600px — standard forms (default)                 */
.modal-lg { max-width: var(--modal-lg); }  /* 900px — complex data entry                       */
.modal-xl { max-width: var(--modal-xl); }  /* > 900px — only when content requires it, capped  */

/* ===== Regions ===== */
.modal-header {
  display: flex;
  align-items: center;                 /* title and close share one row, vertically aligned */
  justify-content: space-between;      /* title on the left, close on the right */
  gap: 12px;
  padding: calc(var(--modal-padding) / 2) var(--modal-padding) 0;  /* half padding on top */
}
.modal-title {
  font-family: var(--font-heading, 'Montserrat', sans-serif);  /* the fixed heading typeface */
  font-size: var(--text-h3, 18px);     /* 18px from the type scale (not a one-off value) */
  font-weight: 700;
  color: var(--color-primary);
  line-height: 1.3;
}
/* Close button: same row as the title, pinned to the right */
.modal-header .btn-icon {
  flex-shrink: 0;                      /* never let a long title squash the close target */
  margin: -4px -6px 0 0;               /* nudge into the top-right corner */
  font-size: 24px;                     /* larger × glyph — the button box stays 40px */
  line-height: 1;
}
.modal-body {
  padding: 12px var(--modal-padding);
  overflow-y: auto;                    /* only the body scrolls */
  color: var(--color-text);
  scrollbar-width: none;               /* hide the scrollbar visually; scroll still works */
}
.modal-body::-webkit-scrollbar { width: 0; height: 0; }   /* same, WebKit/Blink */
.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding: 0 var(--modal-padding) calc(var(--modal-padding) / 2);  /* half padding on bottom */
}
.modal-footer-3 { justify-content: space-between; }

/* ===== Inputs inside modals ===== */
.modal-input {
  width: 100%;
  margin-top: 10px;
  padding: 10px 12px;
  font: inherit;
  color: var(--color-text);
  background: var(--color-surface);
  border: 1px solid color-mix(in srgb, var(--color-text) 25%, transparent);
  border-radius: 8px;
}
.modal-input:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

/* ===== Responsive (mobile-first) =====
   On small or tall screens the modal becomes near-full-width and docks to the bottom, actions stack,
   and buttons grow to a >= 44px touch target. */
@media (max-width: 640px), (max-aspect-ratio: 1/1) {
  .modal-backdrop { padding: 12px; place-items: end center; }
  .modal { max-width: 100%; max-height: 92vh; }
  .modal-footer { flex-direction: column-reverse; }
  .modal-footer .btn { width: 100%; min-height: 44px; }  /* touch target */
}

/* ===== Reduced motion ===== */
@media (prefers-reduced-motion: reduce) {
  .modal-backdrop, .modal { transition: opacity 0.15s ease; }
  .modal { transform: none; }
}
```

---

## 12. Full JS reference (behavior contract)

Dependency-free implementation of the four ENFORCED rules plus focus management. Frameworks should use
their own dialog primitive (native `<dialog>`, Radix, Headless UI) - the contract is identical.

```js
const FOCUSABLE = [
  'a[href]', 'button:not([disabled])', 'input:not([disabled])',
  'select:not([disabled])', 'textarea:not([disabled])', '[tabindex]:not([tabindex="-1"])'
].join(',');

function openModal(backdrop, { onConfirm, isDirty } = {}) {
  const dialog  = backdrop.querySelector('.modal');
  const trigger = document.activeElement;            // remember opener (§6: return focus)

  document.body.classList.add('modal-open');         // scroll lock
  backdrop.hidden = false;
  requestAnimationFrame(() => backdrop.classList.add('open'));

  // Move focus in: prefer [autofocus] (the SAFE action), else the dialog
  const first = dialog.querySelector('[autofocus]') || dialog.querySelector(FOCUSABLE) || dialog;
  first.focus();

  function close(confirmed = false) {
    // Dirty-form guard on dismiss (§8)
    if (!confirmed && isDirty && isDirty()) {
      return showDiscardGuard(() => teardown());     // route to discard confirmation instead
    }
    teardown();
    if (confirmed && onConfirm) onConfirm();
  }

  function teardown() {
    backdrop.classList.remove('open');
    backdrop.addEventListener('transitionend', () => {
      backdrop.hidden = true;
      document.body.classList.remove('modal-open');
      if (trigger) trigger.focus();                  // §6: return focus to trigger
    }, { once: true });
  }

  function onKeydown(e) {
    if (e.key === 'Escape') { e.preventDefault(); close(false); }   // §6: ESC closes
    if (e.key === 'Tab') trapFocus(e, dialog);                      // §6: focus trap
  }

  // §6: backdrop click closes (ignore clicks inside the dialog)
  backdrop.addEventListener('mousedown', (e) => {
    if (e.target === backdrop) close(false);
  });

  dialog.querySelectorAll('[data-close]').forEach(el =>
    el.addEventListener('click', () => close(false)));
  dialog.querySelectorAll('[data-confirm]').forEach(el =>
    el.addEventListener('click', () => close(true)));

  document.addEventListener('keydown', onKeydown);
  backdrop._cleanup = () => document.removeEventListener('keydown', onKeydown);
}

function trapFocus(e, container) {
  const nodes = [...container.querySelectorAll(FOCUSABLE)].filter(n => n.offsetParent !== null);
  if (!nodes.length) return;
  const first = nodes[0], last = nodes[nodes.length - 1];
  if (e.shiftKey && document.activeElement === first) { e.preventDefault(); last.focus(); }
  else if (!e.shiftKey && document.activeElement === last) { e.preventDefault(); first.focus(); }
}
```

### React note

Prefer a headless dialog primitive (Radix `Dialog`, Headless UI `Dialog`, or native `<dialog>`) for
focus trap, ESC, scroll-lock, and `aria-modal`. Your job is then: track dirty state, choose the size
class, and wire the Danger/Discard buttons per §7-§8.

---

## 13. Accessibility checklist

- [ ] `role="dialog"` (or `role="alertdialog"` for destructive/critical) + `aria-modal="true"`.
- [ ] `aria-labelledby` → title id; `aria-describedby` → body message id.
- [ ] Focus moves **into** the modal on open (safe action / first field / dialog).
- [ ] Focus is **trapped** within the modal (`Tab` / `Shift+Tab` wrap).
- [ ] Focus **returns to the trigger** on close.
- [ ] **ESC** and **backdrop click** close the modal.
- [ ] Background is **inert / `aria-hidden`** and page scroll is **locked**.
- [ ] Close button is a real `<button>` with `aria-label="Close"`.
- [ ] Destructive confirm uses a verb label + Danger color (not color alone).
- [ ] Touch targets ≥ 44 × 44px; modal usable on small/tall screens.
- [ ] `prefers-reduced-motion` respected.

---

## 14. Do's and Don'ts

| ✅ Do | ❌ Don't | Why |
|---|---|---|
| Name the target: "Delete project "Q3 Launch"?" | Use vague "Are you sure?" / "Delete this item?" | Users must know exactly what they're destroying |
| Use `btn-danger` + a verb ("Delete project") for destructive confirm | Use a neutral "OK" or gold/accent button to delete | Verb + red prevents accidental destruction |
| Focus the **safe** action (Cancel / Keep editing) on open | Auto-focus the Delete / Discard button | Stops an instinctive Enter from destroying data |
| Show the discard prompt **only when the form is dirty** | Prompt "discard changes?" on a clean form | Nagging on no-op exits trains users to ignore it |
| Close on ESC and backdrop, returning focus to the trigger | Trap users with no ESC/backdrop and lost focus | The expected, accessible escape contract |
| Let only the **body** scroll; pin header + footer | Let the whole dialog (and the page behind) scroll | Keeps title and actions reachable |
| Size by content: 420 / 600 / 900px | Put a one-line confirmation in a 900px modal | Oversized modals bury a simple choice |
| State recoverability ("This can't be undone") | Hide the consequence of a destructive action | Users need to weigh the risk |
| Resolve success/error with a toast, close the modal | Leave the modal open showing "Deleted ✓" | The toast confirms it; the modal's job is done |
| Open one modal at a time | Stack a modal on top of another modal | Stacked modals disorient and trap focus badly |

---

## 15. Rules for the AI assistant

**ALWAYS**
- Implement the four ENFORCED behaviors (§6): ESC closes, backdrop closes, focus trapped, focus returns to trigger.
- Add `role="dialog"`/`alertdialog`, `aria-modal="true"`, `aria-labelledby`, and `aria-describedby`.
- Pick size by content: 420px (confirmations/simple), 600px (standard forms), 900px (complex data entry).
- For deletes: name the target, state recoverability, use `btn-danger` + verb, and focus the **safe** action.
- For dirty forms: guard every exit path (Cancel, ×, ESC, backdrop, navigation) with a discard prompt.
- Lock background scroll and make the background inert while open.
- Resolve outcomes with a toast; close the modal on success.

**NEVER**
- Auto-focus or default the destructive (Delete / Discard) button.
- Use "OK" / "Yes" / "No" for destructive actions, or a non-Danger color to delete.
- Prompt to discard changes when the form has none.
- Stack a modal on top of another modal.
- Let the page behind scroll, or leave focus loose behind the modal.
- Rely on color alone to signal danger.

---

## 16. Segmented / tabbed (multi-section) form modal

**Optional variant, built only on request.** Not a fixed default. Build a segmented / tabbed form modal
**only when the developer wants it**; never convert a form into tabbed segments on your own. The default
for a form stays a plain form modal (§4), or a **page** for a long or multi-step flow (§2).

When chosen, a segmented form splits a long create/edit form into **segments** shown as an **in-header
tab strip**, so the user works through one focused group of fields at a time. It is the `modal-lg`
"complex / data-entry" type (§4-§5) specialised for long forms.

### 16.1 Three fixed regions (only the body scrolls)

The modal is **header - body - footer**, and only the body scrolls:

- **Header (fixed):** the modal **title** and the **tab strip**. Neither scrolls.
- **Body (scrollable):** the fields of the **active segment only**. The *only* region that scrolls (§5, §11).
- **Footer (fixed):** a helper note on the **left**, the action buttons on the **right**.

Size it as a centered card growing to content up to `max-height: 92vh` (§5) with the standard
`--modal-radius`; never full-viewport height. Behind it sits the standard dimmed **and blurred** backdrop
(§1, §11); a backdrop click or ESC closes it, routed through the dirty-form guard of §8.

```
┌───────────────────────────────────────────────┐  ← dimmed + blurred backdrop
│   ┌─────────────────────────────────────────┐   │
│   │  New <Record>                     [ × ]  │ ← header (fixed): title + close
│   │  [ Essentials * ][ Details ][ More ]  ▸  │ ← tab strip: one line, scrolls sideways if tight
│   ├─────────────────────────────────────────┤   │
│   │  active segment's fields only           │ ← body (the ONLY region that scrolls)
│   ├─────────────────────────────────────────┤   │
│   │  * Required fields     [ Cancel ][ Save ]│ ← footer (fixed): hint left, actions right
│   └─────────────────────────────────────────┘   │
└───────────────────────────────────────────────┘
```

### 16.2 The tab strip (in-header)

- These tabs switch **in-modal sections, not routes**, so they use the **in-page tab widget** from [tabs.md](tabs.md) §4.2: `role="tablist"` / `role="tab"` / `role="tabpanel"`, roving tabindex, and `Left`/`Right`/`Home`/`End` keys. **Not** route-backed links.
- Keep every tab on a **single line**. When space is tight the strip **scrolls horizontally** and never wraps: `overflow-x: auto; overflow-y: hidden` (tabs.md §5). Keep the active tab scrolled into view.
- Put the segment holding the form's **required** fields **first**, and mark that tab with a trailing red `*`. Every other segment is optional.

### 16.3 Footer, helper note, required-fields note

- Actions sit on the **right**, grouped; the helper note stays on the **left** (`justify-content: space-between`).
- Buttons follow [buttons.md](buttons.md) and §3: **one** filled primary action (Save / Create) plus a **ghost** Cancel, not two filled buttons. Each may carry a leading icon.
- Show a small **"\* Required fields"** note: bold, danger color, about `.775rem`. Display it **only while the required (first) tab is active**, and toggle it with **`visibility`** (not `display`) so the footer keeps its width and the buttons never shift.

### 16.4 Validation UX

- Keep a red `*` on each **required field label**.
- On save, validate the required fields and every email field's format, and reject any dropdown value outside its option list. (Field rules belong to the app / [forms.md](forms.md); the modal owns the UX.)
- On a **failed** save, **highlight the invalid fields inline** ([forms.md](forms.md)) and **switch to the first tab that contains an error**.

### 16.5 CSS (adds to §11)

```css
/* Large form modal grows to content, capped; only the body scrolls (§5, §11) */
.modal-lg { max-width: var(--modal-lg); max-height: 92vh; }

/* In-header tab strip: single line, scrolls sideways if tight, scrollbar hidden but functional */
.modal-tabs {
  display: flex;
  gap: var(--pt-gap, 3px);
  padding: 8px var(--modal-padding) 0;
  overflow-x: auto;                      /* scroll sideways, never wrap */
  overflow-y: hidden;                    /* horizontal-only: no stray vertical scrollbar */
  scrollbar-width: none;                 /* hidden scrollbar; scroll still works */
}
.modal-tabs::-webkit-scrollbar { width: 0; height: 0; }
.modal-tab { white-space: nowrap; }      /* full tab styling + a11y: tabs.md §4.2 / §7 */
.modal-tab .req { margin-left: 4px; font-weight: 700; color: var(--color-danger); }  /* required-tab * */

/* Footer: helper note left, actions right */
.modal-footer { justify-content: space-between; align-items: center; }

/* "* Required fields" note: shown only on the required tab; hidden with visibility so the
   footer width (and the buttons) never shift. */
.modal-req-note { font-size: .775rem; font-weight: 700; color: var(--color-danger); }
.modal-req-note.is-off { visibility: hidden; }

/* Inline invalid-field highlight - see forms.md for the full error pattern */
.field-invalid { border-color: var(--color-danger); }
```

### 16.6 JS (adds to §12)

```js
/* Switch modal segments (in-page tab widget, tabs.md §4.2), toggle the required-fields note,
   and on a failed save jump to the first tab that has an error. */
function activateSegment(modal, index) {
  const tabs   = [...modal.querySelectorAll('[role="tab"]')];
  const panels = [...modal.querySelectorAll('[role="tabpanel"]')];
  tabs.forEach((t, i) => {
    const on = i === index;
    t.setAttribute('aria-selected', String(on));
    t.tabIndex = on ? 0 : -1;
    if (panels[i]) panels[i].hidden = !on;
  });
  // required-fields note shows only while the required (first) tab is active
  const note = modal.querySelector('.modal-req-note');
  if (note) note.classList.toggle('is-off', index !== 0);
}

function saveWithValidation(modal, validate) {
  const errors = validate();                 // app returns e.g. [{ field, segment }]
  if (errors.length) {
    errors.forEach(e => e.field.classList.add('field-invalid'));
    activateSegment(modal, Math.min(...errors.map(e => e.segment)));  // first tab with an error
    errors[0].field.focus();
    return false;
  }
  return true;                               // caller persists the record, then shows a toast (toasts.md)
}
```

### 16.7 Do's and Don'ts (in addition to §14)

| ✅ Do | ❌ Don't | Why |
|---|---|---|
| Offer the tabbed form for a long form; keep it the developer's choice | Convert every form to tabbed segments by default | Opt-in variant, not a fixed default (§16) |
| Split a long form into tabbed segments; scroll only the body | Let the whole dialog, or the page behind, scroll | Title, tabs, and actions stay reachable (§16.1) |
| Put the required fields in the **first** tab, marked `*` | Scatter required fields across several tabs | The user knows where the must-fills are (§16.2) |
| One filled Save + ghost Cancel (buttons.md) | Use two filled buttons in the footer | Keeps the kit's button hierarchy (§3) |
| Toggle the required note with `visibility` | Toggle it with `display` | `display` reflows the footer and shifts buttons (§16.3) |
| On a failed save, jump to the first tab with an error | Show a generic error with no tab change | The user sees the problem without hunting (§16.4) |
| Keep tabs on one line; scroll sideways if tight | Wrap tabs onto a second row | One quiet strip; mobile-first (tabs.md §5) |

---

## 17. Quick decision guide

```
Do I even need a modal?
├─ Must the user decide/act right now, blocking the page? ........ yes → modal
├─ Long or browsable content? .................................... no  → page / route
├─ Non-blocking confirmation of something done? .................. no  → toast
└─ Side task that can stay non-blocking? ......................... no  → drawer / inline

What KIND of modal?
├─ Irreversible (delete / remove / revoke) ...................... alertdialog + btn-danger (focus Cancel)
├─ Leaving a form with unsaved edits ............................ discard guard: Keep editing · Discard · Save
├─ A yes/no confirm (non-destructive) ........................... dialog + btn-primary
└─ Create / edit a record ....................................... form modal (primary Save)

What SIZE? (by content, not importance)
├─ Confirmation / simple 1–3 field form ......................... modal-sm  (420px)
├─ Standard create/edit form .................................... modal-md  (600px)
└─ Complex / multi-section data entry ........................... modal-lg  (900px)

Which behaviors are NON-NEGOTIABLE?
└─ ESC closes · backdrop closes · focus trapped · focus returns to trigger
```
