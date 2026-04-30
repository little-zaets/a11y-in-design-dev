---
name: component-a11y-workflow
description: This skill should be used when the user asks to "create a component", "edit a component", "modify a component", "add ARIA", "check accessibility", "review accessibility", "a11y review", "fix accessibility", or when working with files under `src/components/**` or `src/pages/**`. Provides the mandatory phased workflow for classifying components, matching them to rule files in `documentation/components/`, and verifying accessibility conformance.
---

# Component Accessibility Workflow

This skill governs agent behavior when creating, editing, or reviewing UI code. Read it fully before editing any file under `${ui.componentRoots}` (default `src/components/**`), `${ui.pageRoots}` (default `src/pages/**`), or under `${docs.componentRules}` (default `documentation/components/`).

All component code governed by this workflow must conform to **WCAG 2.2** Level AA. The generated and reviewed code must be perceivable, operable, understandable, and robust for people with visual, motor, cognitive, and other disabilities.

## Project paths

This skill uses the following named paths. Defaults match the layout of this repo. To use this skill in another project, fork it and replace the values below — every reference in the workflow body resolves through these names.

- `${ui.componentRoots}` — root(s) for component source files. Default: `src/components/**`
- `${ui.pageRoots}` — root(s) for page-level source files. Default: `src/pages/**`
- `${docs.componentRules}` — directory containing component rule files (PascalCase `*.md`). Default: `documentation/components/`
- `${docs.componentExamples}` — directory containing example files. Default: `documentation/components/Examples/`
- `${docs.authoringGuide}` — the rule authoring guide. Default: `documentation/DOCUMENTATION_GUIDE.md`

## When this workflow applies

- **Creating or editing UI code** under `src/components/**` or `src/pages/**` — follow the Component Accessibility Workflow below and consult the rule library in `documentation/components/`.
- **When unsure about semantics** (link vs button, dialog vs dropdown, tabs vs accordion, etc.) — stop and ask clarifying questions before implementing behavior.

---

## Scope discipline (required)

Apply only what the user explicitly requested. Do not bundle in related "improvements," refactors, comments, renames, or accessibility fixes — even when they seem helpful or are surfaced by the Component Accessibility Workflow.

- **Approvals are scope-bound.** "Yes", "proceed", or "go ahead" authorizes only the originally scoped change. It does not extend to additional changes surfaced during the task.
- **Report, don't bundle.** Violations or improvements discovered mid-task must be reported *separately*, after completing the requested change. Do not apply them in the same edit, and do not describe them as part of the user's original request.
- **Per-fix approval.** When reporting multiple proposed changes, list each as a numbered option. A blanket "yes" is not sufficient — ask the user to specify which numbered items to apply.
- **When in doubt, ask.** If unsure whether something falls inside or outside the requested scope, ask before acting.

---

## Component Accessibility Workflow (required)

The canonical rule library lives in `${docs.componentRules}*.md` (default `documentation/components/*.md`, PascalCase filenames like `Button.md`, `AlertDialog.md`, `RadioGroup.md`).

When creating, modifying, or reviewing components/pages, follow these phases in order.

**Order of operations relative to the requested change:**
0. Run Step 0 (prefer existing primitives) — if a library primitive applies, use it and skip the hand-roll work for that component.
1. Read the code and run Phase 1 (classify) and Phase 2 (match to rule files) *before* editing.
2. Apply the user's requested change (only what was explicitly requested — see Scope discipline).
3. Run Phase 3 (verify) against the post-change state.
4. Report any violations separately, as numbered options awaiting per-fix approval. Do not apply fixes unless the user approves each one.

### Step 0 — Prefer existing accessible primitives

Before classifying a component (Phase 1) or generating new component code, check whether the project already provides an accessible primitive for the pattern in question.

1. Inspect the project's dependencies (e.g., `package.json`) and `src/` imports for established accessible UI libraries: Radix UI, Headless UI, Reach UI, Ariakit, React Aria / React Aria Components, Material UI, Mantine, Chakra UI, shadcn/ui (Radix-based), Bootstrap, and similar.
2. If such a library is in use and provides the pattern needed (e.g., Radix `Tabs`, Headless UI `Dialog`, React Aria `useCheckbox`), use the library primitive rather than hand-rolling the markup, ARIA, and keyboard interaction.
3. Verify the library version actually implements the relevant ARIA pattern. Some libraries are partially accessible (e.g., older releases lack focus management). Check the library's release notes if uncertain.
4. If no library primitive is available, or the project is unstyled and uses native HTML, proceed to Phase 1 as normal.

