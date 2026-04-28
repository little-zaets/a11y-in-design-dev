# Text Input — Code Examples

Canonical good and bad HTML code examples for the Text Input component. See [`../TextInput.md`](../TextInput.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

## Good Examples

**Semantic text input with visible label (recommended)**

The native `<input>` element with an associated `<label>` is the most robust approach. The `for` attribute on the label must match the `id` on the input.

```html
<label for="best-character">
  The best Sesame Street character is:
</label>
<input type="text"
       id="best-character"
       aria-describedby="best-character-hint">
<div class="hint" id="best-character-hint">
  Example: Elmo, Big Bird, Cookie Monster
</div>
```

**Required input with visible indicator**

Mark required inputs with both the native `required` attribute and `aria-required="true"` for maximum assistive technology coverage. Include a visible text indicator near the label.

```html
<label for="second-nato-letter">
 The second NATO letter is: <span>Required</span>
</label>
<input type="text"
       id="second-nato-letter"
       aria-required="true"
       required
       value="Bravo">
```

**Grouped inputs with fieldset and legend**

When multiple related inputs appear together, wrap them in a `<fieldset>` with a `<legend>` so assistive technologies announce the group name before each input's label.

```html
<fieldset>
  <legend>
    Enter your personal information
  </legend>

  <label for="first-name">
    First name
  </label>
  <input type="text" id="first-name">

  <label for="last-name">
    Last name
  </label>
  <input type="text" id="last-name">

  <label for="username">
    Username
  </label>
  <input type="text" id="username">
</fieldset>
```

## Bad Examples

**Placeholder used as the only label**

Placeholder text disappears as soon as the user begins typing, leaving no visible label. It also fails contrast requirements in most browsers and is not reliably announced by all screen readers.

```html
<!-- ❌ No label — placeholder is the only identifier -->
<input type="text" placeholder="Enter your email">
```

```html
<!-- ✅ Correct: visible label with placeholder as supplementary hint -->
<label for="email">Email address</label>
<input type="text" id="email" placeholder="e.g. user@example.com">
```

**Missing label association**

A `<label>` element that is visually near the input but not programmatically associated (no `for` attribute, not wrapping the input) provides no benefit to assistive technology users.

```html
<!-- ❌ Label exists but is not associated with the input -->
<label>Email address</label>
<input type="text" id="email">
```

```html
<!-- ✅ Correct: for attribute matches input id -->
<label for="email">Email address</label>
<input type="text" id="email">
```

**Error message not associated with the input**

An error message that is only visually positioned near the input but not programmatically linked via `aria-describedby` will not be announced to screen reader users when they focus the input.

```html
<!-- ❌ Error is visual-only — not associated with the input -->
<label for="phone">Phone number</label>
<input type="tel" id="phone" aria-invalid="true">
<span class="error">Please enter a valid phone number</span>
```

```html
<!-- ✅ Correct: error linked via aria-describedby -->
<label for="phone">Phone number</label>
<input type="tel"
       id="phone"
       aria-invalid="true"
       aria-describedby="phone-error">
<span class="error" id="phone-error">Please enter a valid phone number</span>
```
