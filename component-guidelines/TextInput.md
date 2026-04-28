# Text Input

## Overview

**Type:** Standalone widget
**Interaction Models:** Input
**Equivalent HTML element:** `<input type="text">`

A text input is a single-line field that allows users to enter and edit free-form text. It is used for collecting short text values such as names, email addresses, search queries, or any other brief textual data.

### When to Use
- To collect a single line of text from the user (e.g., name, email, phone number, search term).
- To accept short-form free text where the expected value is not constrained to a predefined set of options.
- To collect data that benefits from a specific input type (e.g., `email`, `tel`, `url`, `search`, `password`).

### When Not to Use
- **Do not use a text input for multi-line text.** Use a `<textarea>` instead.
- Do not use a text input when the user must choose from a predefined set of options. Use a select, listbox, radio group, or combobox instead.
- Do not use a text input for boolean choices. Use a checkbox or switch instead.

### Contained Components
This component is atomic.

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (role, properties, states) → **Accessible Name** (labels, placeholder, groups, Label in Name) → **Keyboard** → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: Text inputs must be focusable and operable using keyboard navigation.
- State: Programmatic state must match visual state at all times.
- Accessible Name: Every text input must have a visible, programmatically associated label that clearly describes its purpose.
- Visible Focus Indicator: When a user tabs to a text input, there must be a visible visual indicator such as an outline or border change.
- Color Contrast: Input text and label text must meet contrast requirements. The input boundary must be distinguishable from its surroundings.
- Sufficient Target Size: Text inputs must meet a minimum target size of 24x24px.
- Prefer Semantic Markup: Use native `<input>` elements with an associated `<label>`. If a custom element is used, it must have `role="textbox"` and proper keyboard handling.
- Error Identification: When a text input contains an error, the error must be programmatically associated with the input and clearly described.

### ARIA Roles, States, and Properties

#### Role

- **`textbox`** — Native `<input type="text">` elements carry this implicitly. Custom elements require `role="textbox"`.

#### Properties

- **Accessible name** — Every text input must have an accessible name, computed from an associated `<label>` element by default. It may also be provided with `aria-labelledby` or `aria-label` (last resort only).
- **`aria-describedby`** — When hint text, instructions, or error messages are present, associate them using `aria-describedby` referencing the describing element's `id`. Multiple IDs can be space-separated to associate both a hint and an error.
- **`autocomplete`** — When the input collects a value with a known purpose (name, email, address, etc.), set the `autocomplete` attribute to the appropriate token (e.g., `autocomplete="email"`) per WCAG 1.3.5.

#### States

- **`aria-required="true"`** — When user input is mandatory, apply both the `required` HTML attribute and `aria-required="true"` for broadest assistive technology support. Include a visible "Required" indicator in or near the label.
- **Disabled** — When the input is unavailable and should not receive focus or participate in submission, use the native `disabled` attribute. When the input must stay in the tab order for discoverability (similar to `aria-disabled` on buttons), use `aria-disabled="true"` and block activation/submission in code — do not conflate this with **read-only** (see next item).
- **Read-only** — For values the user must not edit but should read, select, and copy, use the `readonly` attribute on native `<input>`. For custom `role="textbox"`, set `aria-readonly="true"`. Do **not** use `aria-disabled="true"` for a true read-only field — that communicates "unavailable," not "editable but fixed." Ensure read-only state is also clear visually; some assistive technologies expose `readonly` inconsistently.
- **`aria-invalid="true"`** — When validation fails, set `aria-invalid="true"` on the input and associate the error message via `aria-describedby`.

### Accessible Name

This section expands the **Accessible name** and **`aria-describedby`** bullets under **Properties** above, with emphasis on visible `<label>`, `placeholder` limitations, and grouping.

- **`<label>` element** is the preferred source: use the `for` attribute matching the input's `id`. The label must be visible and must not be hidden on focus.
- **`aria-labelledby`** references one or more visible elements to construct the name. Use when the visual design places the label in a non-standard location relative to the input.
- **`aria-label`** provides a name directly on the element. Use only as a last resort when no visible label can be provided.
- **Do not use `placeholder` as a substitute for `<label>`.** Placeholder disappears while typing, is an unreliable source of name or instruction for assistive technologies, and is the subject of failure technique **F73** when used as the only label. Meet **1.4.3** (contrast) for real **label** and **value** text — not for placeholder standing in for a label.
- **When multiple inputs share similar labels** (e.g., "First name" and "Last name" within a group), wrap them in a `<fieldset>` with a `<legend>` to provide group context.
- **`aria-label` must contain all visible text** when used to add context beyond the visible label (WCAG 2.5.3 Label in Name).

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus into the text input. The focus indicator must be clearly visible. |
| **Shift + Tab** | Moves focus out of the text input to the previous focusable element. |
| **Standard text editing keys** | All platform-standard text editing keys (arrow keys, Home, End, Backspace, Delete, Ctrl/Cmd+A, Ctrl/Cmd+C, Ctrl/Cmd+V, etc.) must function as expected. JavaScript must not interfere with browser-provided text editing functions. |

