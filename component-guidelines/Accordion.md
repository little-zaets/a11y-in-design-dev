# Accordion

## Overview

**Type:** Composite widget
**Interaction Models:** Disclosure
**Equivalent HTML element:** None

An accordion is a vertically stacked set of interactive headings that each contain a title representing a section of content. The headings function as controls that enable users to reveal or hide their associated sections of content. Accordions reduce the need to scroll when presenting multiple sections of content on a single page.

Each accordion item consists of a heading element wrapping a trigger button, and a content panel associated with that trigger. Multiple panels may be expanded simultaneously, or the implementation may restrict expansion to one panel at a time.

### When to Use
- To present multiple sections of content on a single page where users typically need only one or two sections at a time.
- To reduce vertical scrolling by hiding content that is not immediately relevant.
- To give users an overview of available content sections via their headings, allowing them to choose which sections to expand.

### When Not to Use
- **Do not use an accordion when users need to see or compare content from multiple sections simultaneously.** Use separate sections or a single scrollable page instead.
- Do not use an accordion when the content in each section is very short (a sentence or two). Inline content or a definition list may be more appropriate.
- Do not use an accordion when only a single section of content needs to be shown or hidden. Use a disclosure (show/hide) pattern instead.
- Do not use an accordion when the content is sequential or depends on order. Use a step-based flow (wizard/stepper) instead.
- Do not use an accordion to switch between views where only one view is active at a time and the content is not hierarchically related. Use tabs instead.

### Accordion vs. Tabs

Both patterns organize content into labeled sections. An accordion stacks sections vertically and allows multiple sections to be expanded at the same time; each section header acts as a disclosure button using `aria-expanded`. Tabs display exactly one panel at a time and use arrow keys plus roving tabindex to navigate between tab elements. Accordions use heading elements wrapping buttons and the standard page Tab key to move between headers; tabs use `role="tablist"`, `role="tab"`, and `role="tabpanel"` with arrow-key navigation within the tab list. For roles and the keyboard model, see the Accessibility section below.

### Accordion vs. Disclosure

A disclosure (show/hide) is a single button that toggles visibility of one content section. An accordion is a group of related disclosures stacked vertically, where each heading contains a trigger button. If only one section needs expanding, use a disclosure. If multiple related sections are presented together with heading-level structure, use an accordion.

### Contained Components

This component is composite.

- **Accordion heading** — a heading element (`<h2>`–`<h6>`, or `role="heading"` with `aria-level`) that wraps the trigger button. Provides heading-level semantics for structural navigation and screen reader heading lists (equivalent HTML elements: `<h2>`–`<h6>`).
- **Accordion trigger** — a `<button>` element inside the heading that toggles the visibility of its associated panel. It is the only interactive element inside the heading (equivalent HTML element: `<button>`).
- **Accordion panel** — the container for content associated with a trigger. Optionally uses `role="region"` with `aria-labelledby` to create a landmark (no equivalent HTML element for this usage).

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (accordion heading, trigger, and panel, each split into role, properties, states) → **Accessible Name** (trigger and panel names) → **Keyboard** (keys and focus behavior) → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: Accordion trigger buttons must be focusable and operable using keyboard navigation. Each trigger button is a standard tab stop in the page's tab sequence.
- State: Programmatic state must match visual state at all times. The trigger's `aria-expanded` state and the panel's visibility must be synchronized.
- Accessible Name: Each trigger button must have a clear, descriptive accessible name. When panels use `role="region"`, each panel must have an accessible name via `aria-labelledby` referencing its trigger.
- Visible Focus Indicator: When a trigger button has focus, there must be a visible visual indicator such as an outline or border change.
- Color Contrast: Trigger text must meet contrast requirements. The focus indicator and any expand/collapse icons must meet non-text contrast requirements.
- Sufficient Target Size: Trigger buttons must meet a minimum touch target size of 24x24px.
- Heading Structure: Each trigger button must be wrapped in a heading element at the appropriate level for the page's information architecture. The button is the only element inside the heading.

### ARIA Roles, States, and Properties

#### Accordion heading: `<h#>` / `role="heading"`

##### Role

- **`heading`** — Each accordion header must be an element with the `heading` role. Native HTML heading elements (`<h2>`–`<h6>`) carry this role implicitly with the appropriate `aria-level`. Custom elements require `role="heading"` with an explicit `aria-level` value.
- **Required owned elements** — The heading must contain exactly one `<button>` element. No other interactive elements are permitted inside the heading. Other visually persistent elements (icons, badges) that are not part of the heading's accessible name must be placed outside the heading element.

