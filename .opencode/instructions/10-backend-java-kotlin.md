Backend stack: Java / Kotlin (Spring Boot, Maven preferred)

Architecture rules:
- Use layered architecture (controller → service → repository)
- Controllers must be thin
- Business logic must live in services
- No business logic in DTOs or controllers
- Keep domain logic isolated from framework code where possible

Kotlin/Java best practices:
- Prefer immutability (val / final where possible)
- Avoid nulls; use Optional (Java) or nullable types carefully (Kotlin)
- Use data classes for DTOs (Kotlin)
- Avoid over-engineering abstractions
- For Java, make everything final

Maven:
- Standard Maven project structure
- Keep dependencies minimal
- Avoid version conflicts and duplicate libraries

API design:
- RESTful design
- Consistent naming conventions
- Backward compatibility is mandatory unless explicitly stated otherwise

Complexity rules:
- Functions should be small and focused (prefer < 30 lines)
- Avoid deep nesting (> 2-3 levels)
- Prefer early returns over nested conditionals
- Each class should have a single responsibility
- Avoid large “god classes”

Refactoring rules:
- If complexity increases significantly, refactor before continuing

In brownfield code:
- Do not refactor unrelated complex code
- Only reduce complexity in the area you are modifying
