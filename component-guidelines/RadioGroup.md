# Radio Group

## Overview

**Type:** Composite widget
**Interaction Models:** Single Select
**Equivalent HTML element:** `<input type="radio">` (within a `<fieldset>`)

A radio group is a set of selectable buttons — radio buttons — where only one button in the group can be selected at a time. Selecting a new radio button automatically deselects the previously selected one. Some implementations start with no selection to require an explicit user choice before proceeding.

### When to Use
- To let the user choose exactly one option from a set of two or more mutually exclusive choices.
- When all available options should be visible at once (unlike a select/listbox, which hides options behind a dropdown).
- When the number of options is small enough to display inline (typically 2–7 options).

### When Not to Use
- **Do not use a radio group when the user may select more than one option.** Use a checkbox group instead.
- Do not use a radio group for a single on/off toggle. Use a checkbox, switch, or toggle button instead.
- Do not use a radio group when the option list is long. Use a select, listbox, or combobox to conserve space.

### Radio Group vs. Select

Both patterns enforce single selection. A radio group displays all options at once and allows immediate comparison; a select collapses options behind a dropdown and is better suited when space is limited or the list is long. A radio group uses arrow keys to navigate and select within the group; a select uses its own interaction model. For roles and the keyboard model, see **ARIA Roles, States, and Properties** and **Keyboard Interaction** under **Accessibility** below.

### Contained Components

This component is composite.

- **Radio group** — container for all radio buttons (`role="radiogroup"`, equivalent HTML: `<fieldset>` with `<legend>`).
- **Radio button** — an individual selectable option within the group (`role="radio"`, equivalent HTML: `<input type="radio">`).

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (group and radio, each split into role, properties, states) → **Accessible Name** (group and per-option names) → **Keyboard** (default group behavior, then toolbar exception) → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: The radio group must be focusable and navigable using keyboard. Arrow keys move focus and selection between radio buttons; Tab moves focus into and out of the group as a single tab stop.
- State: Programmatic state must match visual state at all times. Checked vs. unchecked mapping (`aria-checked` and native `checked`) is specified in the **ARIA Roles, States, and Properties** section.
- Accessible Name: Both the radio group and each individual radio button must have clear, descriptive accessible names.
- Visible Focus Indicator: When a radio button has focus, there must be a visible visual indicator such as an outline or border change.
- Color Contrast: Radio button label text and the radio graphical indicator must meet contrast requirements.
- Sufficient Target Size: Radio buttons must meet a minimum touch target size of 24x24px.
- Prefer Semantic Markup: Use native `<input type="radio">` elements within a `<fieldset>` and `<legend>`. If custom elements are used, they must implement `role="radiogroup"`, `role="radio"`, `aria-checked`, roving `tabindex`, and proper keyboard handling.

### ARIA Roles, States, and Properties

#### Radio group: `role="radiogroup"`

##### Role