This step is preventive, not advisory: do not propose hand-rolled markup for a pattern the project's UI library already handles correctly.

### Phase 1 — Identify and classify components

Determine each component's semantic role based on its **rendered output**, not its filename or the task prompt alone.

#### Step 1 — Read the code

Before classifying, read the component file and any sub-components it imports. Identify:

- What HTML elements are rendered (`<a>`, `<button>`, `<ul>`, `<nav>`, `<footer>`, `<div>`, etc.)
- What ARIA roles/attributes are present (`role="dialog"`, `aria-expanded`, etc.)
- What event handlers are attached and what they do (navigate, toggle state, submit, etc.)
- What the rendered output structure looks like (a single interactive element? a container with multiple children?)

**Caution:** Component filenames may not reflect the actual ARIA pattern. Always verify by reading the code. For example, a file named `Carousel.jsx` that renders a static flex grid of cards is *not* an ARIA carousel — it has no rotation, no prev/next controls, and no `role="region"`. Classify based on rendered behavior, not the filename.

#### Step 2 — Classify into categories

Place each component (or constituent part) into one of these categories:

1. **Landmarks** — page regions: `<header>`, `<nav>`, `<main>`, `<footer>`, `<aside>`, `<section>` (only when it has an accessible name), `<form>` (only when it has an accessible name), `<search>`.
2. **Composite widgets** — containers managing child widgets per a single [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/): tablist, menu, menubar, listbox, combobox, grid, radiogroup, tree, treegrid, toolbar. The container has its own role and keyboard model; it stays whole through Phase 2 and Phase 3.
3. **Standalone widgets** — individual interactive controls: link, button, checkbox, radio, switch, slider, spinbutton, tab, textbox, searchbox, menuitem, option, treeitem, progressbar. Determine the interaction model for each (see Interaction model table below).
4. **Document structure** — content elements: heading, paragraph, list, listitem, table, figure, article, blockquote, img, meter, separator, tooltip.
5. **Live regions** — dynamic announcements: alert, status, log, timer.
6. **Window roles** — overlay containers: dialog, alertdialog.
7. **Wrapper/layout** — components that provide structure but no semantic meaning (e.g., a `<div>` that centers children). These may not need a rule file themselves but may contain elements that do.
8. **Composite containers** — components that compose multiple semantic elements without matching a single APG pattern (e.g., a list of links, a footer with links and a landmark, a card with static text and a link). These require decomposition (Step 3).

#### Interaction model

For elements classified as **standalone widgets**, use this table to verify the element's rendered behavior matches the correct semantic role. For elements classified as **composite widgets**, decompose the widget into its constituent elements and map each one through this table individually — the combination of roles identifies the composite pattern.

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

#### Step 3 — Decompose composites

For any component classified as a **composite container** or **wrapper/layout component**, decompose it into its constituent parts:

- List each semantic element or interactive control the component renders.
- Classify each constituent individually using the categories above.
- Each constituent maps independently to rule files in Phase 2.

For example, `SocialList.jsx` renders `<ul>`, `<li>`, and `<a>` elements — the `<a>` elements are standalone widgets (links) and are classified independently.

#### Step 4 — Resolve ambiguity

If classification confidence is below 100% for any component or constituent:

- Summarize what has been identified.
- List what is undetermined.
- Ask the user targeted questions (one per undetermined component) and **do not proceed** until answered.

Use the decision rules below to identify *which* ambiguity applies. Each rule describes the question to ask — it does not supply the answer when behavior is unstated.

**Link vs button** — A control that **navigates** to a different page, URL, section, or resource → **link** (`<a>`). A control that **performs an action** on the current page (submit, toggle, open dialog, delete) → **button** (`<button>`). If you do not know which applies, ask — do not guess from styling.

**Heading vs styled text** — Large or bold text is a **heading** only if it introduces a content section. Purely decorative large text (a tagline, a pull quote) is not a heading.

**List vs repeated elements** — A visual sequence of similar items (cards, tags, links) is a **list** (`<ul>`, `<ol>`) even when not visually styled with bullets. Do not use `<div>` sequences for grouped items.

