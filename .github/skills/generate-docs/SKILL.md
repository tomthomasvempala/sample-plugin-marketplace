---
name: generate-docs
description: "Generate JSDoc or TSDoc documentation comments for functions, classes, and modules. Use when: adding docs to undocumented code, updating stale comments, generating API docs. Triggers: 'add docs', 'document this', 'generate comments', 'write jsdoc', 'add tsdoc'."
argument-hint: "File or function to document (e.g. src/utils/parser.ts)"
---

# Generate Docs

Generates accurate JSDoc/TSDoc comments for functions, classes, interfaces, and modules.

## When to Use

- A function or class has no documentation
- Existing documentation is stale or inaccurate
- Preparing code for API reference generation (TypeDoc, JSDoc)

## Procedure

1. **Read the target file** to understand its structure and exported symbols.
2. **Identify undocumented symbols** — functions, classes, interfaces, type aliases, and constants that are exported or complex enough to warrant explanation.
3. **For each symbol**, write a comment that includes:
   - A one-line summary describing *what* it does, not *how*.
   - `@param` entries for every parameter with type and purpose.
   - `@returns` describing the return value and type.
   - `@throws` if the function can throw, with the error type and condition.
   - `@example` for any non-obvious usage patterns.
4. **Do not** restate type information already visible in the signature — add context, not noise.
5. **Edit the file in place** — insert comments directly above each symbol.

## Style Rules

- Use `/** ... */` block comments for JSDoc/TSDoc, not `//`.
- Keep the summary line under 80 characters.
- Use present tense: "Parses the input string" not "This function parses".
- For TypeScript, use `@typeParam T` for generic type parameters.

## Example

**Before:**
```ts
export function paginate<T>(items: T[], page: number, size: number): T[] {
  return items.slice((page - 1) * size, page * size);
}
```

**After:**
```ts
/**
 * Returns a single page of items from an array.
 *
 * @param items - The full array to paginate.
 * @param page - 1-based page number.
 * @param size - Number of items per page.
 * @returns The items for the requested page, or an empty array if out of range.
 *
 * @example
 * paginate([1, 2, 3, 4, 5], 2, 2); // [3, 4]
 */
export function paginate<T>(items: T[], page: number, size: number): T[] {
  return items.slice((page - 1) * size, page * size);
}
```
