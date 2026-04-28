# Checkbox

## Overview

**Type:** Standalone widget
**Interaction Models:** Multi-Select
**Equivalent HTML element:** `<input type="checkbox">`

A checkbox is an interactive control that allows users to toggle a boolean option on or off, or to select one or more items from a set of choices. Checkboxes support two states (checked/unchecked) and, in composite groupings, a third mixed (indeterminate) state on a parent checkbox that reflects partial selection of its children.

### When to Use
- To let the user toggle a single independent option on or off (e.g., "Remember me", "I agree to terms").
- To let the user select zero or more options from a set of related choices.
- To provide a parent "select all" control over a group of related checkboxes (mixed/tri-state pattern).

### When Not to Use
- **Do not use a checkbox when only one option can be selected from a group.** Use a radio group instead.
- Do not use a checkbox for actions that take effect immediately upon activation. Use a switch or toggle button instead.
- Do not use a checkbox as a substitute for a button that triggers an action (e.g., submitting a form).

### Checkbox vs. Switch

A checkbox represents a selection state that is typically applied later (e.g., on form submission). A switch represents an on/off setting that takes effect immediately. If the change is instant, use a switch. If the change requires explicit confirmation, use a checkbox. For roles and keys, see **ARIA Roles, States, and Properties** and **Keyboard Interaction** under **Accessibility** below.

### Contained Components
This component is atomic.

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (role, properties, states) → **Accessible Name** (label, group `legend`, avoiding interactive content in labels) → **Keyboard** → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: Checkboxes must be focusable and togglable using keyboard navigation.
- State: Programmatic state must match visual state at all times. Use `aria-checked` / native `checked` / indeterminate as specified in the **ARIA Roles, States, and Properties** section and in **States** checklist.
- Accessible Name: Every checkbox must have a clear, descriptive label that explains its purpose.
- Visible Focus Indicator: When a user tabs to a checkbox, there must be a visible visual indicator such as an outline or border change.
- Color Contrast: Checkbox label text and the checkbox graphical indicator must meet contrast requirements.
- Sufficient Target Size: Checkboxes must meet a minimum touch target size of 24x24px.
- Prefer Semantic Markup: Use native `<input type="checkbox">` elements with an associated `<label>`. If a custom element is used, it must have `role="checkbox"`, `tabindex="0"`, `aria-checked`, and proper JavaScript keyboard handlers.

### ARIA Roles, States, and Properties

#### Role

- **`checkbox`** — Native `<input type="checkbox">` elements carry this implicitly. Custom elements require `role="checkbox"`.

#### Properties

- **Accessible name** — Every checkbox must have an accessible name, computed from an associated `<label>` element by default. It may also be provided with `aria-labelledby` or `aria-label` (last resort only).
- **`aria-controls`** — A mixed-state parent checkbox should reference its child checkboxes via `aria-controls` with a space-separated list of child `id` values. Note: `aria-controls` has limited assistive technology support and should not be the sole indicator of the relationship.
- **`aria-describedby`** — When supplementary text is present (hints, disclaimers), associate it via `aria-describedby` referencing the describing element's `id`.
- **Grouping (`role="group"` / `<fieldset>`)** — Related checkboxes must be enclosed in an element with `role="group"` (or a native `<fieldset>`) with an accessible name provided by `aria-labelledby` (or `<legend>`).

#### States

- **`aria-checked`** — `aria-checked="true"` when checked, `aria-checked="false"` when unchecked. Native checkboxes manage this implicitly via the `checked` property; custom checkboxes must set it explicitly.
- **`aria-checked="mixed"`** — For a parent checkbox that controls a group: set `aria-checked="mixed"` when some but not all child checkboxes are checked. The mixed state is only valid on a controlling parent, never on a standalone checkbox.
- **Disabled** — When the checkbox is unavailable, use the native `disabled` attribute. When the checkbox should remain focusable and discoverable, use `aria-disabled="true"`.
- **`aria-required="true"`** — When at least one checkbox in a group must be selected, apply `aria-required="true"` to the group container and include a visible "Required" indicator.

### Accessible Name

This section expands the **Accessible name**, **`aria-describedby`**, and **Grouping** bullets under **Properties** above, including `fieldset` / `legend` and interactive content next to labels.

