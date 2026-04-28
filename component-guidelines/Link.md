# Link

## Overview

**Type:** Standalone widget
**Interaction Models:** Navigation
**Equivalent HTML element:** `<a>`

A link is an interactive element that navigates the user to a different resource — such as another page, a section within the current page, a downloadable file, or an external URL. Links may also trigger actions handled by the browser, such as opening an email client or initiating a phone call.

### When to Use
- To navigate to a different page or URL.
- To jump to a section within the current page (anchor links).
- To trigger a download.
- To open an email client (`mailto:`) or phone app (`tel:`).

### When Not to Use
- **Do not use a link when the action does not navigate.** If activating the element submits a form, opens a dialog, toggles a state, or performs an in-page action, use a button instead.
- Do not use a link styled as a button to perform an action. The element's role must match its behavior — assistive technology users expect a link to navigate.
- Do not use a link with `href="#"` or `href="javascript:void(0)"` as a click handler. Use a `<button>` instead.

### Contained Components
This component is atomic.

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (role, properties, states) → **Accessible Name** (how to build the name in practice) → **Current page in a set** (`aria-current`, for links in nav-like sets) → **Keyboard** → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: Links must be focusable and operable using keyboard navigation.
- State: Programmatic state must match visual state at all times.
- Accessible Name: Links must have clear, descriptive text that communicates their destination or purpose.
- Visible Focus Indicator: When a user tabs to a link, there must be a visible visual indicator, such as an outline or underline.
- Color Contrast: Link text must have a contrast ratio of at least 4.5:1 against its background.
- Sufficient Target Size: Links must meet a minimum touch target size of 24x24px.
- Prefer Semantic Markup: Use HTML `<a>` elements with a valid `href`. If a non-anchor element is used, it must have `role="link"`, `tabindex="0"`, and JavaScript keyboard handlers.

### ARIA Roles, States, and Properties

#### Role

- **`link`** — Native `<a>` elements with an `href` carry this implicitly. Non-anchor elements used as links require `role="link"`.

#### Properties

- **Accessible name** — Every link must have an accessible name, computed from its text content by default. It may also be provided with `aria-labelledby` or `aria-label`.
- **`aria-describedby`** — If a supplementary description of the link's destination is present, set `aria-describedby` referencing the describing element.

#### States

- **`aria-current`** — Marks the link that corresponds to **the current resource** inside a related set (nav, breadcrumb trail, pagination, etc.). Full guidance follows in **Current page in a set** (after **Accessible Name**).
- **`aria-disabled="true"`** — When the link is inactive, apply `aria-disabled="true"` to keep the link focusable and discoverable. Remove or nullify the `href` attribute to prevent navigation.

### Accessible Name

This section expands the **Accessible name** bullet under **Properties** above: sources for the name (`text`, `aria-labelledby`, `aria-label`), avoiding duplicate `aria-label` text, link purpose, disambiguation, and WCAG 2.5.3 Label in Name.

The accessible name is the calculated string assistive technologies use to identify the link.

- **Text content** is the default source: the inner text of the `<a>` element.
- **`aria-labelledby`** references another visible element on the page to construct the name.
- **`aria-label`** provides a name directly on the element. Use only when no visible text exists or when visible text alone is ambiguous.
- **Do not duplicate visible text in `aria-label`** — some screen readers announce both, causing redundant output.
- **Link text must describe the destination or purpose.** Avoid generic text like "Learn more", "Click here", or "Read more" unless the surrounding context (e.g., a heading associated via `aria-describedby`) makes the purpose unambiguous.
- **When multiple links share the same visible label** (e.g., repeated "Buy Now" links), each must have a unique accessible name via `aria-label` that **includes the visible label text** and adds disambiguating context (e.g., "Buy Now, Tapered Black Pants" and "Buy Now, Pretty in Pink Sweatshirt") so the name satisfies WCAG 2.5.3 Label in Name.

### Current page in a set (`aria-current`)

