# Accessible Code Generation Guidelines

All generated code must conform to **WCAG 2.2** Level AA. The generated code must be perceivable, operable, understandable, and robust for people with visual, motor, cognitive, and other disabilities.

For every element in the design, follow these two phases in order:

**Phase 1 — Classify** using the **Classify elements** section:

1. Match the element against the **Categories** list to determine its broad type.
2. For **standalone widgets**, consult the **Interaction model** table to determine the specific role and HTML element from the element's observed behavior.
3. For **composite widgets**, decompose the widget into its constituent elements, map each constituent through the **Interaction model** table, then match the resulting combination of roles to a known APG composite pattern (tabs, menu, listbox, combobox, grid, radiogroup, tree, toolbar). The composite stays whole — generate it as one unit under the **Composite widgets** subsection in Phase 2.
4. For **composite containers** (elements that compose multiple semantic parts but do **not** match any single APG pattern), decompose into constituents and classify each one individually using the Categories list. The container itself has no semantic role and does not carry forward into Phase 2 — only its constituents do.
5. Apply **What a screenshot does not reveal** (below). For any element whose function falls into those gaps and is not made unambiguous by the user prompt or whole-page context, add it to a **warnings** list. Do not infer activation behavior from appearance alone.
6. Use **Resolve ambiguity** to apply default resolution rules when function is unclear. Document each assumption in the warnings list.
7. **Warnings summary** — After steps 1–6, list every element whose function you **inferred** rather than **confirmed**. For each item, state the assumption made, the reasoning, and a confidence indicator (high / medium / low). Emit this as a warnings summary alongside the generated output — do not block implementation. Mark each unconfirmed classification in the generated markup with a comment (e.g., `<!-- a11y-warning: classified as link based on navigation context; activation behavior not confirmed -->`).

**Cautions** — The following signals are unreliable on their own and should not be the sole basis for classification. When no better signal is available, the agent may proceed using best judgment but must document the assumption in the warnings summary:

- Industry or "common" UI patterns (e.g. "a hamburger usually opens a drawer," "a cart icon usually goes to the cart page")
- Reasonable defaults or "practical" choices made to avoid blocking implementation
- Informal layer or frame names and visual grouping alone as proof of what happens on activation (structured component properties and designer-authored descriptions are valid evidence when consistent with apparent function — informal naming alone is not)
- Short prompts such as "create this design," "implement the import," or "build this screen" — they do not specify activation behavior

If Phase 1 produces a non-empty warnings list, document each item in the warnings summary and proceed to Phase 2 using the most accessibility-conformant classification for each unresolved item.

**Phase 2 — Generate markup** according to the **Rules for generated output** section:

1. Follow every rule in **Universal rules** when writing the element's markup. These apply regardless of category.
2. Read the subsection matching the element's classification and follow every rule in it when writing the element's markup:
   - **Landmarks** — for elements classified as page regions
   - **Standalone widgets** — for individual interactive controls
   - **Composite widgets** — for containers managing child widgets
   - **Window roles** — for dialog and alertdialog
   - **Document structure** — for headings, lists, tables, images, figures, articles
   - **Live regions** — for dynamic announcements
   - **Forms** — for form controls, labels, errors, grouping

---

## Classify elements

Determine each element's semantic role from its **function** — what happens when the element is activated or what purpose it serves in the page structure.

Use all available signals to identify that function. When the design source provides structured metadata, the following are valid evidence for classification:

- **Layer and component names** — signal which APG pattern or semantic HTML element to apply. Keywords anywhere in the name indicate the classification (e.g., `Accordion` points to the APG Accordion pattern, `Navigation` maps to `<nav>`, names containing `Link` map to `<a>`, names containing `Button` map to `<button>`).
- **Component properties / variants** — declare state and configuration (e.g., `disabled: true`, `expanded: false`, `selected: true`) that map to ARIA state attributes.
- **Dedicated accessible-name layers** (e.g., an `AriaLabel` text layer) — provide the designer's explicit accessible name when no visible label exists.
- **Component descriptions** — clarify usage context, expected behavior, and interaction patterns.
- **Annotations** — specify behavioral intent, constraints, or relationships between elements.
- **Documentation links** — reference implementation guidance or usage guidelines.
- **Design tokens / variables** — relevant for contrast verification.
- **Code Connect mappings** — link the design component to an existing code implementation.
- **Screenshot / visual output** — the element's apparent function in context.

