# Design Principles

Read when output feels visually inconsistent, spacing is off, or a component doesn't feel like it belongs with the rest of the product.

---

## Visual Hierarchy

Structure the component so the most important element draws the eye first. Achieve this through size, weight, contrast, and position — not decoration.

The user should be able to identify the primary action or content within two seconds of seeing the component. If they can't, the hierarchy is wrong.

---

## Consistency

Every spacing, color, border radius, typography, and shadow value must come from the project's design tokens or utility classes. Never hardcode raw values like `padding: 13px` or `color: #3a3a3a`.

Hardcoded values create two problems:

1. The component looks slightly off compared to everything around it
2. Design changes require hunting down every hardcoded value in every file

---

## Whitespace

Space is a design element, not an absence of content. Use generous, consistent spacing based on the project's spacing scale. Cramped
components feel unfinished regardless of code quality.

When in doubt, add more space. Tight spacing is harder to fix than generous spacing because it makes every element compete for attention.

---

## Feedback

Every interactive element must communicate state back to the user:

| Element      | Required states                           |
| ------------ | ----------------------------------------- |
| Button       | default, hover, active, disabled, loading |
| Input        | default, focus, error, disabled           |
| Link         | default, hover, visited                   |
| Async action | loading indicator while in-flight         |

Never leave the user uncertain about whether their action was registered. A button that doesn't change on click feels broken even if it works.

---

## Accessibility

Accessibility is built in from the start, not retrofitted later.

**Keyboard navigation**
Every interactive element must be reachable and operable by keyboard. Tab order must follow the visual reading order.

**ARIA**

- Use semantic HTML first (`<button>`, `<nav>`, `<main>`) — ARIA is for
  cases where semantic HTML isn't enough
- Every interactive element needs a label a screen reader can announce
- `aria-live` on content that updates without a page reload

**Color contrast**
Meet WCAG AA minimums:

- Normal text: 4.5:1 contrast ratio
- Large text (18px+ or 14px bold): 3:1
- UI components and focus indicators: 3:1

Never rely on color alone to convey meaning — use a secondary indicator (icon, text, pattern) alongside color.

---

## Responsiveness

Design for the smallest screen first, then expand outward.

| Breakpoint | Width  | What to check                                                   |
| ---------- | ------ | --------------------------------------------------------------- |
| Mobile S   | 375px  | No horizontal scroll, readable text, tappable targets ≥ 44×44px |
| Tablet     | 768px  | Layout adapts, no awkward wrapping                              |
| Desktop    | 1280px | No stretched or collapsed elements                              |

Never assume a fixed container width. A component that only works at one width is not finished.