- **`radiogroup`** — For **custom** containers (e.g. a `<div>` wrapping `role="radio"` items), the group must have `role="radiogroup"`. A native `<fieldset>` with `<legend>` already groups radios and exposes a group name; the [APG radio examples](https://www.w3.org/WAI/ARIA/apg/patterns/radio/) use `<fieldset>`/`<legend>` **without** adding `role="radiogroup"` to the `<fieldset>`. Use `role="radiogroup"` when you are not using `<fieldset>`, or when following a specific pattern that requires it.

##### Properties

- **Accessible name** — The radio group must have an accessible name provided by `aria-labelledby` referencing a visible heading or `<legend>`, or by `aria-label` when no visible label exists.
- **`aria-describedby`** — When supplementary instructions or error messages apply to the group as a whole, associate them via `aria-describedby`.

##### States

- **`aria-required="true"`** — When the user must make a selection, apply `aria-required="true"` to the radio group container and include a visible "Required" indicator.
- **`aria-invalid="true"`** — When validation fails (no selection made on a required group), set `aria-invalid="true"` on the radio group container and associate the error message via `aria-describedby`.

#### Radio button: `role="radio"`

##### Role

- **`radio`** — Native `<input type="radio">` elements carry this implicitly. Custom elements require `role="radio"`.

##### Properties

- **Accessible name** — Each radio button must have an accessible name, computed from an associated `<label>` element by default. It may also be provided with `aria-labelledby` or `aria-label`.
- **`aria-describedby`** — When supplementary text is present for an individual option (hints, additional context), associate it via `aria-describedby`.
- **`name` attribute** — Native radio buttons in the same group must share the same `name` attribute to ensure mutual exclusivity and proper form submission.

##### States

- **`aria-checked`** — `aria-checked="true"` on the selected radio button; `aria-checked="false"` on all others. Native radio buttons manage this implicitly via the `checked` property; custom implementations must set it explicitly.
- **Disabled** — When an individual option is unavailable, use `aria-disabled="true"` to keep it focusable and discoverable. Use the native `disabled` attribute only when the option should be completely removed from keyboard navigation.

### Accessible Name

This section expands the **Accessible name** (and related) bullets for both the **Radio group** and each **Radio button** in the ARIA section above.

#### Radio group

- **`<legend>` element** is the preferred source when using a native `<fieldset>`. The legend text becomes the group's accessible name.
- **`aria-labelledby`** references a visible heading or other text element. Use when the group is built with custom elements or when the visual label is not a `<legend>`.
- **`aria-label`** provides a name directly on the group container. Use only when no visible label can be provided.

#### Radio button

- **`<label>` element** is the preferred source: use the `for` attribute matching the radio button's `id`. The label must be visible.
- **`aria-labelledby`** references one or more visible elements to construct the name.
- **`aria-label`** provides a name directly on the element. Use only as a last resort when no visible label exists.
- **Do not place interactive elements inside `<label>` tags.** If supplementary interactive content must accompany a radio button, place it outside the label and associate it via `aria-describedby`.

### Keyboard Interaction

Start with the default **radio group** pattern. When the same radios sit **inside a toolbar**, keyboard behavior differs (second subsection).

#### Within the radio group

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus into the radio group. If a radio button is checked, focus lands on the checked button. If no button is checked, focus lands on the first button in the group. |
| **Shift + Tab** | Moves focus out of the radio group to the previous focusable element. The entire group is a single tab stop. |
| **Right Arrow / Down Arrow** | Moves focus to the next radio button in the group, checks it, and unchecks the previously checked button. Wraps from the last button to the first. |
| **Left Arrow / Up Arrow** | Moves focus to the previous radio button in the group, checks it, and unchecks the previously checked button. Wraps from the first button to the last. |
| **Space** | Checks the focused radio button if it is not already checked. |

The radio group acts as a single tab stop in the page's tab order. Only one radio button within the group is in the tab sequence at a time (the checked button, or the first button if none is checked). This is managed via roving `tabindex`: the focusable button has `tabindex="0"` and all others have `tabindex="-1"`.

#### Within a toolbar

When a radio group is nested inside a toolbar, the keyboard interaction changes:

| Key | Behavior |
|-----|----------|
| **Space / Enter** | Checks the focused radio button if unchecked. Does nothing if already checked. |
| **Right Arrow** | Moves focus to the next radio button, or to the next toolbar element if at the end of the group. Wraps to the start of the toolbar. |
| **Left Arrow** | Moves focus to the previous radio button, or to the previous toolbar element if at the start of the group. Wraps to the end of the toolbar. |
| **Down Arrow / Up Arrow** | (Optional) Cycles focus within the radio group only, without moving to other toolbar elements. |

In a toolbar context, arrow keys move focus but do not automatically check the radio button — the user must press Space or Enter to confirm selection.

### Color and Contrast

- **Text contrast:** Radio button label text must meet a minimum 4.5:1 contrast ratio against its background (WCAG 1.4.3). Large text (18px+ or 14px+ bold) requires 3:1.
- **Non-text contrast:** The radio button graphical indicator (the circle, fill, and any border) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Focus indicator contrast:** The focus ring must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Do not rely solely on color to indicate state.** Selected, unselected, focused, and disabled states must be distinguishable through means other than color alone (e.g., filled circle vs. empty circle, border style, opacity) (WCAG 1.4.1).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled radio buttons are still visually recognizable as radio buttons.

### Code Examples

Canonical good and bad HTML code examples for the Radio Group component live in a separate file: [`./Examples/RadioGroup.md`](./Examples/RadioGroup.md).

## Checklist

### Markup
- [ ] The radio group container uses `<fieldset>` with `<legend>` (or `role="radiogroup"` with `aria-labelledby`). [1.3.1, 4.1.2]
- [ ] Each radio button uses a native `<input type="radio">`. If a custom element must be used, add `role="radio"`, roving `tabindex`, `aria-checked`, and keyboard event handlers. [1.3.1, 4.1.2]
- [ ] Every radio button has a visible `<label>` with a `for` attribute matching the radio button's `id`. [1.3.1, 3.3.2, 4.1.2]
- [ ] All native radio buttons in the same group share the same `name` attribute. [1.3.1]
- [ ] `aria-label` is only used when no visible label can be provided. [4.1.2]
- [ ] No interactive elements are nested inside `<label>` tags. Supplementary interactive content uses `aria-describedby`. [1.3.1, 4.1.2]
- [ ] Hint text and error messages are associated via `aria-describedby` on the appropriate element (group or individual radio). [1.3.1, 3.3.1]

### States
- [ ] The selected radio button has `aria-checked="true"`; all others have `aria-checked="false"`. Native radio buttons manage this implicitly. [4.1.2]
- [ ] Disabled radio buttons use `aria-disabled="true"` (preferred for discoverability) or the native `disabled` attribute. [4.1.2]
- [ ] Required radio groups use `aria-required="true"` on the group container with a visible "Required" indicator. [3.3.2, 4.1.2]
- [ ] Invalid radio groups use `aria-invalid="true"` on the group container with an associated error message via `aria-describedby`. [3.3.1, 4.1.2]
- [ ] Programmatic state (selected, disabled, required, invalid) matches visual state at all times. [4.1.2]

### Keyboard
- [ ] The radio group is a single tab stop. Tab moves focus into the group (to the checked button, or the first button if none is checked). [2.1.1, 2.4.3]
- [ ] Arrow keys move focus and selection between radio buttons, wrapping at both ends. [2.1.1]
- [ ] Space checks the focused radio button. [2.1.1]
- [ ] Focus is not trapped in the group — Tab and Shift+Tab move focus out. [2.1.2]
- [ ] Custom implementations use roving `tabindex` (`0` on the focusable button, `-1` on all others). [2.4.3]

### Visual
- [ ] Label text meets 4.5:1 contrast ratio against its background (3:1 for large text). [1.4.3]
- [ ] Radio button graphical indicator meets 3:1 contrast ratio against adjacent colors. [1.4.11]
- [ ] Selected, unselected, focused, and disabled states are distinguishable by more than color alone. [1.4.1]
- [ ] Focus outline is not removed without replacement. [2.4.7]
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content. [1.4.11]
- [ ] Touch target is at least 24x24px. [2.5.8]

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/radio/
- https://www.w3.org/WAI/ARIA/apg/patterns/radio/examples/radio/
- https://www.w3.org/WAI/ARIA/apg/patterns/radio/examples/radio-activedescendant/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/error-identification.html
- https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#radiogroup
- https://www.w3.org/TR/wai-aria/#radio
- https://www.w3.org/TR/wai-aria/#aria-checked
- https://www.w3.org/TR/wai-aria/#aria-disabled
- https://www.w3.org/TR/wai-aria/#aria-required
- https://www.w3.org/TR/wai-aria/#aria-invalid
- https://www.w3.org/TR/wai-aria/#aria-describedby
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-labelledby
- https://www.w3.org/TR/wai-aria/#aria-activedescendant