**Table vs layout grid** — A visual grid of aligned data with row/column relationships is a **table** (`<table>` with `<th>`). A visual grid used for layout (cards, image gallery) is CSS grid/flexbox with no table semantics.

**Navigation landmark vs list of links** — A group of links is a `<nav>` landmark only when the links represent **site or page navigation**. A list of related links within content (e.g., social links, resource links in an article) is a `<ul>` inside whatever landmark contains it.

**Dialog vs disclosure** — An overlay that obscures the page and **traps focus** is a **dialog** (`role="dialog"`). An inline panel that expands in place without trapping focus is a **disclosure** (`<button aria-expanded>` + hidden content).

**Menu vs navigation dropdown** — A dropdown of **application actions** (cut, copy, paste, delete) is a **menu** (`role="menu"` with `role="menuitem"`, arrow key navigation). A dropdown of **navigation links** is a **disclosure** pattern (`<button aria-expanded>` with a list of `<a>` elements). Do not use `role="menu"` for site navigation.

**Decorative vs meaningful image** — A graphic is **decorative** if removing it would not change the page's information or function; it is **meaningful** if it conveys information not available from surrounding text. For `<img>`, use `alt=""` when decorative and a non-empty `alt` when meaningful. For non-`<img>` image content (e.g. inline `<svg>`, `role="img"`), use `aria-label` or `aria-labelledby` when meaningful and `aria-hidden="true"` when decorative.

**Section vs region landmark** — A `<section>` becomes a `region` landmark only when it has an **accessible name** and its content is important enough for users to navigate to. Not every `<section>` should be a landmark.

**Static content vs live region** — Content that updates dynamically after initial render needs a **live region** (`role="status"`, `role="alert"`) to announce changes. Content that renders once and does not change is static and needs no live region.

If none of these decision rules resolve the ambiguity, ask the user a targeted clarifying question. The question must identify what is undetermined and offer the specific options (e.g., "Does 'Forgot password?' navigate to a separate page, or does it open a form on the current page?").

### Phase 2 — Match to rule files

Using the classified components and constituents from Phase 1, determine which rule files apply.

1. List all files in `${docs.componentRules}` (default `documentation/components/`), excluding the `Examples/` subdirectory.
2. Identify relevant component types using **all three** of these methods:
   a. **From the current filename** — strip common prefixes and suffixes to extract the base pattern name:
      - `SubmitButton.jsx` → type is **button** → `Button.md`
      - `PrimaryButton.tsx` → type is **button** → `Button.md`
      - `CheckboxGroup.vue` → type is **checkbox** → `Checkbox.md`
      - `AlertDialog.jsx` → type is **alert-dialog** → `AlertDialog.md`
      - `RadioGroup.tsx` → type is **radio-group** → `RadioGroup.md`
   b. **From Phase 1 classification** — every standalone widget, composite widget, landmark, window role, and live region identified (including constituents from decomposition) maps to a potential rule file. Search `documentation/components/` for a match.
   c. **Fallback when no pattern keyword is found** — if neither the filename nor any suffix/prefix maps to a known pattern (e.g., `Hero.jsx`, `CardInfo.jsx`, `Content.jsx`):
      1. Use the Phase 1 classification (from code reading) to determine what semantic elements the component renders.
      2. Match each rendered semantic element to rule files (e.g., if `CardInfo` renders an `<a>` tag, `Link.md` applies).
      3. If the component is a pure layout wrapper with no semantic or interactive elements, no rule file is required — note this explicitly and move on.

A single component file may require **multiple rule files** when it composes several semantic elements. For example:

- `SocialList.jsx` renders `<ul>`, `<li>`, and `<a>` elements → `Link.md` applies to each link.
- `Footer.jsx` renders `<footer>` (landmark) + `<ul>` + `<a>` elements → `Link.md` applies to the links; landmark rules apply to the `<footer>`.
- `App.jsx` renders `<nav>`, `<button>`, and router `<Link>` → `Button.md`, `Link.md`, and landmark rules all apply.

Rules:

- Each type must match a filename in `documentation/components/` (PascalCase, e.g., `Button.md`, `Checkbox.md`, `AlertDialog.md`).
- Read **every** matching rule file in full. **Multiple rule files may apply** to a single file.
- For each matching rule file, also read its sibling examples file in `${docs.componentExamples}` (default `documentation/components/Examples/`) (e.g., `Button.md` → `Examples/Button.md`). The examples file contains the canonical good and bad code patterns for that component and is a required input for Phase 3 verification. If the examples file is missing, note it and proceed with the rule file alone.

