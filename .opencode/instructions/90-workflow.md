### 0. Setup (version control)

- Ensure the project is a git repository
- Create a new branch for the feature
- Use a separate git worktree for isolation
- Perform all changes inside this worktree
- Do not modify the main branch directly
- Commit changes in small, logical steps
- Never leave the worktree in a broken state (tests must pass)

---

### 1. Understand context
- Check existing code and patterns
- Read relevant OpenAPI specs
- Identify constraints from instructions (backend, testing, brownfield)

---

### 2. Plan before coding
- Identify impacted areas
- Break down into small, incremental changes
- Identify risks and edge cases
- Consider OpenAPI impact
- Keep changes minimal and localized

Do NOT start implementation yet if planning is incomplete.

---

### 3. Implement (incremental)
- Implement in small, safe steps
- Follow backend/frontend rules
- Respect brownfield constraints
- Prefer TDD:
    - write tests before or alongside implementation

---

### 4. Feedback loop (MANDATORY)

After each implementation step:

#### 4.1 Run tests
- Run unit and integration tests
- If tests fail:
    - analyze failures
    - fix issues before continuing

#### 4.2 Evaluate complexity
- Check for:
    - large or unclear methods
    - deep nesting
    - duplicated logic
    - poor separation of concerns

- If complexity is too high:
    - refactor locally (only in modified area)
    - keep behavior unchanged
    - re-run tests

Repeat until:
- tests pass
- complexity is acceptable

---

### 5. Validate
- Ensure OpenAPI consistency
- Ensure backward compatibility
- Verify no regressions
- Perform critical self-review

---

## Definition of done

A task is NOT complete unless:

- All tests pass
- Tests exist for new or changed behavior
- No breaking changes are introduced
- OpenAPI is aligned with implementation (if applicable)
- Code changes are small, safe, and localized
- Complexity is acceptable for the modified area
