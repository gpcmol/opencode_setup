Testing is mandatory for all backend changes.

TDD is the default approach.

Guidelines:
- Write tests before or alongside implementation whenever feasible
- For simple or low-risk changes, tests may follow implementation
- All new functionality must be covered by tests

Strategy:
- Unit tests for business logic
- Integration tests for API endpoints

Integration testing:
- Use WireMock for external dependencies
- Use real Spring context for integration tests
- Ensure tests cover happy path + key edge cases

Rules:
- No feature is complete without tests
- Fix or update tests when changing behavior
- Avoid overly complex test setups

Frontend (if applicable):
- Use Playwright for end-to-end flows when needed
