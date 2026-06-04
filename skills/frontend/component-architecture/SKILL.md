---
name: component-architecture
description: "Use this when creating, editing, refactoring, or reviewing any UI component, layout, page, or frontend interface element in the project"
---

# Component Architecture

## Before Writing Any Code

Read the existing codebase before creating anything new. Check whether a similar component already exists. If it does, extend or adapt it rather than creating a duplicate. Identify the single responsibility of the component — if it cannot be described in one sentence, it needs to be split into smaller components.

Identify all possible states the component can be in before writing a line of code: default, loading, error, empty, and any edge case states specific to this component. All states must be handled — never leave a state unaccounted for.

## Design Principles

**Visual Hierarchy**
Structure the component so that the most important element draws the eye first. Achieve this through size, weight, contrast, and position — not decoration.

**Consistency**
Every value used in the component must come from the project's design tokens or utility classes — spacing, color, border radius, typography, and shadow. Never use raw hardcoded values like `padding: 13px` or `color: #3a3a3a`. Hardcoded values break consistency across the product and make design changes painful.

**Whitespace**
Space is a design element, not absence of content. Use generous, consistent spacing based on the project's spacing scale. Cramped components feel unfinished regardless of code quality.

**Feedback**
Every interactive element must communicate state back to the user. Buttons must have hover, active, and disabled states. Forms must show validation feedback. Async actions must show a loading indicator. Never leave the user uncertain about whether their action was registered.

**Accessibility**
Accessibility is built in from the start, not added later. Every component must be fully keyboard navigable, have meaningful ARIA labels on interactive elements, meet WCAG AA color contrast minimums, and be usable with a screen reader.

**Responsiveness**
Design for the smallest screen first, then expand outward. Every component must function correctly at mobile (375px), tablet (768px), and desktop (1280px) breakpoints minimum. Never assume a fixed container width.

## Component Structure Rules

- **Single responsibility** — one component, one job. If a component handles layout, data, and error display simultaneously, split it.
- **Props over internal state** — components that receive data via props are easier to test, reuse, and reason about than components that manage their own data.
- **No prop drilling beyond two levels** — if a prop passes through three or more components to reach its destination, use context or lift state instead.
- **Component names describe what they render** — `UserProfileCard` is correct. `RenderUser` or `HandleUser` are not.
- **Maximum 200 lines per component file** — if a file exceeds this, it is a signal to extract sub-components.
- **No inline styles** — use utility classes or CSS variables. Reserve inline styles only for values that are genuinely dynamic and cannot be expressed in CSS.
- **No magic numbers** — every spacing, sizing, or timing value must reference a named token or variable, not a raw number with no context.

## State Management

Before adding state to a component, determine where it belongs:

- **UI state** (open/closed, hovered, focused) belongs in the component locally
- **Shared state** (used by more than one component) must be lifted up or placed in a store — never duplicated across components
- **Derived state** (calculated from props or existing state) must be computed directly, never stored as redundant state

## After Writing the Component

Before marking the component as complete:

- Verify every state renders correctly — default, loading, error, and empty
- Tab through the component using only the keyboard and confirm all interactions work
- Check the component at mobile, tablet, and desktop widths
- Add a one to two sentence comment at the top of the file explaining what the component does and when to use it
- Print a summary listing every file created or modified and any limitations or edge cases identified during implementation