##### Properties

- **`aria-level`** — When using a native heading element (`<h2>`–`<h6>`), the level is implicit. When using `role="heading"`, set `aria-level` to a value appropriate for the page's information architecture.

##### States

- No states apply to the heading element.

#### Accordion trigger: `<button>`

##### Role

- **`button`** — Native `<button>` elements carry this role implicitly. The button element is the sole child of the heading that provides the expand/collapse interaction.
- **Required context role** — The button must be contained within a heading element (a native `<h2>`–`<h6>` or an element with `role="heading"`).

##### Properties

- **Accessible name** — Each trigger button must have an accessible name, computed from its text content by default. It may also be provided with `aria-labelledby` or `aria-label`.
- **`aria-controls`** — Each trigger button must have `aria-controls` set to the `id` of its associated panel element.
- **Decorative icons** — Expand/collapse icons (e.g., plus/minus, chevron) inside the button are decorative because `aria-expanded` already communicates the state programmatically. Hide them from assistive technologies with `aria-hidden="true"` and `focusable="false"` (the latter addresses SVGs being focusable by default in some browsers).

##### States

- **`aria-expanded="true"` / `aria-expanded="false"`** — Indicates whether the associated panel is visible (`true`) or hidden (`false`). Must be set on the `<button>` element, never on the heading.
- **`aria-disabled="true"`** — When a trigger cannot be toggled (e.g., the panel must remain open), apply `aria-disabled="true"` to keep the button focusable and discoverable.

#### Accordion panel: `role="region"` (optional)

##### Role

- **`region`** — Optionally, each element that serves as a container for panel content has `role="region"`. This creates a landmark region that helps screen reader users perceive the panel as a distinct structural section. Avoid using `role="region"` when it would cause landmark region proliferation — for example, in an accordion with more than approximately six panels that can be expanded at the same time. The `region` role is especially helpful when panels contain heading elements or a nested accordion.

##### Properties

- **`aria-labelledby`** — When `role="region"` is used, set `aria-labelledby` to the `id` of the trigger button that controls the panel. The `region` role requires an accessible name to be identified as a landmark.

##### States

- **Hidden** — Collapsed panels must be hidden using the `hidden` attribute or `display: none`. Do not use `aria-hidden` as the sole hiding mechanism — this removes the panel from the accessibility tree but does not prevent keyboard interaction with its content.

### Accessible Name

This section expands the **Accessible name** bullets for both the **Accordion trigger** and each **Accordion panel** in the ARIA section above: text content, `aria-labelledby`, `aria-label`, and the link between trigger and panel.

#### Accordion trigger

- **Text content** is the default source: the inner text of the `<button>` element serves as the accessible name.
- **`aria-label`** provides a name directly on the button. Use only when no visible text exists or when visible text alone is ambiguous.
- **Do not duplicate visible text in `aria-label`** — some screen readers announce both, causing redundant output.
- **When multiple accordions exist on the same page** with similar heading text, ensure each trigger has a unique accessible name that clearly describes its section.

#### Accordion panel

- **`aria-labelledby`** referencing the associated trigger button is the naming mechanism for panels using `role="region"`. This creates a relationship: the trigger's `aria-controls` points to the panel, and the panel's `aria-labelledby` points back to the trigger.
- Each panel's accessible name is derived from its associated trigger's text content via this `aria-labelledby` reference.
- Panels without `role="region"` do not require an accessible name.

### Keyboard Interaction

Each accordion trigger button participates in the standard page tab sequence. Unlike tabs, accordion triggers do not use a roving tabindex pattern — each trigger is an independent tab stop.

| Key | Behavior |
|-----|----------|
| **Tab** | Moves focus to the next focusable element in the page tab sequence. All accordion trigger buttons and focusable elements within expanded panels are included in the tab sequence. |
| **Shift + Tab** | Moves focus to the previous focusable element in the page tab sequence. |
| **Enter** | When focus is on the trigger button of a collapsed panel, expands the associated panel. When focus is on the trigger button of an expanded panel, collapses the panel if the implementation supports collapsing. |
| **Space** | Same as Enter. |
| **Down Arrow** (optional) | When focus is on an accordion trigger, moves focus to the next accordion trigger. When focus is on the last trigger, optionally wraps to the first trigger. |
| **Up Arrow** (optional) | When focus is on an accordion trigger, moves focus to the previous accordion trigger. When focus is on the first trigger, optionally wraps to the last trigger. |
| **Home** (optional) | When focus is on an accordion trigger, moves focus to the first accordion trigger. |
| **End** (optional) | When focus is on an accordion trigger, moves focus to the last accordion trigger. |