Structured metadata (typed properties, descriptions, dedicated accessibility layers) is valid evidence when consistent with the element's apparent function. Colloquial terminology in user prompts or informal layer names (e.g., a user calling every control a "button") is not a classification signal — always determine the semantic role from the element's function, not from what the user calls it.

When structured metadata and apparent function **agree**, classification is confirmed. When they **conflict** (e.g., a layer named "Carousel" that renders a static grid with no rotation controls), document the conflict in the warnings summary and choose the classification that best matches the element's apparent function. When structured metadata is **absent**, classify from apparent function and document the assumption.

If the element's function is not evident from the available signals — for example, the user describes a control but does not specify what happens on activation — apply the **Resolve ambiguity** default rules, document the assumption, and proceed.

### What a screenshot does not reveal

A screenshot or static design shows **appearance**, not **activation behavior** or **purpose**. Structured metadata (component properties, descriptions, annotations) can fill some of these gaps when available — but do not treat visual inference alone as confirmed classification.

The following typically **cannot** be determined from visuals alone unless structured metadata, the user prompt, or full design context makes them unambiguous:

- **Interactive controls** — What happens on activation (navigate vs. perform action, open a modal that traps focus vs. inline disclosure, application menu vs. navigation dropdown, toggle vs. submit). Link vs. button, dialog vs. disclosure, and similar pairs require knowing the function.
- **Images** — Whether an image is decorative or meaningful depends on whether it conveys information not available from surrounding text and on page purpose — not on the image file alone. After you decide, express it in markup as in **Document structure — Images** (`<img>` + `alt`, or non-`img` image elements with `aria-label` / `aria-labelledby` or `aria-hidden="true"`).
- **Text styling** — Whether large or bold text is a **heading** or decorative styled text depends on whether it introduces a content section, not on font size alone.
- **Lists vs. layout** — Repeated visual elements may be a semantic list or a CSS layout; the relationship between items determines the markup.
- **Dynamic content** — A single frame does not show whether content updates after initial render (live regions) or stays static.
- **Landmark scope** — Whether a group of links belongs in a `<nav>` landmark or a `<ul>` in content depends on the links' purpose (site navigation vs. related links in content), not on visual grouping.
- **Accessible names for unlabeled controls** — When a control has no visible text label, the accessible name may be provided by a dedicated metadata layer (e.g., an `AriaLabel` text property) or must be deduced from the icon's meaning, component description, or surrounding context.

For every element that falls into one of these gaps and is not made unambiguous by the prompt or clear whole-page context, add it to the **warnings** list with the assumption made. Apply the **Resolve ambiguity** defaults and proceed. Examples where whole-page context can narrow ambiguity: a primary **Submit** on a form performs an in-page action; on a product detail page, a hero **product image** is often a meaningful image for `alt`.

### Categories

Match each element to one of the following categories:

1. **Landmarks** — page regions: `<header>`, `<nav>`, `<main>`, `<footer>`, `<aside>`, `<section>` (only when it has an accessible name), `<form>` (only when it has an accessible name), `<search>`.
2. **Composite widgets** — containers managing child widgets per a single [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/): tablist, menu, menubar, listbox, combobox, grid, radiogroup, tree, treegrid. The container has its own role and keyboard model; it stays whole through Phase 2.
3. **Standalone widgets** — individual interactive controls: link, button, checkbox, radio, switch, slider, spinbutton, tab, textbox, searchbox, menuitem, option, treeitem, progressbar.
4. **Document structure** — content elements: heading, paragraph, list, listitem, table, figure, article, blockquote, img, meter, separator, toolbar, tooltip.
5. **Live regions** — dynamic announcements: alert, status, log, timer.
6. **Window roles** — overlay containers: dialog, alertdialog.
7. **Wrapper/layout** — elements providing visual structure but no semantic meaning (a `<div>` centering children). These do not need semantic markup but may contain elements that do.
8. **Composite containers** — elements composing multiple semantic parts that do **not** match any single APG pattern (a card with text and a link, a footer with links). Unlike composite widgets, the container has no semantic role of its own. Decompose into constituents and classify each one individually using the categories above — only the constituents carry forward into Phase 2.

### Interaction model

For elements classified as **standalone widgets**, use this table to map the element's observed behavior to a specific semantic role and HTML element. For elements classified as **composite widgets**, decompose the widget into its constituent elements and map each one through this table individually — the combination of roles identifies the composite pattern (e.g., a container with tab-switching children and content panels → tabs pattern). Then consult the Composite widgets section for the full structure, ARIA, and keyboard requirements.

