# Accordion — Code Examples

Canonical good and bad HTML code examples for the Accordion component. See [`../Accordion.md`](../Accordion.md) for the full rule file (ARIA, accessible name, keyboard interaction, color and contrast, and checklist).

**How to read this file:** **Good Examples** show recommended patterns; **Bad Examples** show common mistakes and what to avoid.

## Good Examples

**Semantic heading + button accordion (recommended)**

Each accordion item uses a native heading element wrapping a `<button>`. The button carries `aria-expanded` and `aria-controls`. Panels use `role="region"` with `aria-labelledby` referencing their trigger button. Collapsed panels are hidden with the `hidden` attribute. The heading level (`h3` here) fits the page's information architecture.

```html
<div class="accordion">
  <h3>
    <button id="accordion-btn-1"
            aria-expanded="true"
            aria-controls="accordion-panel-1">
      Personal Information
    </button>
  </h3>
  <div id="accordion-panel-1"
       role="region"
       aria-labelledby="accordion-btn-1">
    <p>Content for the personal information section.</p>
  </div>

  <h3>
    <button id="accordion-btn-2"
            aria-expanded="false"
            aria-controls="accordion-panel-2">
      Billing Address
    </button>
  </h3>
  <div id="accordion-panel-2"
       role="region"
       aria-labelledby="accordion-btn-2"
       hidden>
    <p>Content for the billing address section.</p>
  </div>

  <h3>
    <button id="accordion-btn-3"
            aria-expanded="false"
            aria-controls="accordion-panel-3">
      Shipping Address
    </button>
  </h3>
  <div id="accordion-panel-3"
       role="region"
       aria-labelledby="accordion-btn-3"
       hidden>
    <p>Content for the shipping address section.</p>
  </div>
</div>
```

**ARIA disclosure widget (simplified single-item pattern)**

A single disclosure item built with a semantic `<button>` and `aria-expanded`. This simplified pattern omits the heading wrapper and `role="region"` — when used inside a full accordion, wrap the button in the appropriate heading element (e.g., `<h3>`) and consider adding `role="region"` with `aria-labelledby` on the panel. The `aria-controls` attribute links the button to the content panel.

```html
<div class="expander-group">
  <button class="expander-toggle"
          aria-expanded="false"
          aria-controls="expanderContent">
    About Sesame Street
  </button>
  <div class="expander-content" id="expanderContent">
    Sesame Street is an American educational
    children's television series that combines
    live-action, sketch comedy, animation, and puppetry.
  </div>
</div>
```

**Accordion with expand/collapse icon**

An accordion trigger using an SVG icon to visually indicate the expanded/collapsed state. The icon is hidden from assistive technologies with `aria-hidden="true"` and `focusable="false"`. The `aria-expanded` attribute on the button communicates state programmatically.

```html
<h3>
  <button id="faq-btn-1"
          aria-expanded="false"
          aria-controls="faq-panel-1">
    How do I reset my password?
    <svg viewBox="0 0 10 10" aria-hidden="true" focusable="false">
      <rect class="vert" height="8" width="2" y="1" x="4"/>
      <rect height="2" width="8" y="4" x="1"/>
    </svg>
  </button>
</h3>
<div id="faq-panel-1"
     role="region"
     aria-labelledby="faq-btn-1"
     hidden>
  <p>Visit the account settings page and select "Reset password".</p>
</div>
```

## Bad Examples

**Using `role="tab"` and `role="tabpanel"` for accordion items**

Tabs and accordions are distinct patterns with different ARIA contracts and keyboard models. Using tab roles on an accordion causes screen readers to announce incorrect semantics and expect arrow-key navigation within a tablist, which the accordion does not implement.

```html
<!-- ❌ Tab roles applied to an accordion structure -->
<div role="tablist">
  <h3>
    <button role="tab" aria-selected="true" aria-controls="panel-1">
      Section One
    </button>
  </h3>
  <div role="tabpanel" id="panel-1">
    <p>Section one content.</p>
  </div>
  <h3>
    <button role="tab" aria-selected="false" aria-controls="panel-2">
      Section Two
    </button>
  </h3>
  <div role="tabpanel" id="panel-2" hidden>
    <p>Section two content.</p>
  </div>
</div>
```

```html
<!-- ✅ Correct: use heading + button + aria-expanded for accordions -->
<div>
  <h3>
    <button aria-expanded="true" aria-controls="panel-1">
      Section One
    </button>
  </h3>
  <div id="panel-1" role="region" aria-labelledby="...">
    <p>Section one content.</p>
  </div>
</div>
```

**`aria-expanded` placed on the heading instead of the button**

Placing `aria-expanded` on the heading means the state is not associated with the interactive control. Screen readers announce the expanded/collapsed state only when it is on a focusable, interactive element — the button. The heading itself is not interactive.

```html
<!-- ❌ aria-expanded on the heading, not the button -->
<h3 aria-expanded="false">
  <button aria-controls="panel-1">
    Section One
  </button>
</h3>
<div id="panel-1" hidden>
  <p>Section one content.</p>
</div>
```

```html
<!-- ✅ Correct: aria-expanded on the button -->
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Section One
  </button>
</h3>
<div id="panel-1" hidden>
  <p>Section one content.</p>
</div>
```

**Using `aria-hidden` as the sole hiding mechanism for panels**

`aria-hidden="true"` removes the panel from the accessibility tree but does not prevent keyboard interaction with its content. A keyboard user can still Tab into links, inputs, and other focusable elements inside the panel. Use the `hidden` attribute or `display: none` instead, which hides the content from all users.

```html
<!-- ❌ Panel hidden only with aria-hidden — focusable content still reachable -->
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Section One
  </button>
</h3>
<div id="panel-1" aria-hidden="true">
  <p>Content with a <a href="/link">focusable link</a> that keyboard users can still reach.</p>
</div>
```

```html
<!-- ✅ Correct: hidden attribute prevents all interaction -->
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Section One
  </button>
</h3>
<div id="panel-1" hidden>
  <p>Content with a <a href="/link">focusable link</a> that is properly hidden.</p>
</div>
```

**Button not wrapped in a heading element**

Without a heading wrapper, accordion triggers lose their heading-level semantics. Screen reader users cannot navigate to accordion sections using heading navigation shortcuts, and the content structure is not conveyed programmatically.

```html
<!-- ❌ Button without a heading wrapper — no heading semantics -->
<button aria-expanded="false" aria-controls="panel-1">
  Section One
</button>
<div id="panel-1" hidden>
  <p>Section one content.</p>
</div>
```

```html
<!-- ✅ Correct: button inside a heading -->
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Section One
  </button>
</h3>
<div id="panel-1" hidden>
  <p>Section one content.</p>
</div>
```

Additional antipatterns to watch for:
- Interactive content (links, buttons, inputs) placed inside the heading alongside the trigger button — only the `<button>` should be inside the heading.
- Overriding the heading role with `role="button"` on the heading element itself, which removes heading semantics entirely.
