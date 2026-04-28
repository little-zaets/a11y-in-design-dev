# Radio Group — Code Examples

Canonical good and bad HTML code examples for the Radio Group component. See [`../RadioGroup.md`](../RadioGroup.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

## Good Examples

**Semantic HTML radio group (recommended)**

Native `<input type="radio">` elements with associated `<label>` elements, grouped in a `<fieldset>` with a `<legend>`. All radio buttons in the group share the same `name` attribute to ensure mutual exclusivity.

```html
<fieldset>
  <legend>Choose your favorite NATO letter</legend>
  
  <input type="radio" name="nato" id="alphaRadio">
  <label for="alphaRadio">Alpha</label>
  
  <input type="radio" name="nato" id="bravoRadio">
  <label for="bravoRadio">Bravo</label>
  
  <input type="radio" name="nato" id="charlieRadio" aria-describedby="hint-charlie" checked>
  <label for="charlieRadio">Charlie</label>
  <div class="hint" id="hint-charlie">This is the third NATO letter</div>
</fieldset>
```

**Required radio group with error state**

Apply `aria-required="true"` and `aria-invalid="true"` to the `<fieldset>` (with `role="radiogroup"` for broadest support). Include a visible "Required" indicator in the `<legend>` and associate the error message via `aria-describedby`.

```html
<fieldset aria-required="true" 
          aria-invalid="true" 
          role="radiogroup">
  <legend>Choose your second favorite NATO letter <span>Required</span></legend>
  
  <input type="radio" name="natoReq" id="deltaRadioReq">
  <label for="deltaRadioReq">Delta</label>
  
  <input type="radio" name="natoReq" id="echoRadioReq">
  <label for="echoRadioReq">Echo</label>
  
  <input type="radio" name="natoReq" id="foxtrotRadioReq">
  <label for="foxtrotRadioReq">Foxtrot</label>
</fieldset>
```

**Custom radio group with roving tabindex**

When native elements are not available, use `role="radiogroup"` on the container with `aria-labelledby`, and `role="radio"` with `aria-checked` and roving `tabindex` on each option. Only the focusable radio button has `tabindex="0"`.

```html
<h3 id="groupLabel">Choose a crust</h3>
<div role="radiogroup" aria-labelledby="groupLabel">
  <div role="radio" tabindex="0" aria-checked="false">Regular</div>
  <div role="radio" tabindex="-1" aria-checked="true">Deep dish</div>
  <div role="radio" tabindex="-1" aria-checked="false">Thin crust</div>
</div>
```

## Bad Examples

**Missing group container**

Radio buttons without a `<fieldset>`/`<legend>` (or `role="radiogroup"` with `aria-labelledby`) lose their group context. Screen reader users hear each option's label in isolation without understanding what the choices belong to or that they are mutually exclusive.

```html
<!-- ❌ No grouping — context and mutual exclusivity are unclear -->
<input type="radio" name="color" id="red">
<label for="red">Red</label>
<input type="radio" name="color" id="green">
<label for="green">Green</label>
<input type="radio" name="color" id="blue">
<label for="blue">Blue</label>
```

```html
<!-- ✅ Correct: grouped with fieldset and legend -->
<fieldset>
  <legend>Choose a color</legend>
  <input type="radio" name="color" id="red">
  <label for="red">Red</label>
  <input type="radio" name="color" id="green">
  <label for="green">Green</label>
  <input type="radio" name="color" id="blue">
  <label for="blue">Blue</label>
</fieldset>
```

**Mismatched or missing `name` attributes**

Native radio buttons that do not share the same `name` attribute are not treated as a group by the browser. Multiple selections become possible, breaking the single-selection contract.

```html
<!-- ❌ Different name values — browser allows multiple selections -->
<fieldset>
  <legend>Pick a size</legend>
  <input type="radio" name="size1" id="small">
  <label for="small">Small</label>
  <input type="radio" name="size2" id="medium">
  <label for="medium">Medium</label>
  <input type="radio" name="size3" id="large">
  <label for="large">Large</label>
</fieldset>
```

```html
<!-- ✅ Correct: all share the same name -->
<fieldset>
  <legend>Pick a size</legend>
  <input type="radio" name="size" id="small">
  <label for="small">Small</label>
  <input type="radio" name="size" id="medium">
  <label for="medium">Medium</label>
  <input type="radio" name="size" id="large">
  <label for="large">Large</label>
</fieldset>
```

**Custom radio buttons without roving tabindex**

Custom radio buttons that all have `tabindex="0"` require the user to Tab through every option individually, breaking the expected single-tab-stop interaction model and making the group tedious to navigate.

```html
<!-- ❌ Every radio is a separate tab stop -->
<div role="radiogroup" aria-labelledby="groupLabel">
  <div role="radio" tabindex="0" aria-checked="false">Regular</div>
  <div role="radio" tabindex="0" aria-checked="true">Deep dish</div>
  <div role="radio" tabindex="0" aria-checked="false">Thin crust</div>
</div>
```

```html
<!-- ✅ Correct: roving tabindex — only the active option is in the tab order -->
<div role="radiogroup" aria-labelledby="groupLabel">
  <div role="radio" tabindex="-1" aria-checked="false">Regular</div>
  <div role="radio" tabindex="0" aria-checked="true">Deep dish</div>
  <div role="radio" tabindex="-1" aria-checked="false">Thin crust</div>
</div>
```
