# Link — Code Examples

Canonical good and bad HTML code examples for the Link component. See [`../Link.md`](../Link.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

## Good Examples

**Semantic HTML link (recommended)**

Always prefer the native `<a>` element with a valid `href`. It provides built-in keyboard support, role, and focus management with no extra attributes required.

```html
<a href="/about/">
  About
</a>
```

**Link with descriptive context from a heading**

When link text alone is ambiguous, use `aria-describedby` to associate it with a nearby heading that provides context.

```html
<h3 id="unique-id">About our coffee subscriptions</h3>
<a href="/about/" aria-describedby="unique-id">Learn more</a>
```

**Heading as a link**

Wrapping the heading text inside the link keeps the accessible name descriptive.

```html
<h3><a href="/about/">About our coffee subscriptions</a></h3>
<p>Get the best coffee delivered to your door</p>
```

**Current page in a navigation set (`aria-current`)**

When a list of links includes the destination for the page the user is on, mark that link with `aria-current="page"` so it is not mistaken for an ordinary off-page link. Keep a valid `href` if the link still performs navigation (for example same-page refresh or hash navigation); ensure only one link in that nav uses `aria-current` at a time.

```html
<nav aria-label="Main">
  <a href="/">Home</a>
  <a href="/about/" aria-current="page">About</a>
  <a href="/contact/">Contact</a>
</nav>
```

**Custom element as link (when semantic HTML is not possible)**

A non-anchor element used as a link requires `role="link"` and `tabindex="0"` to make it focusable. JavaScript must handle `click` and `keydown` (Enter) to perform navigation. Prefer a real URL in script (for example from `data-href`) instead of reconstructing destinations from visible text alone.

```html
<!--
  Script should navigate on click and on Enter (keydown), and match native link expectations.
  Prefer <a href="/about/">About</a> whenever markup allows it.
-->
<span role="link" tabindex="0" data-href="/about/">
  About
</span>
```

**Disambiguating repeated links**

When the design calls for multiple links with the same visible text, use `aria-label` to give each one a unique accessible name.

```html
<a href="/tapered-black-pants/" aria-label="Buy now, Tapered Black Pants">
  Terms & Conditions
</a>
<a href="/pretty-in-pink/" aria-label="Buy now, Pretty in Pink Sweatshirt">
  Terms & Conditions
</a>
```

**Disabled link**

When a link must be present but inactive, use `aria-disabled="true"` to keep the element discoverable. Use `tabindex="0"` so the item remains in the tab order when you need it discoverable; use `tabindex="-1"` if it should be removed from sequential focus instead.

```html
<a aria-disabled="true" tabindex="0">
  Continue
</a>
```

## Bad Examples

**Generic link text without context**

Link text that does not describe the destination forces screen reader users to search for surrounding context. Either make the link text descriptive or associate it with a describing element via `aria-describedby`.

```html
<!-- ❌ Purpose unclear without surrounding context -->
<h3>About our coffee subscriptions</h3>
<a href="/about/">Learn more</a>
```

**Duplicating visible text in `aria-label`**

Do not repeat the visible link text in `aria-label`. Some screen readers announce both, causing redundant output.

```html
<!-- ❌ aria-label duplicates the inner text -->
<a href="/page/" aria-label="Do not repeat yourself">
  Do not repeat yourself
</a>
```

**JavaScript in `href`**

Using `javascript:void(0)` as an `href` value produces nonsensical screen reader output and provides no meaningful destination.

```html
<!-- ❌ Screen reader output is nonsensical -->
<a href="javascript:void(0)">Click me</a>
```

**Hash-only `href`**

An `href` of `#` provides no meaningful destination. If the element performs an action rather than navigating, it should be a `<button>`.

```html
<!-- ❌ No meaningful destination -->
<a href="#">Click me</a>
```

**Link used for an action**

If the element performs an action on the current page rather than navigating, it must be a `<button>`. Using an anchor misleads assistive technology users about the expected behavior.

```html
<!-- ❌ Anchor performs an action, not navigation -->
<a href="#" class="btn" onclick="submitForm()">
  Submit
</a>
```
