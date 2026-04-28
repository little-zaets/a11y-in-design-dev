# Tabs

## Overview

**Type:** Composite widget
**Interaction Models:** Single Select
**Equivalent HTML element:** None

Tabs are a set of layered sections of content — known as tab panels — that display one panel at a time. Each tab panel has an associated tab element that, when activated, displays the panel. The list of tab elements is arranged along one edge of the currently displayed panel, most commonly the top edge.

### When to Use
- To organize related content into discrete panels where only one panel needs to be visible at a time.
- When all panel content is closely related to the same topic or entity (e.g., profile, settings, billing for the same account).
- When users do not need to compare content across panels simultaneously.

### When Not to Use
- **Do not use tabs when users need to see or compare content from multiple panels at the same time.** Use separate sections or a single scrollable page instead.
- Do not use tabs when the content is sequential or depends on order. Use a step-based flow (wizard/stepper) instead.
- Do not use tabs for content that should be expanded and collapsed in place. Use an accordion (disclosure pattern) instead.
- Do not use tabs for site or page navigation. Use links and a `<nav>` landmark instead.

### Tabs vs. Accordion

Both patterns organize content into sections. Tabs display exactly one panel at a time and use arrow keys to navigate between options; an accordion allows multiple sections to be expanded simultaneously and each section header acts as a disclosure button. For roles and the keyboard model, see **ARIA Roles, States, and Properties** and **Keyboard Interaction** under **Accessibility** below.

### Contained Components

This component is composite.

- **Tab list** — container for all tabs (`role="tablist"`, no equivalent HTML element).
- **Tab** — an individual element that serves as a label for one of the tab panels and can be activated to display that panel (`role="tab"`, no equivalent HTML element).
- **Tab panel** — the element that contains the content associated with a tab (`role="tabpanel"`, no equivalent HTML element).

## Accessibility

Subsections follow this order: **Core Rules** (summary) → **ARIA** (tab list, tab, and tab panel, each split into role, properties, states) → **Accessible Name** (tab list and tab panel names) → **Keyboard** (within the tab list, between tab list and tab panel) → **Color and Contrast** → **Code Examples** → **Checklist**.

### Core Rules
- Keyboard Accessibility: The tab list must be navigable using arrow keys. Tab (key) moves focus into and out of the tab list. The tab list acts as a single tab stop.
- State: Programmatic state must match visual state at all times. The selected tab's `aria-selected` state and panel visibility must be synchronized.
- Accessible Name: The tab list must have an accessible name. Each tab provides the accessible name for its associated tab panel via `aria-labelledby`.
- Visible Focus Indicator: When a tab has focus, there must be a visible visual indicator such as an outline or border change. Focus and selection must be separately perceivable.
- Color Contrast: Tab label text and the tab boundary must meet contrast requirements.
- Sufficient Target Size: Tabs must meet a minimum touch target size of 24x24px.
- Roving Tabindex: The active tab has `tabindex="0"`; all inactive tabs have `tabindex="-1"`. This ensures only the active tab is in the page tab sequence.

### ARIA Roles, States, and Properties

#### Tab list: `role="tablist"`

##### Role

- **`tablist`** — The element that serves as the container for the set of tabs must have `role="tablist"`.

##### Properties

- **Accessible name** — The tab list must have an accessible name. When a visible label exists, use `aria-labelledby` referencing that element. Otherwise, use `aria-label`.
- **`aria-orientation`** — When the tab list is vertically oriented, set `aria-orientation="vertical"`. The default value is `horizontal` and does not need to be explicitly set for horizontal tab lists.

##### States

- No states apply to the tab list container.

#### Tab: `role="tab"`

##### Role

- **`tab`** — Each element that serves as a tab must have `role="tab"` and must be contained within the element with `role="tablist"`.

##### Properties

- **Accessible name** — Each tab receives its accessible name from its text content. It may also be provided with `aria-labelledby` or `aria-label`.
- **`aria-controls`** — Each tab must have `aria-controls` set to the `id` of its associated `tabpanel` element.
- **`aria-haspopup`** — When a tab has an associated popup menu, set `aria-haspopup` to `menu` or `true`.

##### States

- **`aria-selected="true"`** — The active tab has `aria-selected="true"`. All other tabs have `aria-selected="false"`.
- **`tabindex`** — The active tab has `tabindex="0"` (or relies on native focusability when using `<button>`). All inactive tabs have `tabindex="-1"`. This roving tabindex pattern ensures only one tab is in the page tab sequence.

