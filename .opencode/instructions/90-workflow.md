```md
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
- Never leave the worktree in a broken state
- Required validation must pass for the task type:
  - Tests for application code
  - Validation/linting/dry-runs for infra
  - Execution validation/linting for scripts
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

## 1.5 Task classification (MANDATORY)

Before implementation, classify the task:

### Application code
Examples:
- Backend/frontend business logic
- APIs
- Services
- Libraries
- UI components
- Domain logic

Validation expectations:
- Unit tests expected where meaningful
- Integration tests when behavior crosses boundaries
- Prefer TDD when practical

---

### Infrastructure/configuration
Examples:
- Terraform
- Kubernetes
- Helm
- Docker
- CI/CD
- Cloud configuration
- IaC modules

Validation expectations:
- Prefer validation, linting, plans, or dry-runs
- Verify manifests/configuration
- Automated tests optional unless repository standards require them

---

### Operational/scripts
Examples:
- Bash scripts
- Automation tooling
- Developer utilities
- Maintenance scripts
- Local tooling wrappers

Validation expectations:
- Prefer execution verification
- Use shellcheck or equivalent linting
- Use smoke tests where appropriate
- Formal unit tests optional unless complexity justifies them

---

Do NOT create artificial tests solely to satisfy workflow requirements.

---

## 2. Plan before coding (MANDATORY)

- Identify impacted areas
- Break down into small, incremental steps
- Define:
  - Expected behavior
  - Validation strategy
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

Prefer TDD for application code when practical:
- Write a failing test first (if feasible)
- Implement minimal code to pass

For infrastructure or scripting tasks:
- Prefer validation-first workflows
- Use linting, dry-runs, syntax validation, or smoke checks where appropriate

Avoid:
- Large refactors outside scope
- Speculative abstractions
- Unnecessary dependencies

---

## 4. Feedback loop (MANDATORY after EACH step)

### 4.1 Run validation appropriate to the task

### Application code
- Run unit tests
- Run integration tests (if applicable)

### Infrastructure/configuration
- Run relevant validation tools
- Run linting
- Run dry-runs/plans when supported
- Validate manifests/configuration

### Scripts/automation
- Run shellcheck or equivalent linting
- Verify execution behavior
- Run smoke tests if appropriate

If validation fails:
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
- Re-run applicable validation

---

### 4.3 Sanity check

- Does this step still match the plan?
- Is scope creeping?
- Is this the simplest possible solution?

---

Repeat until:
- Validation passes
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

Ensure the correct validation standard was applied:
- Tests for application logic
- Validation/plans for infrastructure
- Execution/linting for scripts

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

# Definition of Done (STRICT)

A task is NOT complete unless:

- Appropriate validation passes for the task type
- Tests exist for new or changed application behavior when applicable
- Infra/config/script changes include appropriate validation or verification
- No breaking changes are introduced
- OpenAPI is aligned with implementation (if applicable)
- Code/config changes are:
  - Small
  - Safe
  - Localized
- Complexity is acceptable in modified areas
- Existing patterns are followed
- Worktree is clean and stable
- Branch is ready for merge without additional fixes
```
