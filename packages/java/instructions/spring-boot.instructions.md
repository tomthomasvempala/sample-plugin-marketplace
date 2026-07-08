---
description: "Spring Boot patterns and conventions. Applied to Spring Boot projects using @RestController, @Service, @Repository, or similar annotations."
applyTo: "src/main/java/**/*.java"
---

# Spring Boot Standards

## Layering

Follow strict three-layer architecture: Controller → Service → Repository.

- **Controllers** (`@RestController`): validate input, delegate to service, map to HTTP response. No business logic.
- **Services** (`@Service`): own all business logic and transactions. No direct repository calls from controllers.
- **Repositories** (`@Repository`): data access only. Use Spring Data JPA interfaces where possible.

## Dependency Injection

- Use constructor injection exclusively — never `@Autowired` on fields.
- Declare injected dependencies `final`.
- Use Lombok `@RequiredArgsConstructor` to avoid boilerplate constructor code.

## REST Controllers

- Annotate controllers with `@RequestMapping("/v1/resource")` at class level.
- Return `ResponseEntity<T>` to control status codes explicitly.
- Use `@Valid` on request body parameters to trigger Bean Validation.
- Map domain exceptions to HTTP responses via `@ControllerAdvice` — not in the controller itself.

## Transactions

- Annotate service methods with `@Transactional` at the method level, not class level.
- Default to `readOnly = true` for query-only methods: `@Transactional(readOnly = true)`.
- Never start a transaction inside a repository method — transactions belong in the service layer.

## Configuration

- Externalize all environment-specific values to `application.yml` / `application-{profile}.yml`.
- Bind configuration to `@ConfigurationProperties` classes — do not use `@Value` for more than one or two properties.
- Never hardcode URLs, credentials, or environment names in source code.

## Exception Handling

- Define a domain exception hierarchy rooted at a custom `ApplicationException`.
- Use a `@ControllerAdvice` class to translate domain exceptions into consistent `ProblemDetail` (RFC 9457) responses.
- Return `404 Not Found` for missing resources, `409 Conflict` for state violations, `422` for semantic validation errors.
