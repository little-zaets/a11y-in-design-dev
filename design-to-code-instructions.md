# Figma Make: Development Guidelines

These guidelines define how Figma design metadata is used to classify elements and generate semantic, accessible HTML that conforms to **WCAG 2.2**. This means the generated code must be perceivable, operable, understandable, and robust for people with visual, motor, cognitive, and other disabilities.

The agent must use **all available metadata sources together** to determine each element's role, state, relationships, and accessible name.

## 1. Metadata Sources for Classification

Every element is classified by combining information from four complementary sources. None is sufficient alone — each contributes a different dimension of the accessibility picture.

### 1.1 Layer & Component Names
Names identify **what an element is** — its semantic role, element type, and structural position.

* **Component Type:** Names like `Accordion`, `Navbar`, or `Modal` dictate the semantic role and container structure.
* **Element Type:** Keywords like `Button`, `Link`, `List`, `Item` anywhere in the name indicate the HTML element (see Section 3).
* **Hierarchy:** Names like `First/Last`, `Parent/Child`, or `Slot` guide DOM nesting and relationship mapping.

### 1.2 Component Properties
Properties define **the current state and configuration** of a component instance.

* **State properties** such as `disabled`, `expanded`, `selected`, `checked`, or `active` map directly to ARIA state attributes (`aria-disabled`, `aria-expanded`, `aria-selected`, `aria-checked`, `aria-current`).
* **Label visibility properties** indicate whether a visible label is present. When a label property is set to hidden/false, the agent must look for the `AriaLabel` nested layer (see 1.3).
* All property values provided by the designer must be treated as authoritative — do not infer state from visual appearance when a property explicitly declares it.

### 1.3 `AriaLabel` Nested Layer
When a component has **no visible label**, designers provide an `AriaLabel` text layer nested inside the component. This text must be used as the `aria-label` for the interactive element.

* The `AriaLabel` value is set by the designer to reflect the component's specific function in context.
* This takes precedence over any name inferred from layer names or icons.
* If no `AriaLabel` layer is present and no visible label exists, flag this as a potential accessibility gap.

### 1.4 Component Description & Annotations
Descriptions and annotations provide **usage context, behavioral expectations, and design intent**.

* **Component descriptions** clarify what a component is for and how it should behave, which can inform role selection and ARIA attribute choices.
* **Figma annotations** may specify constraints, interaction patterns, or relationships between elements (e.g., that a tooltip is associated with a specific control, or that a set of tabs controls a panel region).
* Use this information to establish `aria-controls`, `aria-describedby`, `aria-owns`, and other relationship attributes.

---

## 2. Landmark & Semantic Structure
To maintain a clean and accessible DOM, follow the rule of **Minimal Landmarks**:

* **Top-Level Only:** Use high-level landmarks like `<main>`, `<header>`, and `<footer>` based on the overall frame structure.
* **Confidence Threshold:** Do not wrap elements in additional landmarks (like `<section>`, `<aside>`, or `<nav>`) unless the metadata sources enable you to identify them with **100% confidence**.
* **Default to Generic:** If a container's role is ambiguous across all metadata sources, default to a `<div>` to avoid misrepresenting the page structure to assistive technologies.

| Figma Layer Name | HTML Element |
| :--- | :--- |
| `Navigation`, `Navbar` | `<nav>` |
| `Header`, `Top Bar` | `<header>` |
| `Footer`, `Bottom Bar` | `<footer>` |
| `Main Content`, `Page` | `<main>` |

---

## 3. Element Selection Logic
Determine HTML elements using all metadata sources — layer names, component properties, descriptions, and annotations.

* **Prefer existing project components.** Before generating new markup, check whether the target project already provides an accessible component for the pattern. Use it rather than hand-rolling new markup. Code Connect mappings, when available, link design components directly to their codebase implementations.
* **Layer name terminology** is the starting signal: keywords like `Link`, `Button`, `List`, `Item` anywhere in the name map to `<a>`, `<button>`, `<ul>`/`<ol>`, `<li>`.
* **Component properties** may refine or override this (e.g., a property indicating the element functions as a link vs. a button).
* **Component descriptions and annotations** may clarify ambiguous cases (e.g., whether a clickable element navigates or triggers an action).

---

## 4. Accessibility Attribute Mapping

Combine all metadata sources to produce the correct accessibility attributes for each element.

### Accessible Name Resolution
1. **`AriaLabel` nested layer** — if present, use its text as the `aria-label`. This is the designer's explicit accessible name for the component.
2. **Visible label** — if the component has a visible text label, it serves as the accessible name (no `aria-label` needed).
3. **Icon-only controls** — if no `AriaLabel` layer exists and no visible label is present, deduce the accessible name from the icon's layer name, component description, annotations, or surrounding context.

### State Attributes
Map component property values to their ARIA equivalents (`aria-expanded`, `aria-disabled`, `aria-selected`, `aria-checked`, etc.).

### Relationships
Use component descriptions and annotations to establish relationships between elements via `aria-controls`, `aria-describedby`, `aria-labelledby`, and `aria-owns`.

### Images and Icons
The method for hiding decorative images or providing alternative text depends on the element type:

* **`<img>` elements:**
  * Decorative: set `alt=""` (empty alt attribute). Do not use `aria-hidden`.
  * Meaningful: set `alt` to a description of the image's content or function.
* **`<svg>` elements:**
  * Decorative: set `aria-hidden="true"` and `focusable="false"`.
  * Meaningful: add a `<title>` element inside the SVG and associate it with `aria-labelledby`, or use `role="img"` with an `aria-label`.
* **CSS background images / other non-semantic elements:**
  * Decorative: no additional markup needed — they are already invisible to assistive technologies.
  * Meaningful: add a visually hidden text alternative or use `role="img"` with an `aria-label` on the container.

---

## 5. Preservation of Metadata
To ensure the code remains maintainable and traceable:
* **Maintain `data-name` attributes:** Always include the original Figma layer names in the HTML as `data-name` attributes.
* **Design Tokens:** Use CSS variable notation for fills and colors (e.g., `var(--fill-0, #4285F4)`) to align with Figma's design tokens.

---

> **Note:** The goal is to build the *right* functionality based on designer documentation. Layer names, component properties, `AriaLabel` layers, descriptions, and annotations are all part of that documentation. Only implement what is explicitly signaled across these sources.
