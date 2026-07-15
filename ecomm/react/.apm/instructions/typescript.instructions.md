---
description: "TypeScript-specific coding rules. Applied automatically to .ts and .tsx files."
applyTo: "**/*.{ts,tsx}"
---

# TypeScript Standards

## Types

- Never use `any`. Prefer `unknown` when the type is genuinely unknown, then narrow with guards.
- Avoid type assertions (`as SomeType`) except when consuming untyped third-party APIs. Add a comment explaining why.
- Use `interface` for object shapes that may be extended; use `type` for unions, intersections, and mapped types.
- Do not use `Function` as a type — use a specific signature: `(input: string) => boolean`.

## Strictness

- Enable `strict: true` in `tsconfig.json`. Never disable individual strict checks per-file.
- Do not use non-null assertions (`!`) to silence the compiler — handle the null case explicitly.

## Generics

- Name generic parameters descriptively when the constraint is non-obvious: `TEntity`, `TResponse`.
- Single-letter generics (`T`, `K`, `V`) are acceptable for simple, self-evident cases.

## Async

- Always `await` promises or explicitly handle them — do not fire-and-forget unless intentional.
- Mark intentional fire-and-forget with `void`: `void sendAnalytics(event)`.
- Prefer `async/await` over raw `.then()` chains for readability.

## Imports

- Use named imports over default imports where the module supports it.
- Group imports: external packages first, then internal modules, separated by a blank line.
- Use path aliases (`@/utils/...`) rather than deep relative paths (`../../../utils/...`).