Some implementations allow only one panel to be expanded at a time. In such implementations, expanding one panel automatically collapses the previously expanded panel.

### Color and Contrast

- **Text contrast:** Trigger button text must meet a minimum 4.5:1 contrast ratio against the button/heading background (WCAG 1.4.3). Large text (18px+ or 14px+ bold) requires 3:1.
- **Non-text contrast:** Expand/collapse icons (e.g., plus/minus, chevron, caret) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11). The visual boundary distinguishing accordion items must also meet 3:1 if it conveys structure.
- **Focus indicator contrast:** The focus ring on trigger buttons must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Do not rely solely on color to indicate state.** Expanded and collapsed states must be distinguishable through means other than color alone (e.g., icon rotation, icon change between plus/minus, border, background) (WCAG 1.4.1).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled triggers are still visually recognizable as interactive elements.

### Code Examples

Canonical good and bad HTML code examples for the Accordion component live in a separate file: [`./Examples/Accordion.md`](./Examples/Accordion.md).

## Checklist

### Markup
- [ ] Each trigger button is wrapped in a heading element (`<h2>`–`<h6>` or `role="heading"` with `aria-level`). [1.3.1, 4.1.2]
- [ ] The heading level is appropriate for the page's information architecture. [1.3.1, 2.4.6]
- [ ] The `<button>` element is the only interactive element inside the heading. [1.3.1, 4.1.2]
- [ ] Each trigger button has an accessible name — from text content, `aria-labelledby`, or `aria-label`. [1.1.1, 4.1.2]
- [ ] Each trigger button has `aria-controls` set to the `id` of its associated panel. [1.3.1, 4.1.2]
- [ ] When panels use `role="region"`, each panel has `aria-labelledby` referencing its trigger button's `id`. [1.3.1, 4.1.2]
- [ ] `role="region"` is not used when it would cause landmark proliferation (more than approximately six simultaneously expandable panels). [1.3.1]
- [ ] Expand/collapse icons inside the button are hidden from assistive technologies with `aria-hidden="true"` and `focusable="false"`. [1.1.1]

### States
- [ ] Each trigger button has `aria-expanded` set to `"true"` when its panel is visible and `"false"` when hidden. [4.1.2]
- [ ] `aria-expanded` is set on the `<button>` element, not on the heading. [4.1.2]
- [ ] Collapsed panels are hidden via the `hidden` attribute or `display: none` — not `aria-hidden` alone. [1.3.1, 4.1.2]
- [ ] Programmatic state (`aria-expanded`, panel visibility) matches visual state at all times. [4.1.2]
- [ ] Disabled triggers use `aria-disabled="true"` to remain focusable and discoverable. [4.1.2]

### Keyboard
- [ ] Each trigger button is focusable via Tab. [2.1.1, 2.4.3]
- [ ] Enter and Space toggle the associated panel's visibility. [2.1.1]
- [ ] Focus is not trapped — Tab and Shift+Tab move focus through triggers and expanded panel content in natural document order. [2.1.2]
- [ ] If optional arrow key navigation is implemented, Down/Up Arrow move focus between triggers and optionally wrap. [2.1.1]

### Visual
- [ ] Trigger button text meets 4.5:1 contrast ratio against the background (3:1 for large text). [1.4.3]
- [ ] Expand/collapse icons meet 3:1 contrast ratio against adjacent colors. [1.4.11]
- [ ] Expanded and collapsed states are distinguishable by more than color alone. [1.4.1]
- [ ] Focus outline is not removed without replacement. [2.4.7]
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content. [1.4.11]
- [ ] Touch target is at least 24x24px. [2.5.8]

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/accordion/
- https://www.w3.org/WAI/ARIA/apg/patterns/accordion/examples/accordion/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html
- https://www.w3.org/WAI/WCAG22/Understanding/headings-and-labels.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#heading
- https://www.w3.org/TR/wai-aria/#button
- https://www.w3.org/TR/wai-aria/#region
- https://www.w3.org/TR/wai-aria/#aria-expanded
- https://www.w3.org/TR/wai-aria/#aria-controls
- https://www.w3.org/TR/wai-aria/#aria-labelledby
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-level
- https://www.w3.org/TR/wai-aria/#aria-disabled
