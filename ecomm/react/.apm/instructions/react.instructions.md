---
description: "React component and hook patterns. Applied to .tsx files and React project source files."
applyTo: "**/*.{tsx,jsx}"
---

# React Standards

## Component Design

- Prefer function components over class components.
- One component per file. File name matches the exported component name in PascalCase.
- Keep components focused — if a component needs more than ~100 lines, split it.
- Lift state only as high as needed; keep state as local as possible.

## Props

- Define props with a named `interface` (e.g. `interface ButtonProps`), not an inline type.
- Destructure props at the top of the function signature.
- Avoid boolean props with ambiguous meaning — prefer explicit values: `variant="primary"` over `isPrimary`.
- Mark optional props with `?` and provide sensible defaults via default parameter values.

## Hooks

- Follow the Rules of Hooks: only call at the top level, only inside function components or custom hooks.
- Name custom hooks with the `use` prefix: `useAuth`, `useFetchUser`.
- Extract reusable stateful logic into custom hooks rather than duplicating `useEffect`/`useState` calls.
- Declare all `useEffect` dependencies honestly — do not suppress the exhaustive-deps lint rule without a comment.

## State Management

- Use `useState` for local UI state. Use `useReducer` when state transitions are complex.
- Avoid derived state in `useState` — compute it directly during render.
- Do not store server data in local state when a data-fetching library (React Query, SWR) is available.

## Performance

- Do not `useMemo`/`useCallback` prematurely — profile first.
- Apply `React.memo` only on components that demonstrably re-render unnecessarily.
- Keep render functions free of side effects.

## Event Handlers

- Define event handlers outside JSX: `const handleClick = () => {}`, not `onClick={() => {}}`.
- Prefix handler names with `handle`: `handleSubmit`, `handleClose`.
