---
name: component-architecture
description: "Use this when creating, editing, refactoring, or reviewing any components, UI systems, or frontend architecture - even if it does not explicitly mention 'components' but describe UI elements, layout, or reusable frontend logic. Do NOT use for full page layouts (use page-layout skill), data fetching logic, state management architecture, or design token definition."
---

# Component Architecture

## Workflow

### Phase 1 — Audit before writing

Check whether a similar component already exists in the codebase. If it does, extend or adapt it — do not create a duplicate.

Define the component's single responsibility in one sentence before writing any code. If you cannot describe it in one sentence, it has more than one responsibility and must be split.

### Phase 2 — Map all states

Before writing any code, enumerate every state the component can be in:

- **Default** — normal render with data present
- **Loading** — data is being fetched
- **Error** — fetch failed or invalid input
- **Empty** — data fetch succeeded but returned nothing
- **Edge cases** — zero items, very long strings, missing optional props

All states must be handled. Never leave a state that renders a blank white area or an unhandled exception.

### Phase 3 — Structure

Scaffold in this order:

1. TypeScript interface for props
2. Component shell with named export
3. Sub-components if the render exceeds ~50 lines or has distinct sections
4. Implement each state in order: default → loading → error → empty

### Phase 4 — Review against hard rules

Before writing the summary, run through the Hard Rules section below. Any violation is a blocker — fix before marking complete.

---

## Hard Rules

**Types**

- Every component must have a named TypeScript interface for its props.
- Never use `any`, inline type literals, or untyped props.

**Styling**

- Every spacing, color, border radius, and shadow value must come from the project's design or utility classes. Never use hardcoded
  values like raw pixel numbers or hex colors.
- Inline styles are only acceptable for values that are genuinely dynamic at runtime and cannot be expressed in a utility class or CSS variable.

**Data and side effects**

- Data must arrive via props or a custom hook. Never fetch, call axios, or make any API call inside a component body or useEffect.

**State**

- Never store derived state. If a value can be computed from props or existing state, compute it directly — do not put it in useState.
- Never duplicate state across sibling components. Lift to the nearest common parent or a shared store.

**Size and responsibility**

- Maximum 200 lines per component file. Exceeding this is a signal to extract sub-components, not a reason to continue adding code.
- No prop drilling beyond two levels. If a prop passes through three or more components to reach its destination, use context or lift state.
- No more than one useEffect per component. Two effects means two responsibilities — split the component.

**Naming**

- Component names must describe what they render, not what they do. Prefixes like Render, Handle, or Do are always wrong.

---

## Reference Files

Read these only when the relevant phase or check requires it — do not load all of them upfront:

`references/design-principles.md` - Output is visually inconsistent or spacing feels wrong.

---

## QA

Run before marking the component complete.

**Automated checks — both must pass with zero errors:**

npx tsc --noEmit
npx eslint src/components/ComponentName.tsx

**State verification — render each state explicitly, do not just read the code:**

- Default state renders with realistic data
- Loading state shows a skeleton or spinner — not a blank area
- Error state shows a user-visible message — check the browser console is clean (no unhandled exceptions)
- Empty state renders a meaningful empty message — not a blank area

**Hard rules spot checks:**

```bash
# Must return nothing (no inline styles)
grep -n 'style={{' src/components/ComponentName.tsx

# Must return nothing (no any types)
grep -n ': any' src/components/ComponentName.tsx

# Must return nothing (no direct fetch inside component)
grep -n 'fetch(\|axios\.' src/components/ComponentName.tsx
```

Any match from the above greps is a blocker — fix before proceeding.

**Keyboard and accessibility:**

- Tab through the component with no mouse — every interactive element is reachable and operable by keyboard alone
- Every interactive element has a visible focus ring
- All images have meaningful `alt` text; decorative images have `alt=""`

**Responsiveness — test at these three widths:**

- 375px — no horizontal scroll, no truncated text without tooltip
- 768px — layout adapts correctly
- 1280px — no awkward stretching or collapsed elements

**Before submitting:**

- One-to-two sentence comment at the top of the file: what it does, when to use it
- Co-located test file exists: `ComponentName.test.tsx`
- t least one test covers the primary user-facing behavior
- Print a summary: files created or modified, known limitations, edge cases identified during implementation

**Stop condition:** Do not iterate further once all checklist items pass and both automated checks return zero errors. Do not refactor working code for style preferences after QA passes.