When a link belongs to a **related set** of links (for example primary navigation, a breadcrumb trail, or pagination) and represents **the current page or step**, set `aria-current` using the token that matches the pattern (commonly `page` in site navigation). Only one item in that set should have `aria-current` at a time. The [APG Breadcrumb pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/) illustrates the idea; see also the **`aria-current`** bullet under **States** in **ARIA Roles, States, and Properties** above.

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus to the link. The focus indicator must be clearly visible. |
| **Enter** | Activates the link and navigates to the target destination. |

After activation, focus moves to the target resource. For same-page anchor links, focus moves to the target element.

### Color and Contrast

- **Text contrast:** Link text must meet a minimum 4.5:1 contrast ratio against its background (WCAG 2.2, criterion 1.4.3). Large text (18px+ or 14px+ bold) requires a 3:1 contrast ratio.
- **Distinguishing links (Use of Color, 1.4.1):** If a link is identified only by color relative to surrounding body text, add a non-color cue (e.g. underline) **or** ensure at least 3:1 contrast between **link and surrounding text** so the link is distinguishable (see G183; related to 1.4.1, not 1.4.11).
- **Focus indicator contrast:** The focus indicator must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 2.2, criterion 1.4.11).
- **Do not rely solely on color to indicate state.** Focus, hover, active, and disabled states must be distinguishable through means other than color alone (e.g., underline, outline, icon, opacity change). This ensures usability for people with color vision deficiencies (WCAG 2.2, criterion 1.4.1).

### Code Examples

Canonical good and bad HTML code examples for the Link component live in a separate file: [`./Examples/Link.md`](./Examples/Link.md).

## Checklist

### Markup
- [ ] Use a native `<a>` element with a valid `href`. If a non-anchor element must be used, add `role="link"`, `tabindex="0"`, and a keyboard event handler for Enter.
- [ ] Every link has an accessible name — from text content, `aria-labelledby`, or `aria-label`.
- [ ] `aria-label` is only used when no visible text exists or when visible text is ambiguous. It is not a duplicate of the visible text.
- [ ] When `aria-label` is used to add context, it contains all of the visible text.
- [ ] Repeated links with the same visible label each have a unique accessible name via `aria-label` that includes the full visible link text plus disambiguating context.
- [ ] Link text describes the destination or purpose. Generic text like "Learn more" or "Click here" is only used when context is provided via `aria-describedby` or an enclosing heading.
- [ ] No interactive elements (buttons, inputs, other links) are nested inside an `<a>`.
- [ ] `href` is not `javascript:void(0)` or `#`.

### States
- [ ] Disabled links use `aria-disabled="true"`.
- [ ] In navigation (or similar) link sets, the link for the **current** resource uses `aria-current` with the correct token (commonly `page`); only one item in the set uses `aria-current` at a time.
- [ ] Hover, focus, active, and disabled states are programmatically and visually distinguishable.

### Keyboard
- [ ] Link is focusable via Tab.
- [ ] Link activates on Enter.
- [ ] Focus moves to the target resource after activation.

### Visual
- [ ] Link text meets 4.5:1 contrast ratio against its background (3:1 for large text).
- [ ] Links within body text are distinguishable by more than color alone (e.g., underline, or 3:1 contrast ratio against surrounding text).
- [ ] Focus, hover, active, and disabled states are distinguishable by more than color alone.
- [ ] Focus outline is not removed without replacement. If a global reset or component style sets `outline: 0` or `outline: none`, a visible focus indicator must be explicitly restored via `:focus` or `:focus-visible` (e.g., `outline: 2px solid #555; outline-offset: 2px`).
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content.
- [ ] Touch target is at least 24x24px.

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/link/
- https://www.w3.org/WAI/ARIA/apg/patterns/link/examples/link/
- https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/ (example of `aria-current="page"` on the current link in a trail)
- https://www.w3.org/TR/wai-aria/#aria-current

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/link-purpose-in-context.html
- https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/#aria-current