After interaction, focus remains in the text input until the user explicitly moves it (via Tab, Shift+Tab, or pointer).

### Color and Contrast

- **Text contrast:** Input value text and label text must meet a minimum 4.5:1 contrast ratio against their respective backgrounds (WCAG 1.4.3). Large text (18px+ or 14px+ bold) requires 3:1.
- **Non-text contrast:** The input boundary (border or other visual indicator distinguishing the input from the background) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Focus indicator contrast:** The focus indicator must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Do not rely solely on color to indicate state.** Error, focus, disabled, and read-only states must be distinguishable through means other than color alone (e.g., border style, icon, text label) (WCAG 1.4.1).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled inputs are still visually recognizable as inputs.

### Code Examples

Canonical good and bad HTML code examples for the Text Input component live in a separate file: [`./Examples/TextInput.md`](./Examples/TextInput.md).

## Checklist

### Markup
- [ ] Use a native `<input>` element with an appropriate `type` attribute. If a custom element must be used, add `role="textbox"` and `tabindex="0"`. [1.3.1, 4.1.2]
- [ ] Every text input has a visible `<label>` with a `for` attribute matching the input's `id`. [1.3.1, 3.3.2, 4.1.2]
- [ ] `aria-label` is only used when no visible label can be provided. It is not a substitute for `<label>`. [4.1.2]
- [ ] `placeholder` is not used as the only label or instruction. [1.3.1, 3.3.2]
- [ ] When `aria-label` is used to add context, it contains all of the visible text. [2.5.3]
- [ ] Hint text and error messages are associated via `aria-describedby`. [1.3.1, 3.3.1]
- [ ] Related inputs are grouped with `<fieldset>` and `<legend>`. [1.3.1, 3.3.2]
- [ ] Inputs collecting known data types use the appropriate `autocomplete` token. [1.3.5]
- [ ] `tabindex` is `0` or omitted (native inputs are focusable by default), never a positive integer. [2.4.3]

### States
- [ ] Required inputs use `aria-required="true"` and/or the native `required` attribute, with a visible "Required" indicator. [3.3.2]
- [ ] Disabled inputs use the native `disabled` attribute, or `aria-disabled="true"` when the field must remain focusable for discoverability (match behavior in the ARIA section above). [4.1.2]
- [ ] Read-only inputs use the `readonly` attribute (native) or `aria-readonly="true"` (custom textbox), without `aria-disabled` unless the control is actually unavailable. [4.1.2]
- [ ] Invalid inputs use `aria-invalid="true"` with an associated error message via `aria-describedby`. [3.3.1, 4.1.2]
- [ ] Programmatic state (required, disabled, read-only, invalid) matches visual state at all times. [4.1.2]

### Keyboard
- [ ] Text input is focusable via Tab. [2.1.1, 2.4.7]
- [ ] Standard text editing keys function as expected and are not intercepted by JavaScript. [2.1.1]
- [ ] Focus is not trapped in the input — Tab and Shift+Tab move focus as expected. [2.1.2]

### Visual
- [ ] Label text and input value text meet 4.5:1 contrast ratio against their backgrounds (3:1 for large text). [1.4.3]
- [ ] Input boundary meets 3:1 contrast ratio against adjacent colors. [1.4.11]
- [ ] Focus, error, disabled, and read-only states are distinguishable by more than color alone. [1.4.1]
- [ ] Focus outline is not removed without replacement. [2.4.7]
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content. [1.4.11]
- [ ] Touch target is at least 24x24px. [2.5.8]
- [ ] Text spacing can be adjusted without loss of content or functionality. [1.4.12]

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/practices/forms/
- https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/identify-input-purpose.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/on-focus.html
- https://www.w3.org/WAI/WCAG22/Understanding/on-input.html
- https://www.w3.org/WAI/WCAG22/Understanding/error-identification.html
- https://www.w3.org/WAI/WCAG22/Understanding/labels-or-instructions.html
- https://www.w3.org/WAI/WCAG22/Understanding/error-suggestion.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#textbox
- https://www.w3.org/TR/wai-aria/#aria-required
- https://www.w3.org/TR/wai-aria/#aria-disabled
- https://www.w3.org/TR/wai-aria/#aria-readonly
- https://www.w3.org/TR/wai-aria/#aria-invalid
- https://www.w3.org/TR/wai-aria/#aria-describedby
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-labelledby
