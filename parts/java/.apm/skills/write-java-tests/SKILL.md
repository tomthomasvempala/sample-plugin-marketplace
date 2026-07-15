---
name: write-java-tests
description: "Write Java unit and integration tests using JUnit 5 and Mockito. Use when: adding test coverage to Java classes, testing Spring services, mocking dependencies, writing slice tests. Triggers: 'write Java tests', 'add JUnit tests', 'test this service', 'mock this dependency', 'write integration test'."
argument-hint: "Class to test (e.g. src/main/java/com/example/OrderService.java)"
---

# Write Java Tests

Writes focused JUnit 5 unit tests and Spring Boot slice tests, following the project's existing test patterns.

## When to Use

- A class or method has no test coverage
- You need to verify business logic in a service
- You need a Spring slice test (`@WebMvcTest`, `@DataJpaTest`) for a controller or repository
- Adding edge-case or error-path coverage

## Procedure

1. **Read the target class** to understand its dependencies, public methods, and expected behavior.
2. **Scan for existing tests** under `src/test/java` to identify:
   - JUnit 5 patterns already in use (`@ExtendWith`, `@SpringBootTest`, etc.)
   - Mockito usage (`@Mock`, `@InjectMocks`, `@MockBean`)
   - Shared fixtures and test utilities
3. **Choose the right test type**:
   - Pure unit test for classes with no Spring context dependency.
   - `@WebMvcTest` for controllers (loads only the web layer).
   - `@DataJpaTest` for repositories (loads only JPA layer with an in-memory database).
   - `@SpringBootTest` only when full context is genuinely needed — avoid by default.
4. **Identify test scenarios**:
   - Happy path — valid inputs produce the expected result.
   - Edge cases — null inputs, empty lists, zero values, boundary conditions.
   - Error paths — service throws domain exceptions, repository returns empty, external call fails.
5. **Write the tests** and place them at the mirrored path under `src/test/java`.

## Rules

- Name tests with the Given-When-Then or Should pattern: `should_ReturnOrder_When_IdIsValid`.
- Arrange-Act-Assert (AAA) inside every test method.
- Never use `@SpringBootTest` for unit-testable logic — mock dependencies with Mockito.
- Use `assertThat` from AssertJ for readable assertions instead of `assertEquals`.
- Do not use `@Autowired` in pure unit tests — instantiate the class under test directly.
- Use `@MockBean` only in Spring slice tests, not in plain unit tests.

## Example — Service Unit Test

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @InjectMocks
    private OrderService orderService;

    @Test
    void should_ReturnOrder_When_IdIsValid() {
        // Arrange
        var expected = new Order(1L, "PENDING");
        when(orderRepository.findById(1L)).thenReturn(Optional.of(expected));

        // Act
        var result = orderService.getOrder(1L);

        // Assert
        assertThat(result).isEqualTo(expected);
    }

    @Test
    void should_ThrowOrderNotFoundException_When_IdDoesNotExist() {
        when(orderRepository.findById(99L)).thenReturn(Optional.empty());

        assertThatThrownBy(() -> orderService.getOrder(99L))
            .isInstanceOf(OrderNotFoundException.class)
            .hasMessageContaining("99");
    }
}
```

## Example — Controller Slice Test

```java
@WebMvcTest(OrderController.class)
class OrderControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private OrderService orderService;

    @Test
    void should_Return200_When_OrderExists() throws Exception {
        when(orderService.getOrder(1L)).thenReturn(new Order(1L, "PENDING"));

        mockMvc.perform(get("/v1/orders/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(1));
    }

    @Test
    void should_Return404_When_OrderDoesNotExist() throws Exception {
        when(orderService.getOrder(99L)).thenThrow(new OrderNotFoundException(99L));

        mockMvc.perform(get("/v1/orders/99"))
            .andExpect(status().isNotFound());
    }
}
```