#### Tab panel: `role="tabpanel"`

##### Role

- **`tabpanel`** — Each element that contains the content panel for a tab must have `role="tabpanel"`.

##### Properties

- **`aria-labelledby`** — Each tab panel must have `aria-labelledby` set to the `id` of its associated tab element. This provides the tab panel's accessible name.
- **`tabindex="0"`** — When a tab panel does not contain any focusable elements or the first element with content is not focusable, the tab panel should have `tabindex="0"` to include it in the tab sequence of the page.

##### States

- **Hidden** — Inactive tab panels must be hidden from the DOM using the `hidden` attribute or `display: none`. Do not use `aria-hidden` as the sole mechanism to hide inactive panels — this removes them from the accessibility tree but does not prevent interaction with their content.

### Accessible Name

This section expands the **Accessible name** (and related) bullets for both the **Tab list** and each **Tab panel** in the ARIA section above: visible labels, `aria-labelledby`, `aria-label`, and the bidirectional link between tabs and panels.

#### Tab list

- **`aria-labelledby`** references a visible heading or other text element that describes the purpose of the set of tabs. Use when a visible label exists.
- **`aria-label`** provides a name directly on the tab list container. Use only when no visible label is available.
- **Do not include the word "tabs" in the label.** The `tablist` role already conveys the component type. A label like "Account settings tabs" is announced redundantly as "Account settings tabs, tab list."

#### Tab panel

- **`aria-labelledby`** referencing the associated tab element is the required naming mechanism. This creates a bidirectional link: the tab's `aria-controls` points to the panel, and the panel's `aria-labelledby` points back to the tab.
- Each tab panel's accessible name is derived from its associated tab's text content via this `aria-labelledby` reference.

### Keyboard Interaction

Tabs support two activation models: **automatic** (the panel activates when a tab receives focus via arrow keys) and **manual** (arrow keys move focus between tabs, but the user must press Enter or Space to activate the focused tab). Automatic activation is recommended when panels can be displayed instantly — that is, all panel content is present in the DOM. Manual activation is recommended when displaying a panel requires a network request or other delay.

#### Within the tab list

| Key | Behavior |
|-----|----------|
| **Tab** | When focus moves into the tab list, places focus on the active tab element. When the tab list already contains focus, moves focus to the next element in the page tab sequence outside the tab list, which is the tab panel (unless the first element inside the tab panel is focusable). |
| **Right Arrow** (horizontal) | Moves focus to the next tab. If focus is on the last tab, moves focus to the first tab. Optionally activates the newly focused tab (automatic activation). |
| **Left Arrow** (horizontal) | Moves focus to the previous tab. If focus is on the first tab, moves focus to the last tab. Optionally activates the newly focused tab (automatic activation). |
| **Down Arrow** (vertical) | When `aria-orientation="vertical"`, performs the same function as Right Arrow in horizontal orientation. |
| **Up Arrow** (vertical) | When `aria-orientation="vertical"`, performs the same function as Left Arrow in horizontal orientation. |
| **Space / Enter** | Activates the focused tab if it was not activated automatically on focus (manual activation). |
| **Home** (optional) | Moves focus to the first tab. Optionally activates the newly focused tab. |
| **End** (optional) | Moves focus to the last tab. Optionally activates the newly focused tab. |
| **Shift + F10** | If the tab has an associated popup menu, opens the menu. |
| **Delete** (optional) | If deletion is allowed, deletes the current tab and its associated panel, then moves focus to the next tab (or the previous tab if the deleted tab was last). |

#### Between tab list and tab panel

After a tab is activated, pressing **Tab** moves focus from the active tab into the associated tab panel's content (or to the tab panel itself if `tabindex="0"` is set and no focusable content exists). Pressing **Shift + Tab** from within the tab panel returns focus to the active tab.

### Color and Contrast

- **Text contrast:** Tab label text must meet a minimum 4.5:1 contrast ratio against the tab background (WCAG 1.4.3). Large text (18px+ or 14px+ bold) requires 3:1.
- **Non-text contrast:** The visual indicator distinguishing the active tab from inactive tabs (e.g., border, background, underline) must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Focus indicator contrast:** The focus ring must meet a minimum 3:1 contrast ratio against adjacent colors (WCAG 1.4.11).
- **Do not rely solely on color to indicate state.** Active, inactive, focused, and disabled states must be distinguishable through means other than color alone (e.g., border weight, position change, underline, icon) (WCAG 1.4.1).
- **Focus and selection must be separately perceivable.** In high contrast modes, ensure users can distinguish which tab is focused from which tab is selected. Use separate visual treatments (e.g., a focus ring distinct from the selection indicator).
- **Disabled state contrast:** While WCAG does not require disabled elements to meet contrast minimums, ensure disabled tabs are still visually recognizable as tabs.

