---
name: write-tests
description: "Write unit tests for selected functions, classes, or modules. Use when: adding test coverage, practicing TDD, testing edge cases. Triggers: 'write tests', 'add unit tests', 'test this function', 'generate test cases', 'increase coverage'."
argument-hint: "Function or file to test (e.g. src/utils/validator.ts)"
---

# Write Tests

Writes focused unit tests for the target code, following the project's existing test patterns.

## When to Use

- A function or module has no test coverage
- You want to add edge-case coverage to existing tests
- Practicing test-first development

## Procedure

1. **Read the target file** to understand what the code does and its expected behavior.
2. **Scan for an existing test file** (e.g. `*.test.ts`, `*.spec.ts`) to identify:
   - The test framework in use (Jest, Vitest, Mocha, etc.)
   - Naming conventions and file co-location patterns
   - How mocks and fixtures are set up
3. **Identify test scenarios** for each exported function or class:
   - Happy path — normal inputs produce correct outputs
   - Edge cases — empty inputs, zero, null, max boundaries
   - Error cases — invalid inputs, throws, rejected promises
4. **Write the tests** following the discovered patterns. If no test file exists, create one at the co-located path (e.g. `src/utils/validator.test.ts`).
5. **Do not test implementation details** — test observable behavior only.

## Test Quality Rules

- Each test has a single assertion focus — use `it('should ...')` naming.
- Arrange-Act-Assert (AAA) structure inside every test body.
- Mock external dependencies (network, file system, timers) — never rely on real I/O in unit tests.
- Avoid `any` type casts in TypeScript test files.

## Example

**Source:**
```ts
export function clamp(value: number, min: number, max: number): number {
  return Math.min(Math.max(value, min), max);
}
```

**Generated tests:**
```ts
import { clamp } from './math';

describe('clamp', () => {
  it('should return the value when within range', () => {
    expect(clamp(5, 0, 10)).toBe(5);
  });

  it('should return min when value is below range', () => {
    expect(clamp(-5, 0, 10)).toBe(0);
  });

  it('should return max when value is above range', () => {
    expect(clamp(15, 0, 10)).toBe(10);
  });

  it('should return min when value equals min', () => {
    expect(clamp(0, 0, 10)).toBe(0);
  });

  it('should return max when value equals max', () => {
    expect(clamp(10, 0, 10)).toBe(10);
  });
});
```
