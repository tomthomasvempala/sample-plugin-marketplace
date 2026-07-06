---
description: "General coding standards applied to all source files. Covers naming, structure, immutability, and error handling conventions."
applyTo: "src/**"
---

# Coding Standards

## Naming

- Use `camelCase` for variables and functions, `PascalCase` for types and classes.
- Boolean variables and functions start with `is`, `has`, or `can`: `isLoading`, `hasPermission`.
- Event handlers start with `on` or `handle`: `onSubmit`, `handleClick`.
- Avoid single-letter variables except for short-lived loop indices (`i`, `j`).

## Structure

- Keep functions under 30 lines. If a function is longer, it likely does too much.
- Prefer early returns over nested `if` blocks to reduce indentation depth.
- Export only what consumers need — keep implementation details unexported.

## Immutability

- Default to `const`. Use `let` only when reassignment is necessary.
- Prefer non-mutating array methods (`map`, `filter`, `reduce`) over `push`/`splice`.
- Do not mutate function parameters.

## Error Handling

- Handle errors at the boundary where they can be meaningfully addressed.
- Do not swallow errors silently (`catch (e) {}`). Log or rethrow.
- Use typed errors where possible rather than plain `Error` strings.

## Comments

- Write comments to explain *why*, not *what*.
- Delete commented-out code before committing — use version control instead.
- TODOs must include a reason: `// TODO: remove after migrating to v2 API`.