### Code Examples

Canonical good and bad HTML code examples for the Tabs component live in a separate file: [`./Examples/Tabs.md`](./Examples/Tabs.md).

## Checklist

### Markup
- [ ] The tab list container has `role="tablist"`. [1.3.1, 4.1.2]
- [ ] The tab list has an accessible name via `aria-labelledby` or `aria-label`. [1.3.1, 4.1.2]
- [ ] Each tab has `role="tab"` and is contained within the `role="tablist"` element. [1.3.1, 4.1.2]
- [ ] Each tab has `aria-controls` set to the `id` of its associated tab panel. [1.3.1, 4.1.2]
- [ ] Each tab panel has `role="tabpanel"`. [1.3.1, 4.1.2]
- [ ] Each tab panel has `aria-labelledby` set to the `id` of its associated tab. [1.3.1, 4.1.2]
- [ ] Tab panels that lack focusable content have `tabindex="0"`. [2.1.1, 2.4.3]
- [ ] Vertical tab lists set `aria-orientation="vertical"`. [1.3.1, 4.1.2]

### States
- [ ] The active tab has `aria-selected="true"`; all other tabs have `aria-selected="false"`. [4.1.2]
- [ ] Inactive tabs have `tabindex="-1"`; the active tab has `tabindex="0"` (roving tabindex). [4.1.2]
- [ ] Inactive tab panels are hidden via `hidden` attribute or `display: none` — not `aria-hidden` alone. [1.3.1, 4.1.2]
- [ ] Programmatic state (`aria-selected`, panel visibility) matches visual state at all times. [4.1.2]

### Keyboard
- [ ] Tab (key) moves focus into the tab list (onto the active tab) and out of the tab list (into the active panel). [2.1.1, 2.4.3]
- [ ] Arrow keys move focus between tabs, wrapping at both ends. [2.1.1]
- [ ] For manual activation, Space or Enter activates the focused tab. [2.1.1]
- [ ] Focus is not trapped — Tab and Shift+Tab move focus into and out of the tab list and tab panel. [2.1.2]
- [ ] Vertical tab lists use Up/Down Arrow instead of Left/Right Arrow. [2.1.1]

### Visual
- [ ] Tab label text meets 4.5:1 contrast ratio against the tab background (3:1 for large text). [1.4.3]
- [ ] The active tab indicator meets 3:1 contrast ratio against adjacent colors. [1.4.11]
- [ ] Active, inactive, focused, and disabled states are distinguishable by more than color alone. [1.4.1]
- [ ] Focus and selection are separately perceivable. [1.4.11]
- [ ] Focus outline is not removed without replacement. [2.4.7]
- [ ] The focus indicator provides a 3:1 contrast ratio with surrounding content. [1.4.11]
- [ ] Touch target is at least 24x24px. [2.5.8]

---

## Design Pattern References
- https://www.w3.org/WAI/ARIA/apg/patterns/tabs/
- https://www.w3.org/WAI/ARIA/apg/patterns/tabs/examples/tabs-automatic/
- https://www.w3.org/WAI/ARIA/apg/patterns/tabs/examples/tabs-manual/

## WCAG 2.2 References
- https://www.w3.org/WAI/WCAG22/Understanding/info-and-relationships.html
- https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
- https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
- https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html
- https://www.w3.org/WAI/WCAG22/Understanding/no-keyboard-trap.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html
- https://www.w3.org/WAI/WCAG22/Understanding/focus-visible.html
- https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html
- https://www.w3.org/WAI/WCAG22/Understanding/name-role-value.html

## WAI-ARIA References
- https://www.w3.org/TR/wai-aria/
- https://www.w3.org/TR/wai-aria/#tablist
- https://www.w3.org/TR/wai-aria/#tab
- https://www.w3.org/TR/wai-aria/#tabpanel
- https://www.w3.org/TR/wai-aria/#aria-selected
- https://www.w3.org/TR/wai-aria/#aria-controls
- https://www.w3.org/TR/wai-aria/#aria-orientation
- https://www.w3.org/TR/wai-aria/#aria-haspopup
- https://www.w3.org/TR/wai-aria/#aria-label
- https://www.w3.org/TR/wai-aria/#aria-labelledby
