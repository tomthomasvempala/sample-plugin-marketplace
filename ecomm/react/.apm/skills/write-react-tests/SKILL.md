---
name: write-react-tests
description: "Write React component and hook tests using React Testing Library and Vitest/Jest. Use when: testing components, custom hooks, user interactions, or async rendering. Triggers: 'write component tests', 'test this component', 'add RTL tests', 'test this hook'."
argument-hint: "Component or hook to test (e.g. src/components/Button.tsx)"
---

# Write React Tests

Writes focused React component and hook tests using React Testing Library (RTL), following project test patterns.

## When to Use

- A component or hook has no test coverage
- You need to verify user interactions or async rendering behavior
- Testing accessibility and role-based queries

## Procedure

1. **Read the target file** to understand the component's props, state, and rendered output.
2. **Scan for an existing test file** (e.g. `*.test.tsx`, `*.spec.tsx`) to identify:
   - The test framework (Vitest or Jest)
   - Whether `@testing-library/user-event` or `fireEvent` is used for interactions
   - Custom render wrappers (e.g. providers for context, router, theme)
3. **Identify test scenarios**:
   - Renders correctly with default props
   - Renders correctly with each significant prop variation
   - User interactions trigger the correct callbacks or state changes
   - Async states: loading, error, success
   - Conditional rendering branches
4. **Write the tests** using RTL best practices (see rules below).
5. **Create the test file** co-located with the source if it doesn't exist: `Button.test.tsx` next to `Button.tsx`.

## RTL Rules

- Query by role or accessible name first: `getByRole('button', { name: /submit/i })`.
- Fall back to `getByLabelText`, `getByPlaceholderText`, then `getByTestId` — in that order.
- Never query by CSS class or internal implementation details.
- Use `userEvent` over `fireEvent` for simulating real user interactions.
- Wrap state-updating interactions in `act()` if not already handled by `userEvent`.
- Use `waitFor` or `findBy*` queries for async assertions.

## Hook Testing

- Use `renderHook` from `@testing-library/react` for isolated hook tests.
- Wrap in necessary providers via the `wrapper` option.

## Example

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from './Button';

describe('Button', () => {
  it('should call onClick when clicked', async () => {
    const handleClick = vi.fn();
    render(<Button onClick={handleClick}>Submit</Button>);

    await userEvent.click(screen.getByRole('button', { name: /submit/i }));

    expect(handleClick).toHaveBeenCalledOnce();
  });

  it('should be disabled when the disabled prop is set', () => {
    render(<Button disabled>Submit</Button>);
    expect(screen.getByRole('button', { name: /submit/i })).toBeDisabled();
  });
});
```