| Observed behavior | Semantic role | HTML element |
|---|---|---|
| Navigate to page/URL/anchor/download/mailto/tel | Link | `<a href="...">` |
| Perform action (submit, save, delete, confirm) | Button | `<button>` |
| Show/hide a panel | Disclosure button | `<button aria-expanded>` |
| Toggle on/off state (mute, bold, favorite) | Toggle button | `<button aria-pressed>` |
| Select one from a group | Radio group | `<fieldset>` + `<input type="radio">` |
| Select multiple from a group | Checkbox group | `<fieldset>` + `<input type="checkbox">` |
| Enter text | Input | `<input>`, `<textarea>` |
| Switch between content panels | Tabs | `role="tablist"` + `role="tab"` + `role="tabpanel"` |

### Resolve ambiguity

When the Categories list and Interaction model table do not produce a confident classification, apply these default resolution rules. Each rule specifies the **default** to use when the element's function is ambiguous. Document every default applied in the warnings summary.

**Link vs button** — A control that **navigates** to a different page, URL, section, or resource → **link** (`<a>`). A control that **performs an action** on the current page (submit, toggle, open dialog, delete) → **button** (`<button>`). **Default when ambiguous:** if the element appears to lead to another destination (contains a URL, points to another page, or is positioned in a navigation context), default to **link**. Otherwise, default to **button**.

**Heading vs styled text** — Large or bold text is a **heading** only if it introduces a content section. Purely decorative large text (a tagline, a pull quote) is not a heading. **Default when ambiguous:** if the text precedes a distinct content block, default to **heading**.

**List vs repeated elements** — A visual sequence of similar items (cards, tags, links) is a **list** (`<ul>`, `<ol>`) even when not visually styled with bullets. Do not use `<div>` sequences for grouped items. **Default:** treat repeated similar items as a **list**.

**Table vs layout grid** — A visual grid of aligned data with row/column relationships is a **table** (`<table>` with `<th>`). A visual grid used for layout (cards, image gallery) is CSS grid/flexbox with no table semantics. **Default when ambiguous:** if the grid contains headers or labeled columns, default to **table**. Otherwise, default to **layout**.

**Navigation landmark vs list of links** — A group of links is a `<nav>` landmark only when the links represent **site or page navigation**. A list of related links within content (e.g., social links, resource links in an article) is a `<ul>` inside whatever landmark contains it. **Default when ambiguous:** if the links appear in a header, footer, or sidebar position, default to **`<nav>`**. Otherwise, default to **`<ul>`**.

**Dialog vs disclosure** — An overlay that obscures the page and **traps focus** is a **dialog** (`role="dialog"`). An inline panel that expands in place without trapping focus is a **disclosure** (`<button aria-expanded>` + hidden content). **Default when ambiguous:** if the element appears as an overlay or modal, default to **dialog**. Otherwise, default to **disclosure**.

**Menu vs navigation dropdown** — A dropdown of **application actions** (cut, copy, paste, delete) is a **menu** (`role="menu"` with `role="menuitem"`, arrow key navigation). A dropdown of **navigation links** is a **disclosure** pattern (`<button aria-expanded>` with a list of `<a>` elements). Do not use `role="menu"` for site navigation. **Default:** default to **disclosure with links** unless the dropdown clearly contains application actions.

**Decorative vs meaningful image** — A graphic is **decorative** if removing it would not change the page's information or function; it is **meaningful** if it conveys information not available from surrounding text. For **`<img>`**, use `alt=""` when decorative and a non-empty `alt` when meaningful. For **image content not using `<img>`** (e.g. inline `<svg>`, `role="img"`), use `aria-label` or `aria-labelledby` when meaningful and `aria-hidden="true"` when decorative — see **Document structure — Images**. **Default when ambiguous:** if the image is the only content in an interactive element (link or button), treat it as **meaningful**. Otherwise, default to **decorative**.

**Section vs region landmark** — A `<section>` becomes a `region` landmark only when it has an **accessible name** and its content is important enough for users to navigate to. Not every `<section>` should be a landmark. **Default:** do not apply the `region` landmark unless the section has an explicit accessible name.

**Static content vs live region** — Content that updates dynamically after initial render needs a **live region** (`role="status"`, `role="alert"`) to announce changes. Content that renders once and does not change is static and needs no live region. **Default:** treat content as **static** unless metadata or context indicates dynamic updates.

