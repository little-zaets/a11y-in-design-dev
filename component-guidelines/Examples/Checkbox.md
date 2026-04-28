# Checkbox — Code Examples

Canonical good and bad HTML code examples for the Checkbox component. See [`../Checkbox.md`](../Checkbox.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

## Good Examples

**Semantic HTML checkbox group (recommended)**

Native `<input type="checkbox">` elements with associated `<label>` elements, grouped in a `<fieldset>` with a `<legend>`. This provides built-in keyboard support, role, state, and group naming with no extra ARIA attributes required.

```html
<fieldset>
  <legend>Choose your favorite Sesame Street characters:</legend>
  <input type="checkbox" id="elmoCheckbox">
  <label for="elmoCheckbox">Elmo</label>
  <input type="checkbox" id="bigBirdCheckbox">
  <label for="bigBirdCheckbox">Big Bird</label>
  <input type="checkbox" id="cookieCheckbox" checked>
  <label for="cookieCheckbox">Cookie Monster</label>
</fieldset>
```

**Checkbox with supplementary interactive content**

When additional content such as a link must accompany a checkbox, place the interactive element outside the `<label>` and associate it via `aria-describedby`. Never nest interactive elements inside a `<label>`.

```html
<fieldset>
  <legend>Legal disclaimers</legend>
  <div id="hint-tc" class="hint-checkbox">
    <a href="/code-of-conduct/">Read terms and conditions</a>
  </div>
  <input type="checkbox" id="tc-agree" aria-describedby="hint-tc">
  <label for="tc-agree">I agree to the terms and conditions</label>
</fieldset>
```

**Custom element as checkbox (when semantic HTML is not possible)**

A non-interactive element used as a checkbox requires `role="checkbox"`, `tabindex="0"`, and `aria-checked`. JavaScript event listeners for `click` and `keyup` (Space) must be added.

```html
<div role="checkbox" tabindex="0" aria-checked="true">
  Elmo
</div>
```

## Bad Examples

**Missing label association**

A checkbox without a programmatically associated label forces screen reader users to guess its purpose. The `<label>` must use a `for` attribute matching the input's `id`, or wrap the input.

```html
<!-- ❌ Label exists visually but is not associated -->
<input type="checkbox" id="agree">
<span>I agree to the terms</span>
```

```html
<!-- ✅ Correct: label associated via for attribute -->
<input type="checkbox" id="agree">
<label for="agree">I agree to the terms</label>
```

**Interactive element nested inside a label**

Placing a link or button inside a `<label>` creates overlapping click targets and confusing assistive technology behavior. Activating the label may inadvertently toggle the checkbox or follow the link, depending on the browser.

```html
<!-- ❌ Link nested inside label -->
<input type="checkbox" id="tc-agree">
<label for="tc-agree">
  I agree to the <a href="/terms">terms and conditions</a>
</label>
```

```html
<!-- ✅ Correct: link outside label, connected via aria-describedby -->
<div id="hint-tc">
  <a href="/terms">Read terms and conditions</a>
</div>
<input type="checkbox" id="tc-agree" aria-describedby="hint-tc">
<label for="tc-agree">I agree to the terms and conditions</label>
```

**Ungrouped related checkboxes**

Related checkboxes without a `<fieldset>` and `<legend>` (or `role="group"` with `aria-labelledby`) lose their group context. Screen reader users hear each label in isolation without understanding what the choices belong to.

```html
<!-- ❌ No grouping — context is lost -->
<input type="checkbox" id="elmo">
<label for="elmo">Elmo</label>
<input type="checkbox" id="bigBird">
<label for="bigBird">Big Bird</label>
<input type="checkbox" id="cookie">
<label for="cookie">Cookie Monster</label>
```

```html
<!-- ✅ Correct: grouped with fieldset and legend -->
<fieldset>
  <legend>Choose your favorite Sesame Street characters:</legend>
  <input type="checkbox" id="elmo">
  <label for="elmo">Elmo</label>
  <input type="checkbox" id="bigBird">
  <label for="bigBird">Big Bird</label>
  <input type="checkbox" id="cookie">
  <label for="cookie">Cookie Monster</label>
</fieldset>
```
