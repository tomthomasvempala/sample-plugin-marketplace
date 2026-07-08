---
name: generate-docs
description: "Generate documentation comments for functions, classes, and modules. Use when: adding docs to undocumented code, updating stale comments, generating API docs. Triggers: 'add docs', 'document this', 'generate comments'."
argument-hint: "File or function to document (e.g. src/utils/parser)"
---

# Generate Docs

Generates accurate documentation comments for functions, classes, interfaces, and modules.

## When to Use

- A function or class has no documentation
- Existing documentation is stale or inaccurate
- Preparing code for API reference generation

## Procedure

1. **Read the target file** to understand its structure and exported symbols.
2. **Identify undocumented symbols** — functions, classes, interfaces, type aliases, and constants that are exported or complex enough to warrant explanation.
3. **For each symbol**, write a comment that includes:
   - A one-line summary describing *what* it does, not *how*.
   - Parameter entries for every parameter with type and purpose.
   - Return value description.
   - Throws/exceptions if the function can throw, with the condition.
   - An example for non-obvious usage patterns.
4. **Do not** restate type information already visible in the signature — add context, not noise.
5. **Edit the file in place** — insert comments directly above each symbol.

## Style Rules

- Keep the summary line under 80 characters.
- Use present tense: "Parses the input string" not "This function parses".
