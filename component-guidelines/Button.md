# Button

## Overview

**Type:** Standalone widget
**Interaction Models:** Action, Disclosure
**Equivalent HTML element:** `<button>`

A button is an interactive element that allows users to trigger an action or event — such as submitting a form, opening a dialog, cancelling an action, or performing a delete operation.

### When to Use
- To initiate an action on the current page (e.g., submit, save, delete, confirm).
- To open or close interface elements like dialogs, menus, or disclosure panels.
- To toggle a state (e.g., mute/unmute, bold/unbold).

### When Not to Use
- **Do not use a button when the action navigates to a new page or URL.** Use a link instead.
- Do not use a button for inline text that links to another resource. Use a link instead.
- Do not use a button to toggle between two views when the content is persistent and both states are always available. Use tabs instead.

### Contained Components
This component is atomic.

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (role, properties, states) → **Accessible Name** (patterns for naming in practice) → **Keyboard** (keys and post-activation focus) → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: Buttons must be focusable and operable using keyboard navigation.
- State: Programmatic state must match visual state at all times.
- Accessible Name: Buttons must have clear, descriptive text or alternative text (for icon buttons) that explains their purpose.
- Visible Focus Indicator: When a user tabs to a button, there must be a visible visual indicator, such as an outline or underline.
- Color Contrast: Button text must have a contrast ratio of at least 4.5:1 against its background. The button itself, if it is a graphical object, should have a 3:1 contrast ratio with its background.
- Sufficient Target Size: Buttons must meet a minimum touch target size of 24x24px.
- Prefer Semantic Markup: Use HTML `<button>` or `<input>` elements. If other elements are used, they must have role="button" and proper JavaScript keyboard handlers.

### ARIA Roles, States, and Properties

#### Role

- **`button`** — Native `<button>` elements carry this implicitly. Custom elements require `role="button"`.

#### Properties

- **Accessible name** — Every button must have an accessible name, computed from its text content by default. It may also be provided with `aria-labelledby` or `aria-label`.
- **`aria-describedby`** — If a supplementary description of the button's function is present, set `aria-describedby` referencing the describing element.
- **`aria-controls`** — When the button controls another element on the page (e.g., expands a panel, opens a dialog), set `aria-controls` to the `id` of the controlled element. Note: `aria-controls` has limited support across assistive technologies and should not be relied upon as the sole indicator of a relationship.

#### States

- **`aria-disabled="true"`** — When the action is unavailable, apply `aria-disabled="true"` to keep the button focusable and discoverable.
- **`aria-pressed`** — For toggle buttons, include `aria-pressed`. Set to `"true"` when on and `"false"` when off. The label must not change when the state changes.
- **`aria-expanded`** — For triggers that display and hide additional content, set `aria-expanded` to indicate whether an associated container is visible or hidden.

### Accessible Name

This section expands the **Accessible name** bullet under **Properties** above: text content, `aria-labelledby`, `aria-label`, avoiding duplicates, and disambiguating repeated labels.

The accessible name is the calculated string assistive technologies use to identify the name of the button.

- **Text content** is the default source: the inner text of the `<button>` element.
- **`aria-labelledby`** references another visible element on the page to construct the name.
- **`aria-label`** provides a name directly on the element. Use only when no visible text exists (icon-only buttons) or when visible text alone is ambiguous.
- **Do not duplicate visible text in `aria-label`** — some screen readers announce both, causing redundant output.
- **When multiple buttons share the same visible label** (e.g., repeated "Edit" buttons in a list), each must have a unique accessible name via `aria-label` with relevant context (e.g., "Edit billing address", "Edit shipping address").

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus to the button. The focus indicator must be clearly visible. |
| **Enter** | Activates the button. |
| **Space** | Activates the button. |

After activation, focus placement depends on the type of action:
- **Opens a dialog** → focus moves inside the dialog.
- **Closes a dialog** → focus returns to the trigger button (unless the action logically leads elsewhere, e.g., a confirmation that deletes the originating page).
- **Does not change context** (e.g., "Apply", "Recalculate") → focus stays on the button.
- **Triggers a context change** (e.g., "Next Step") → focus moves to the starting point of the new context.
- **Activated by shortcut key** → focus remains in the context where the shortcut was used.

### Color and Contrast

- **Text contrast:** Button label text must meet a minimum 4.5:1 contrast ratio against the button background (WCAG 2.2, criterion 1.4.3). Large text (18px+ or 14px+ bold) requires a 3:1 contrast ratio.
- **Non-text contrast:** The button boundary (or a visual indicator distinguishing it from the background) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 2.2, criterion 1.4.11).
- **Focus indicator contrast:** The focus ring must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 2.2, criterion 1.4.11).
- **Do not rely solely on color to indicate state.** Focus, hover, active, and disabled states must be distinguishable through means other than color alone (e.g., border, underline, icon, opacity change). This ensures usability for people with color vision deficiencies (WCAG 2.2, criterion 1.4.1).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled buttons are still visually recognizable as buttons.

### Code Examples

Canonical good and bad HTML code examples for the Button component live in a separate file: [`./Examples/Button.md`](./Examples/Button.md).

## Checklist

### Markup
- [ ] Use a native `<button>` element. If a non-interactive element must be used, add `role="button"`, `tabindex="0"`, and keyboard event handlers for Enter and Space.
- [ ] Every button has an accessible name — from text content, `aria-labelledby`, or `aria-label`.
- [ ] `aria-label` is only used when no visible text exists or when visible text is ambiguous. It is not a duplicate of the visible text.
- [ ] When `aria-label` is used to add context, it contains all of the visible text.
- [ ] Repeated buttons with the same visible label each have a unique accessible name via `aria-label`.
- [ ] No interactive elements (links, inputs, buttons, elements with `tabindex="0"`) are nested inside a `<button>`.
- [ ] `tabindex` is `0`, never a positive integer.

### States
- [ ] Disabled buttons use `aria-disabled="true"` (preferred) or the native `disabled` attribute, depending on discoverability needs.
- [ ] Toggle buttons use `aria-pressed` with a constant label.
- [ ] Menu buttons use `aria-haspopup` and `aria-expanded`. `aria-haspopup="menu"` is only used with `role="menu"` containers, never for site navigation.

### Keyboard
- [ ] Button is focusable via Tab.
- [ ] Button activates on Enter and Space.
- [ ] Focus moves appropriately after activation (into dialog, back to trigger, stays on button, or to new context).

### Visual
- [ ] Button text meets 4.5:1 contrast ratio against the button background (3:1 for large text).
- [ ] Button boundary meets 3:1 contrast ratio against adjacent colors.
- [ ] Focus, hover, active, and disabled states are distinguishable by more than color alone.
- [ ] Focus outline is not removed without replacement. If a global reset or component style sets `outline: 0` or `outline: none`, a visible focus indicator must be explicitly restored via `:focus` or `:focus-visible` (e.g., `outline: 2px solid #555; outline-offset: 2px`).
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content.
- [ ] Touch target is at least 24x24px.

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/button/
- https://www.w3.org/WAI/ARIA/apg/patterns/button/examples/button/
- https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/examples/menu-button-links/
- https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/
- https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation-hybrid/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#button
- https://www.w3.org/TR/wai-aria/#aria-disabled
- https://www.w3.org/TR/wai-aria/#aria-pressed
- https://www.w3.org/TR/wai-aria/#aria-expanded
- https://www.w3.org/TR/wai-aria/#aria-haspopup
- https://www.w3.org/TR/wai-aria/#aria-controls
- https://www.w3.org/TR/wai-aria/#aria-describedby
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-labelledby
