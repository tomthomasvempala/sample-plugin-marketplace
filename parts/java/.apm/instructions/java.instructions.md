---
description: "Java coding standards and best practices. Applied to all .java source files."
applyTo: "**/*.java"
---

# Java Standards

## Naming

- Classes and interfaces in `PascalCase`: `UserService`, `OrderRepository`.
- Methods and variables in `camelCase`: `findById`, `totalPrice`.
- Constants in `UPPER_SNAKE_CASE`: `MAX_RETRY_COUNT`.
- Package names in lowercase with dots: `com.example.order.service`.

## Structure

- Keep methods under 30 lines. Extract private helpers for complex logic.
- Prefer early returns and guard clauses over deeply nested `if`/`else` chains.
- One top-level class per file. Nested classes are acceptable when they are tightly coupled to the outer class.
- Follow standard Java source layout: `src/main/java` for production code, `src/test/java` for tests.

## Immutability

- Declare fields `final` wherever possible.
- Return defensive copies of mutable collections from public methods.
- Prefer immutable value objects (`record` in Java 16+) for data carriers.

## Null Safety

- Do not return `null` from public methods — use `Optional<T>` or throw a domain exception.
- Annotate parameters and return types with `@NonNull` / `@Nullable` (e.g. from `org.springframework.lang`).
- Do not pass `null` as a method argument — use method overloads or builder patterns instead.

## Error Handling

- Use checked exceptions only for recoverable, expected failure conditions.
- Use unchecked exceptions (`RuntimeException` subclasses) for programming errors and unrecoverable failures.
- Never catch `Exception` or `Throwable` without rethrowing or logging with full context.
- Provide meaningful messages in exceptions: `new IllegalArgumentException("userId must be positive, got: " + userId)`.

## Logging

- Use SLF4J (`LoggerFactory.getLogger(MyClass.class)`) — never `System.out.println`.
- Log at the appropriate level: `DEBUG` for diagnostic detail, `INFO` for key lifecycle events, `WARN` for recoverable issues, `ERROR` for failures.
- Use parameterized log messages: `log.info("Order {} created", orderId)` — not string concatenation.
- Never log sensitive data (passwords, tokens, PII).