---

## Warnings output format

When Phase 1 produces inferred classifications, include a structured warnings summary with the generated output. Each warning must contain:

1. **Element** — which element or component the warning applies to
2. **Assumption** — the classification chosen and why
3. **Confidence** — high, medium, or low
4. **Alternative** — what the classification would be under a different interpretation

Additionally, mark each affected element in the generated markup with an inline comment:

```html
<!-- a11y-warning: [brief description of assumption] -->
```

---

## Rules for generated output

All generated markup must conform to **WCAG 2.2** Level AA. Generate all markup according to the rules below. For every element, first follow the **Universal rules**, then follow the rules in the subsection matching the element's classification.

### Universal rules

These apply to every element in the generated output.

- **Prefer existing project components.** Before generating new markup for a pattern, check whether the target project already provides an accessible component for it (e.g., a Button, Dialog, or Tabs component in the project's component library or design system). Use the existing component rather than hand-rolling new markup. Code Connect mappings, when available, link design components directly to their codebase implementations.
- **Prefer semantic HTML** over ARIA. Native elements (`<a>`, `<button>`, `<input>`, `<select>`, `<nav>`, `<main>`, `<table>`, `<ul>`) provide built-in keyboard support, roles, and focus management.
- **Do not assign redundant roles.** Native elements already carry implicit roles. Do not add `role="button"` to `<button>`, `role="link"` to `<a href>`, `role="navigation"` to `<nav>`, `role="main"` to `<main>`, etc.
- **Accessible name required** on all interactive elements and named landmarks — from text content, `aria-labelledby`, `aria-label`, or `<label>`. For images and graphics, follow **Document structure — Images** (`<img>` + `alt`; non-`img` image markup with `aria-label` / `aria-labelledby` or `aria-hidden="true"`).
- **Do not duplicate visible text in `aria-label`.** Only use `aria-label` when no visible text exists or visible text is ambiguous.
- **Label in Name.** When `aria-label` is used on an element with visible text, the value must contain the visible text as a substring.
- **Keyboard accessible.** All interactive elements must be focusable and operable via keyboard.
- **No positive tabindex.** Use `tabindex="0"` for custom elements in natural tab order, `tabindex="-1"` for programmatic focus only.
- **Visible focus indicator.** Never remove `outline` without a replacement. Focus indicators must meet 3:1 contrast against adjacent colors.
- **Target size.** Interactive targets must be at least 24×24 CSS pixels.
- **No nested interactive elements.** Never place a link, button, or input inside another interactive element.
- **State must match.** Programmatic state (ARIA attributes) must match visual state at all times.
- **Text contrast:** 4.5:1 minimum (3:1 for large text: 18px+ or 14px+ bold).
- **Non-text contrast:** UI boundaries, state indicators, and meaningful graphics require 3:1.
- **Do not rely solely on color** to indicate state, errors, selection, or any information. Always add a non-color cue (underline, outline, icon, opacity, border, text).

### Landmarks

[APG Landmark Regions practice](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)

| HTML element | ARIA landmark role | Condition |
|---|---|---|
| `<header>` | `banner` | Only when not nested inside `<article>`, `<aside>`, `<main>`, `<nav>`, or `<section>` |
| `<nav>` | `navigation` | Always |
| `<main>` | `main` | One per page |
| `<footer>` | `contentinfo` | Only when not nested inside `<article>`, `<aside>`, `<main>`, `<nav>`, or `<section>` |
| `<aside>` | `complementary` | Always |
| `<section>` | `region` | Only when it has an accessible name (`aria-label` or `aria-labelledby`) |
| `<form>` | `form` | Only when it has an accessible name. Use `search` instead when the form is for search functionality. |
| `<search>` | `search` | Always. Use instead of `form` for search functionality. |

#### Structure

- All perceivable page content must be contained within a landmark region.
- Each page must have one `main` landmark. `banner`, `main`, `complementary`, and `contentinfo` must be top-level landmarks (not nested inside other landmarks).
- Landmarks can be nested to identify parent/child relationships (e.g., a `navigation` inside a `banner`).
- Aim for seven or fewer landmark regions per page. Value diminishes as the count grows.
- Do not wrap modal dialog content in a landmark — the dialog itself provides the container, name, and boundaries.

#### Labeling

- If a landmark role is used more than once on a page, each instance must have a unique label.
- If an area begins with a heading (`h1`–`h6`), use `aria-labelledby` to reference it as the label.
- If no heading is available, use `aria-label`.
- Do not include the landmark role in the label. A `<nav aria-label="Site Navigation">` is announced as "Site Navigation Navigation". Use `aria-label="Site"` instead.
- If two instances of the same landmark have identical content and purpose, they may share the same label.

#### Page-level rules

- Set page language: `<html lang="en">`.
- Provide a descriptive `<title>`.
- Include a skip link as the first focusable element targeting `<main>`.
- Heading hierarchy must reflect the page outline without skipped levels.
- DOM order must match visual reading order. Do not use CSS reordering that breaks logical sequence.
- Use relative units (`rem`, `em`, `%`) for font sizes. Text must be resizable to 200%.
- At 320 CSS px width, primary content must not require horizontal scrolling.

### Standalone widgets

For each standalone widget: use the native HTML element, provide an accessible name, and support the listed keyboard interactions.

**Link** — `<a href>`. Activates on Enter. Link text must describe the destination. `href` must not be `javascript:void(0)` or `#`. Use `aria-current="page"` for the current page in navigation sets. Use `aria-disabled="true"` for inactive links. [APG Link pattern](https://www.w3.org/WAI/ARIA/apg/patterns/link/)

**Button** — `<button>`. Activates on Enter and Space. Use `aria-pressed` for toggles (constant label). Use `aria-expanded` for disclosure. Use `aria-disabled="true"` (preferred) for disabled state. Do not use `aria-haspopup="menu"` for site navigation — use disclosure pattern instead. [APG Button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/)

**Checkbox** — `<input type="checkbox">`. Toggles on Space. Group related checkboxes with `<fieldset>` and `<legend>`. For tri-state, use `aria-checked="mixed"`. [APG Checkbox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/)

**Radio** — `<input type="radio">` inside `<fieldset>` with `<legend>`. Arrow keys move between options. Only one in the group is in the tab order. [APG Radio Group pattern](https://www.w3.org/WAI/ARIA/apg/patterns/radio/)

**Switch** — `<button role="switch">` or `<input type="checkbox" role="switch">`. Use `aria-checked` for on/off state. Toggles on Space (and Enter for button). [APG Switch pattern](https://www.w3.org/WAI/ARIA/apg/patterns/switch/)

**Slider** — `<input type="range">` or custom with `role="slider"`. Requires `aria-valuenow`, `aria-valuemin`, `aria-valuemax`. Arrow keys adjust value. [APG Slider pattern](https://www.w3.org/WAI/ARIA/apg/patterns/slider/)

**Spinbutton** — `<input type="number">` or `role="spinbutton"`. Arrow keys increment/decrement. Requires `aria-valuenow`, `aria-valuemin`, `aria-valuemax`. [APG Spinbutton pattern](https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/)

**Textbox** — `<input>` or `<textarea>`. Every input must have a visible, persistent label (`<label for>` or `aria-labelledby`). Placeholders are not labels. Use `autocomplete` for user-information fields.

### Composite widgets

For each composite: use the correct container/child role structure and implement the full keyboard model from the linked APG pattern.

**Tabs** — `role="tablist"` contains `role="tab"` elements; each tab controls a `role="tabpanel"`. Use `aria-selected` on tabs. Arrow keys move between tabs, Tab moves into the panel. [APG Tabs pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)

**Menu** — `role="menu"` contains `role="menuitem"` (or `menuitemcheckbox` / `menuitemradio`). Arrow keys navigate items, Enter activates, Escape closes. Only use for application-style action menus, not site navigation. [APG Menu pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/)

**Listbox** — `role="listbox"` contains `role="option"`. Use `aria-selected`. Arrow keys navigate, Enter selects. For multi-select, add `aria-multiselectable="true"`. [APG Listbox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/listbox/)

**Combobox** — `role="combobox"` with `aria-expanded` and an associated listbox popup. Arrow keys navigate the popup, Enter selects, Escape closes. [APG Combobox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/)

**Grid** — `role="grid"` contains rows and gridcells. Arrow keys navigate cells. Use for interactive tabular data, not static tables. [APG Grid pattern](https://www.w3.org/WAI/ARIA/apg/patterns/grid/)

**Tree** — `role="tree"` contains `role="treeitem"`. Use `aria-expanded` on parent nodes. Arrow keys navigate and expand/collapse. [APG Tree View pattern](https://www.w3.org/WAI/ARIA/apg/patterns/treeview/)

**Toolbar** — `role="toolbar"` groups controls. Arrow keys move between tools, Tab moves out of the toolbar. [APG Toolbar pattern](https://www.w3.org/WAI/ARIA/apg/patterns/toolbar/)

### Window roles

**Dialog** — `role="dialog"` with `aria-labelledby` pointing to a visible heading. Use `aria-modal="true"` for modal dialogs. Trap focus inside the dialog (Tab/Shift+Tab cycle within). Escape closes. On open, move focus into the dialog. On close, return focus to the trigger. [APG Dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)

**Alert dialog** — `role="alertdialog"` with `aria-labelledby` and `aria-describedby`. Same focus management as dialog. Use when the dialog conveys an urgent message requiring immediate acknowledgment. [APG Alert Dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/alertdialog/)

### Document structure

- **Headings** — Use `<h1>` through `<h6>`. One `<h1>` per page context. Do not skip levels.
- **Lists** — Use `<ul>`, `<ol>`, or `<dl>` for grouped items. Do not simulate lists with `<div>` sequences.
- **Tables** — Use `<table>`, `<th scope="col">` / `<th scope="row">`, and `<caption>` for static tabular data. Use grid role only for interactive tables.
- **Images** — **`<img>`** — Every `<img>` has an `alt` attribute: a **non-empty** string if the image is **meaningful** (describes purpose or action, not surface appearance), or **`alt=""`** if **decorative**. When the `<img>` is inside a link or button, `alt` describes that control's destination or action. **Image markup that is not `<img>`** (e.g. inline `<svg>`, a wrapper with `role="img"`, or other elements standing in for a picture or icon) — If **meaningful**, provide an accessible name with **`aria-label`** or **`aria-labelledby`**. If **decorative** (or the name is supplied by a parent control), use **`aria-hidden="true"`** so assistive technologies skip it. For **inline `<svg>`**, add **`focusable="false"`** (in addition to `aria-hidden` when decorative) so decorative graphics are not focusable. Do not rely on `role="presentation"` alone to hide complex graphics; prefer `aria-hidden="true"` for non-`img` decorative content.
- **SVG icons** — When decorative or when a parent link/button already provides the accessible name: `aria-hidden="true"` and `focusable="false"`. When the SVG alone must convey the name (e.g. icon-only control), use `aria-label` on the control or a child title/desc pattern per platform conventions.
- **Figures** — Wrap with `<figure>` and provide `<figcaption>`.
- **Articles** — Use `<article>` for self-contained content (blog post, comment, card) that makes sense independently.

### Live regions

- **Alert** — `role="alert"` (assertive). Use for urgent messages requiring immediate attention (errors, session expiry). Announced immediately, interrupts current speech.
- **Status** — `role="status"` (polite). Use for non-urgent updates (search result count, save confirmation, progress). Announced at the next pause.
- **Log** — `role="log"`. Use for sequential updates where new entries are appended (chat, activity feed). Announced at the next pause.
- Do not move focus to live region content — it must be announced without focus change.
- The live region container must exist in the DOM before content is injected.

### Forms

- Every form control must have a visible, persistent label. Placeholders are not labels.
- Associate labels programmatically: `<label for="id">`, `aria-label`, or `aria-labelledby`.
- Group related controls with `<fieldset>` and `<legend>`.
- Use `autocomplete` for user-information fields (name, email, tel, address).
- Mark required fields with `aria-required="true"` and a visible indicator (not color alone).
- Mark invalid fields with `aria-invalid="true"`.
- Associate error messages with `aria-describedby`. Error text must be visible — not just a color change.
- **Inline field errors:** associate the error message with the control via `aria-describedby` and place the error text inside a container with `aria-live="polite"` so it is announced without interrupting the user.
- **Form-level error summaries:** use `role="alert"` or move focus to the summary for submission-level errors where the user needs immediate redirection.
- Do not auto-submit or navigate on focus or input change without prior warning.

---

## References

- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)
- [WCAG 2.2 Understanding documents](https://www.w3.org/WAI/WCAG22/Understanding/)
- [WAI-ARIA 1.2 specification](https://www.w3.org/TR/wai-aria/)
- [WAI-ARIA Authoring Practices Guide — Patterns](https://www.w3.org/WAI/ARIA/apg/patterns/)