- **`<label>` element** is the preferred source: use the `for` attribute matching the checkbox's `id`. The label must be visible and must not disappear on focus or state change.
- **`aria-labelledby`** references one or more visible elements to construct the name. Use when the visual design places the label in a non-standard position.
- **`aria-label`** provides a name directly on the element. Use only as a last resort when no visible label can be provided.
- **Do not place interactive elements inside `<label>` tags.** If supplementary content includes links or other interactive elements, place them outside the label and associate them via `aria-describedby`.
- **Group name:** Related checkboxes grouped in a `<fieldset>` receive their group name from the `<legend>`. Screen readers announce the group name before each checkbox's individual label.

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus to the checkbox. Each checkbox in a group is an independent tab stop. |
| **Space** | Toggles the checkbox between checked and unchecked. For a **mixed-state** parent that controls a group, follow the keyboard behavior in the [APG Checkbox (mixed-state) example](https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/examples/checkbox-mixed/) — the exact sequence (including whether a third activation restores a partial selection) is **implementation-specific**, not fixed in the pattern text. |

Focus remains on the checkbox after activation. There is no arrow-key navigation between checkboxes — each checkbox is reached independently via Tab (unlike radio groups, which use arrow keys within the group).

### Color and Contrast

- **Text contrast:** Checkbox label text must meet a minimum 4.5:1 contrast ratio against its background (WCAG 1.4.3). Large text (18px+ or 14px+ bold) requires 3:1.
- **Non-text contrast:** The checkbox graphical indicator (the box, checkmark, and any border) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Focus indicator contrast:** The focus ring must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Do not rely solely on color to indicate state.** Checked, unchecked, mixed, and disabled states must be distinguishable through means other than color alone (e.g., checkmark icon, dash icon, border style, opacity) (WCAG 1.4.1).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled checkboxes are still visually recognizable as checkboxes.

### Code Examples

Canonical good and bad HTML code examples for the Checkbox component live in a separate file: [`./Examples/Checkbox.md`](./Examples/Checkbox.md).

## Checklist

### Markup
- [ ] Use a native `<input type="checkbox">` element. If a custom element must be used, add `role="checkbox"`, `tabindex="0"`, and keyboard event handlers for Space. [1.3.1, 4.1.2]
- [ ] Every checkbox has a visible `<label>` with a `for` attribute matching the checkbox's `id`. [1.3.1, 3.3.2, 4.1.2]
- [ ] `aria-label` is only used when no visible label can be provided. It is not a substitute for `<label>`. [4.1.2]
- [ ] No interactive elements (links, buttons, inputs) are nested inside a `<label>`. Supplementary interactive content uses `aria-describedby`. [1.3.1, 4.1.2]
- [ ] Related checkboxes are grouped with `<fieldset>` and `<legend>` (or `role="group"` with `aria-labelledby`). [1.3.1, 3.3.2]
- [ ] `tabindex` is `0` or omitted (native checkboxes are focusable by default), never a positive integer. [2.4.3]

### States
- [ ] `aria-checked` is `"true"` when checked, `"false"` when unchecked. Native checkboxes manage this implicitly. [4.1.2]
- [ ] Mixed-state parent checkboxes use `aria-checked="mixed"` when child selection is partial, and reference children via `aria-controls`. [4.1.2]
- [ ] Disabled checkboxes use `aria-disabled="true"` (preferred for discoverability) or the native `disabled` attribute. [4.1.2]
- [ ] Programmatic state (checked, unchecked, mixed, disabled) matches visual state at all times. [4.1.2]

### Keyboard
- [ ] Checkbox is focusable via Tab. Each checkbox in a group is an independent tab stop. [2.1.1, 2.4.7]
- [ ] Space toggles the checkbox; mixed-state parents match the intended implementation, consistent with the APG mixed example where applicable. [2.1.1]
- [ ] Focus remains on the checkbox after activation. [2.4.7]

### Visual
- [ ] Label text meets 4.5:1 contrast ratio against its background (3:1 for large text). [1.4.3]
- [ ] Checkbox graphical indicator meets 3:1 contrast ratio against adjacent colors. [1.4.11]
- [ ] Focus, checked, unchecked, mixed, and disabled states are distinguishable by more than color alone. [1.4.1]
- [ ] Focus outline is not removed without replacement. [2.4.7]
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content. [1.4.11]
- [ ] Touch target is at least 24x24px. [2.5.8]

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/
- https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/examples/checkbox/
- https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/examples/checkbox-mixed/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#checkbox
- https://www.w3.org/TR/wai-aria/#aria-checked
- https://www.w3.org/TR/wai-aria/#aria-controls
- https://www.w3.org/TR/wai-aria/#aria-disabled
- https://www.w3.org/TR/wai-aria/#aria-required
- https://www.w3.org/TR/wai-aria/#aria-describedby
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-labelledby
- https://www.w3.org/TR/wai-aria/#group