**Outcome of Phase 2:**

After matching, produce a summary listing every classified constituent alongside its matched rule file (or "no match"). Then:

- **Constituents with matches** → proceed to Phase 3 with their rule files.
- **Constituents without matches** → list them explicitly and take the **"No matching rule file" branch** (below) for each one before continuing to Phase 3. A single component may have both matched and unmatched constituents — handle them independently.

### Phase 3 — Verify against matching rule files

Verify the component satisfies **every section** in each matching rule file, and the good/bad patterns in the sibling examples file:

- **Overview** (correct usage/purpose, when to use/not use, contained components)
- **Accessibility** (core rules, ARIA roles/states/properties, accessible name, keyboard interaction, screen reader expectations, color/contrast)
- **Code Examples** — compare the implementation against the good and bad patterns in `documentation/components/Examples/{Component}.md`. Use those examples to evaluate the current implementation and to shape any proposed fix.
- **Checklist** (markup, states, keyboard, visual)

**Locate associated styles** before verifying styling-related rules. Check for:
- A co-located stylesheet (e.g., `Button.module.css`, `Button.scss`)
- Class names used in the component and trace them to any imported or global stylesheet (e.g., `index.css`, a utility framework)
- Inline styles or CSS-in-JS definitions within the component file

For **Color and Contrast**, **States**, and any other styling-dependent checks, base findings on the actual CSS — **do not** report "cannot verify" when the styles are available in the codebase.

**Flag only genuine violations.** When a rule includes a qualifying condition (e.g., "unless the surrounding context makes the purpose unambiguous"), evaluate the component's name, usage context, and parent components before flagging. Do not flag something that the rule explicitly permits.

**Report each violation using this format:**
- **Rule** (quote the requirement)
- **Location** (file + line/snippet)
- **Fix** (concrete code change)

Phase 3 is **report-only**. Do not auto-apply any fix. See **Scope discipline** at the top of this skill for the approval protocol.

### Phase 2 branch — No matching rule file found

Run this branch for **each** constituent that has no matching rule file. When multiple constituents are unmatched, list them all together so the user can respond once.

1. Present the unmatched constituents to the user:
   > "The following classified elements have no matching rule files: [list each with its category]. Could you provide more information about their purpose, or do any of these map to existing patterns?"
2. If the user's response maps any of them to existing rule files, use those files and return to Phase 3 for those constituents.
3. For any that remain unmatched (confirmed by the user as genuinely new), offer to create rule files:
   > "No existing rule files cover these patterns: [list]. Would you like me to create rule files for any of them?"
   - **If the user accepts (for some or all):** Read `${docs.authoringGuide}` (default `documentation/DOCUMENTATION_GUIDE.md`) and follow its heading hierarchy, section authoring rules, external sources, content rules, and workflow to create a new rule file in `${docs.componentRules}` for each accepted item. Ask the user to fill in missing information for every section except Accessibility. Extract information from applicable W3C ARIA APG patterns and magentaA11y documentation to complete the Accessibility section. If information is missing, warn the user. Do not generate the component code until the rule files are created and confirmed by the user. Once confirmed, return to Phase 3 to verify against the new rule files.
   - **If the user declines:** Proceed without rule files for those constituents. Apply general WCAG 2.2 and ARIA APG best practices during Phase 3 verification, and note in the report which constituents lack component-specific rule files.

---

## AI-generated code requirements

These requirements apply **only** when the user explicitly requests new component code, a full regeneration, or a scaffold — not when performing incremental edits to existing code. For incremental edits, Scope discipline takes precedence: do not add comments, summaries, or checklists that were not requested.

When generating new component code, include:

1. Valid, accessible code that conforms to all applicable rule files.
2. Inline comments explaining:
   - The purpose of accessibility features (ARIA attributes, keyboard handlers, focus management, etc.)
   - Any trade-offs or browser quirks addressed.
3. A plain-language summary of the accessibility rationale.
4. A checklist confirming rule conformance, for traceability.

---

## Response style

- Be concise and direct. Avoid unnecessary fluff or over-explanation.
- Keep responses focused on the task. Do not pad with restatements or preambles.

---

## References

- Rule authoring guide: `${docs.authoringGuide}` (default `documentation/DOCUMENTATION_GUIDE.md`)
