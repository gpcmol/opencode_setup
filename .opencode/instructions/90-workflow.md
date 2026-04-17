# Development Workflow

## -1. Execution defaults (NO PROMPTS)

- Always use sub-agent driven execution
- Always use multi-agent mode (Superpowers) when available
- Never use inline execution
- Always create and use a git worktree
- Never ask the user to choose execution mode, agent mode, or worktree strategy
- Automatically select multi-agent orchestration without prompting or confirmation
- Assume these preferences are mandatory defaults
- Automatically proceed with these choices

---

## 0. Setup (version control)

- Ensure the project is a git repository
- If not:
  - Initialize git
- Create a new branch for the feature
- Create a dedicated git worktree for this branch
- Perform all changes inside this worktree only
- Never modify the main branch directly
- Keep the main branch clean at all times
- Commit changes in small, logical steps
- Use clear commit messages (what + why)
- Never leave the worktree in a broken state (tests must pass)
- Sync regularly with main branch if needed (rebase or merge carefully)

---

## 1. Understand context

- Check existing code and patterns
- Identify architectural conventions
- Read relevant OpenAPI specs (if applicable)
- Identify:
  - Backend/frontend constraints
  - Testing strategy
  - Brownfield limitations
- Locate related modules and dependencies
- Avoid introducing new patterns if existing ones suffice

---

## 2. Plan before coding (MANDATORY)

- Identify impacted areas
- Break down into small, incremental steps
- Define:
  - Expected behavior
  - Test strategy
- Identify:
  - Risks
  - Edge cases
  - Failure scenarios
- Consider:
  - OpenAPI impact
  - Backward compatibility
- Keep changes minimal and localized

Do NOT start implementation if:
- Scope is unclear
- Steps are too large
- Risks are not understood

---

## 3. Implement (incremental & controlled)

- Use sub-agents to:
  - Isolate responsibilities
  - Keep changes scoped
- Implement in small, safe steps
- Follow existing patterns strictly
- Respect brownfield constraints

Prefer TDD:
- Write a failing test first (if feasible)
- Implement minimal code to pass

Avoid:
- Large refactors outside scope
- Speculative abstractions
- Unnecessary dependencies

---

## 4. Feedback loop (MANDATORY after EACH step)

### 4.1 Run tests

- Run unit tests
- Run integration tests (if applicable)

If tests fail:
- Stop
- Analyze root cause
- Fix before continuing

---

### 4.2 Evaluate complexity

Check for:
- Large or unclear methods
- Deep nesting
- Duplicated logic
- Tight coupling
- Poor naming

If complexity is too high:
- Refactor only modified areas
- Keep behavior unchanged
- Re-run tests

---

### 4.3 Sanity check

- Does this step still match the plan?
- Is scope creeping?
- Is this the simplest possible solution?

---

Repeat until:
- Tests pass
- Complexity is acceptable
- Step is clean and complete

---

## 5. Validate (pre-completion)

- Ensure OpenAPI consistency (if applicable)
- Ensure backward compatibility
- Verify no regressions
- Check integration points

Perform critical self-review:
- Would another developer understand this quickly?
- Is this consistent with the codebase?

---

## 6. Finalize

- Clean up:
  - Unused code
  - Debug artifacts
- Ensure commits are:
  - Logically grouped
  - Clearly named
- Optionally squash commits if required by repo strategy

Prepare for merge:
- Ensure branch is up-to-date
- Resolve conflicts cleanly

---

## Definition of Done (STRICT)

A task is NOT complete unless:

- All tests pass
- Tests exist for new or changed behavior
- No breaking changes are introduced
- OpenAPI is aligned with implementation (if applicable)
- Code changes are:
  - Small
  - Safe
  - Localized
- Complexity is acceptable in modified areas
- Code follows existing patterns
- Worktree is clean and stable
- Branch is ready for merge without additional fixes
