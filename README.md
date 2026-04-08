# OpenCode setup with OpenSpec and Superpowers

This guide describes a structured setup for using OpenCode together with OpenSpec and Superpowers.

The goal is **higher quality, safer changes, and better maintainability** through:

* spec-first development (OpenSpec)
* controlled execution (Superpowers)
* strict engineering rules (OpenCode instructions)
* feedback loops (tests + complexity)

> ⚠️ This approach may be slightly slower, but aims to significantly improve code quality.

- The pretentious language is not my own but that of AI.
---

# 🧠 Concept

* **“What are we building?” → OpenSpec**
* **“How do we build it?” → Superpowers**

Reference:
https://docs.bswen.com/blog/2026-03-27-openspec-vs-superpowers/

---

# ⚙️ Installation

## Install OpenCode

```bash
curl -fsSL https://opencode.ai/install | bash
```

Global config:

```
~/.opencode/opencode.json
```

---

## Install Superpowers

Add to your OpenCode config:

```json
"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
```

---

## Install OpenSpec

```bash
npm install -g @fission-ai/openspec@latest
```

---

# 🚀 Getting started

```bash
cd your-project
git init

openspec init
opencode /init
```

Create a proposal:

```
/opsx:propose <feature-description>
```

> ❗ Do NOT run `/opsx:apply`

OpenSpec will generate:

* proposal
* spec
* design
* tasks

👉 Ignore design/tasks — Superpowers will handle execution.

---

# 🔄 Workflow

## High-level flow

```
OpenSpec → Handoff → Superpowers execution
```

---

## 1. OpenSpec (planning phase)

Define:

* Problem
* Requirements
* Constraints
* Acceptance criteria
* Key decisions

---

## 2. Handoff to Superpowers

Use a structured execution prompt:

```md
Use .opencode/specs/feature-x/spec.md as the single source of truth.

Step 1 — Planning:
- Read and analyze the spec
- Create a detailed implementation plan
- Break the plan into concrete tasks
- Identify risks and edge cases
- Highlight any OpenAPI changes
- Consider brownfield constraints

Do NOT implement yet.

Step 2 — Wait for approval.

Step 3 — Execution (after approval):
- Implement tasks step-by-step
- Follow all instructions (backend, testing, brownfield, OpenAPI)
- Prefer TDD: write tests before or alongside implementation
- Ensure all tests pass

Step 4 — Validation:
- Verify no breaking changes
- Ensure OpenAPI alignment
- Perform critical self-review
```

---

# ⚙️ OpenCode configuration

`~/.opencode/opencode.json` or `.opencode/opencode.json` (global vs project)

```json
{
   "$schema": "https://opencode.ai/config.json",
   "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"],
   "agent": {
      "orchestrator": {
         "description": "Coordinates agents and manages workflow from plan to completion.",
         "mode": "primary",
         "model": "github-copilot/gpt-5.4",
         "prompt": "You are the orchestrator agent responsible for coordinating a multi-agent software system.\n\nYou MUST:\n- fully understand the user request before delegating\n- break tasks into phases (plan → build → review)\n- assign work to the correct agent\n- enforce execution order strictly\n- ensure outputs are validated before completion\n\nRules:\n- You do NOT implement full solutions unless absolutely necessary\n- You must always send implementation work to the build agent\n- You must always send validation work to the code-reviewer\n- You may loop between build and reviewer until correct\n\nThink like a technical project manager controlling autonomous engineers. You must wait for each agent to complete its task before moving to the next step. You must not simulate or assume outputs.",
         "tools": {
            "read": true,
            "write": false,
            "edit": false,
            "bash": false
         }
      },
      "plan": {
         "description": "Breaks requests into clear step-by-step plans.",
         "mode": "primary",
         "model": "github-copilot/gpt-5.4-mini",
         "prompt": "You are the planning agent.\n\nYour job is to convert user requests into a clear, step-by-step execution plan.\n\nYou MUST:\n- break work into small actionable steps\n- identify dependencies and risks\n- NOT write code\n- NOT assume implementation details\n\nOutput must be structured and executable by another agent.",
         "tools": {
            "read": true,
            "write": false,
            "edit": false,
            "bash": false
         }
      },
      "build": {
         "description": "Writes and modifies code based on the plan.",
         "mode": "primary",
         "model": "github-copilot/gpt-5.4",
         "prompt": "You are the builder agent.\n\nYou implement code exactly according to the plan.\n\nRules:\n- do NOT redesign architecture\n- do NOT create your own plan\n- follow steps sequentially\n- prefer small incremental changes\n- ensure code is runnable and complete\n\nIf something is missing, request clarification instead of guessing.",
         "tools": {
            "read": true,
            "write": true,
            "edit": true,
            "bash": true
         }
      },
      "code-reviewer": {
         "description": "Reviews code for bugs, security, and quality issues.",
         "mode": "subagent",
         "model": "github-copilot/claude-sonnet-4.6",
         "prompt": "You are a strict code reviewer.\n\nYour job is to audit code for:\n- bugs and logic errors\n- security vulnerabilities\n- performance issues\n- maintainability problems\n\nRules:\n- do NOT modify code\n- do NOT suggest large rewrites unless necessary\n- provide actionable, specific feedback\n- prioritize correctness over style",
         "tools": {
            "read": true,
            "write": false,
            "edit": false
         }
      },
      "researcher": {
         "description": "Finds and summarizes relevant technical information.",
         "mode": "subagent",
         "model": "github-copilot/gemini-3.1-pro-preview",
         "prompt": "You are a research agent.\n\nYour job is to gather and synthesize relevant technical information.\n\nYou MUST:\n- use websearch when needed\n- summarize clearly and concisely\n- focus on facts, APIs, and documentation\n- avoid speculation\n\nDo NOT write implementation code.",
         "tools": {
            "read": true,
            "write": false,
            "edit": false,
            "websearch": true
         }
      },
      "tester": {
         "description": "Runs checks and tests to verify correctness.",
         "mode": "subagent",
         "model": "github-copilot/gpt-5.4-mini",
         "prompt": "You are a testing agent.\n\nYour job is to validate correctness of implementations.\n\nYou MUST:\n- write and/or execute tests\n- check edge cases\n- verify expected behavior\n- report failures clearly\n\nIf code is invalid, explain exactly why it fails.",
         "tools": {
            "read": true,
            "write": false,
            "bash": true
         }
      }
   },
   "provider": {
      "lmstudio": {
         "npm": "@ai-sdk/openai-compatible",
         "name": "LM Studio (local)",
         "options": {
            "baseURL": "http://127.0.0.1:1234/v1"
         },
         "models": {
            "google/gemma-4-26b-a4b": {
               "name": "Gemma 4 26b a4b (local)",
               "tools": true,
               "limit": {
                  "context": 128000,
                  "output": 8192
               }
            }
         }
      }
   }
}
```

