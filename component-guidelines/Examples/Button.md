# Button — Code Examples

Canonical good and bad HTML code examples for the Button component. See [`../Button.md`](../Button.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

## Good Examples

**Semantic HTML button (recommended)**

Always prefer the native `<button>` element. It provides built-in keyboard support, role, and focus management with no extra attributes required.

```html
<button>
  Continue
</button>
```

**Button with `aria-disabled` (preferred disabled pattern)**

Using `aria-disabled="true"` keeps the button in the tab order so screen reader users can discover it, attempt to activate it, and be notified of errors in the form.

```html
<button aria-disabled="true">
  Continue
</button>
```

**Custom element as button (when semantic HTML is not possible)**

A non-interactive element used as a button requires `role="button"` and `tabindex="0"` to make it focusable. JavaScript event listeners for `click`, `keydown` (Enter), and `keyup` (Space) must also be added.

```html
<div role="button"
tabindex="0" >
  Continue
</div>
```

**Icon-only button or button with no meaningful inner text**

When no visible text is present, use `aria-label` to provide an accessible name. Do not repeat the inner text in `aria-label`, as some screen readers will read both.

```html
<div role="button" tabindex="0" aria-label="Continue">
  <!-- icon but no text -->
</div>

<div role="button" tabindex="0" aria-label="Buy now, iPhone 17">
  Buy now <!-- Ambiguous text doesn't describe the intent -->
</div>
```

**Disambiguating repeated buttons**

When the design calls for multiple buttons with the same visible text, use `aria-label` to give each one a unique accessible name that describes its specific purpose.

```html
<button aria-label="Edit payment date">
  Edit
</button>
<button aria-label="Edit payment amount">
  Edit
</button>
```

**Action button using a `<div>`**

A `<div>` made into an action button with `role="button"` and `tabindex="0"`.

```html
<div tabindex="0"
     role="button"
     id="action">
  Print Page
</div>
```

**Toggle button using an `<a>` element**

A non-button element used as a toggle requires `role="button"`, `tabindex="0"`, and `aria-pressed`. The label ("Mute") stays constant regardless of state.

```html
<a tabindex="0"
   role="button"
   id="toggle"
   aria-pressed="false">
  Mute
  <svg aria-hidden="true" focusable="false">
    <use xlink:href="#icon-sound"></use>
  </svg>
</a>
```

## Bad Examples

**`<div>` without role or keyboard support**

This element looks like a button visually but is invisible to assistive technology. It cannot be focused with a keyboard, has no role, and does not respond to Enter or Space. A `<div role="button">` with only an `onclick` handler works for mouse users but not for keyboard users. Custom buttons must handle `keydown` for Enter and `keyup` for Space.

```html
<!-- ❌ Not focusable, no role, no keyboard support -->
<div class="btn" onclick="doSomething()">
  Submit
</div>
```

**Nesting interactive elements inside a button**

Placing a link, input, or another button inside a `<button>` produces invalid HTML and unpredictable assistive technology behavior. A button's content must be non-interactive (text, icons, spans).

```html
<!-- ❌ Interactive element nested inside a button -->
<button>
  <a href="/details">View details</a>
</button>

<!-- ❌ Another button nested inside a button -->
<button>
  Accept <button class="icon-btn">?</button>
</button>
```

**Using positive `tabindex` values**

`tabindex="1"` or higher forces the element to the front of the tab order, breaking the natural reading sequence. Use `tabindex="0"` to place custom buttons in the natural document order.

```html
<!-- ❌ Jumps ahead of every other focusable element -->
<div role="button" tabindex="5" onclick="doSomething()">
  Submit
</div>

<!-- ✅ Correct: participates in natural tab order -->
<div role="button" tabindex="0">
  Submit
</div>
```

**Link used for a button action**

If the element performs an action on the current page rather than navigating, it must be a `<button>`. Using an anchor misleads assistive technology users about the expected behavior.

```html
<!-- ❌ Link performs an action, not navigation -->
<a href="#" class="btn" onclick="submitForm()">
  Submit
</a>
```

**Changing the toggle label instead of using `aria-pressed`**

Swapping the label text between states confuses screen reader users. Use `aria-pressed` and keep the label constant.

```html
<!-- ❌ Label swaps instead of using aria-pressed -->
<button type="button" id="mute-btn">Mute</button>
<script>
  const btn = document.getElementById('mute-btn');
  btn.addEventListener('click', () => {
    btn.textContent = btn.textContent === 'Mute' ? 'Unmute' : 'Mute';
  });
</script>
```

**Overwriting visible text with `aria-label`**

Do not overwrite visible text with an `aria-label` unless additional context is required, in which case the `aria-label` must contain all of the visible text.

```html
<!-- ❌ aria-label overwrites the inner text -->
<button type="button" aria-label="Next">
  Continue
</button>
```

**Using native `disabled` when discoverability matters**

The native `disabled` attribute removes the button from the tab order entirely. If users need to find the button and understand why it is inactive (e.g., incomplete form), prefer `aria-disabled="true"`.

```html
<!-- ❌ Not reachable via keyboard tab -->
<button type="button" disabled>
  Submit
</button>

<!-- ✅ Preferred: still focusable and discoverable -->
<button type="button" aria-disabled="true">
  Submit
</button>
```

**Using `aria-haspopup` for site navigation menus**

`aria-haspopup="menu"` tells screen readers to expect a `role="menu"` popup with arrow-key navigation between `menuitem` children. This is appropriate for application-style action menus, not website navigation. For nav dropdowns, use a disclosure pattern (`aria-expanded`) with a list of links instead.

```html
<!-- ❌ Triggers menu interaction mode for site navigation -->
<button aria-haspopup="true" aria-expanded="false">
  Main Menu
</button>
<ul role="menu">
  <li role="menuitem"><a href="/about">About</a></li>
  <li role="menuitem"><a href="/contact">Contact</a></li>
</ul>

<!-- ✅ Correct: disclosure pattern with a list of links -->
<button aria-expanded="false">
  Main Menu
</button>
<ul>
  <li><a href="/about">About</a></li>
  <li><a href="/contact">Contact</a></li>
</ul>
```