---

# 📜 Instructions (rules)

Location:

```
.opencode/instructions/
```

Structure:

```
00-core.md
10-backend-java-kotlin.md
20-openapi.md
30-testing.md
40-brownfield.md
50-frontend-angular.md
90-workflow.md
```

# Opencode instructions

.opencode/instructions/
00-core.md
10-backend-java-kotlin.md
20-openapi.md
30-testing.md
40-brownfield.md
50-frontend-angular.md
90-workflow.md

## 00-core.md
You are working in a production-grade engineering environment.

Principles:
- Prefer correctness over speed
- Prefer small, safe changes over large rewrites
- Always respect existing architecture unless explicitly told otherwise
- Assume this is a brownfield system unless specified otherwise

General rules:
- Do not introduce unnecessary abstractions
- Keep changes minimal and localized
- Follow existing patterns in the codebase
- If uncertain, ask or make the safest assumption

## 10-backend-java-kotlin.md
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

## 20-openapi.md
OpenAPI is the source of truth for all API contracts.

Rules:
- All backend APIs must align with OpenAPI spec
- Any API change must update OpenAPI
- Do not break existing contracts without explicit instruction
- DTOs must map directly to OpenAPI schemas where possible

Compatibility rules:
- Prefer additive changes (new fields/endpoints)
- Avoid renaming or removing fields
- Use versioning if breaking changes are unavoidable

Validation:
- Always ensure request/response models match OpenAPI definitions

## 30-testing.md
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

## 40-brownfield.md
This is a brownfield codebase.

Rules:
- Do NOT refactor unrelated code
- Do NOT “clean up” code unless explicitly requested
- Keep changes small, incremental, and localized
- Preserve existing API behavior unless specified
- Respect legacy patterns even if they are not ideal

Safety rules:
- Never break existing endpoints
- Always consider backward compatibility
- Introduce changes in a non-invasive way

Refactoring policy:
- Only refactor when it is directly required by the feature
- Refactors must be minimal and justified

## 50-frontend-angular.md
Frontend stack: Angular (standalone components)

Rules:
- Use standalone components (no NgModules unless required by legacy code)
- Prefer reactive patterns (RxJS where appropriate)
- Keep components small and focused
- Separate UI from business logic (use services)

State management:
- Prefer simple service-based state unless complexity requires more

Best practices:
- Avoid logic in templates
- Keep components declarative
- Use Angular best practices for change detection efficiency

## Workflow (must be followed)

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

---

## 🧠 Key principles

* Quality over speed
* Small, safe changes
* Brownfield-first mindset
* OpenAPI as source of truth
* TDD as default
* Feedback loops drive correctness

---

# 🔁 Execution Workflow (core of the system)

## 0. Setup (version control)

* Ensure the project is a git repository
* Create a new branch per feature
* Use a **git worktree** for isolation
* Do NOT modify main directly
* Commit small, logical steps
* Never leave the worktree in a broken state

---

## 1. Understand context

* Read existing code
* Identify patterns
* Check OpenAPI specs
* Apply instructions

---

## 2. Plan before coding

* Identify impacted areas
* Break into small steps
* Identify risks
* Consider OpenAPI impact

> ❗ Do not start coding without a clear plan

---

## 3. Implement (incremental)

* Small, safe changes
* Follow architecture rules
* Respect brownfield constraints
* Prefer TDD

---

## 4. Feedback loop (MANDATORY)

After each step:

### Tests

* Run unit + integration tests
* Fix failures immediately

### Complexity

Check for:

* large methods
* deep nesting
* duplication
* poor structure

Refactor locally if needed (without breaking behavior)

Repeat until:

* tests pass
* complexity is acceptable

---

## 5. Validate

* No breaking changes
* OpenAPI alignment
* No regressions
* Critical self-review

---

# ✅ Definition of Done

A task is NOT complete unless:

* All tests pass
* Tests cover new/changed behavior
* No breaking changes
* OpenAPI aligned
* Changes are small and localized
* Complexity is acceptable

---

# 🧠 Key Insight

> Tests protect correctness
> Complexity protects maintainability
> Worktrees protect your codebase

Together they form a **closed feedback loop system**.

---

# 🚧 TODO

* Add architecture examples (ports/adapters, clean architecture)
* Add Kotlin/Java/Angular code examples
* Add Detekt / linting configuration
* Add reusable prompt templates
* Rules builder: https://www.agentrulegen.com/

---
