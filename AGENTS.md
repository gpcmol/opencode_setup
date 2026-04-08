# Workspace Guide

- This folder is a fresh OpenCode workspace; there is no app code here yet.
- `opencode.json` should load Superpowers with `"plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]`.
- Use OpenCode's native `skill` tool for Superpowers skills.
- Restart OpenCode after changing `opencode.json`.

# Instructions

Follow all rules defined in:

.opencode/instructions/

This includes:
- 00-core.md (general principles)
- 10-backend-java-kotlin.md (backend rules)
- 20-openapi.md (API contracts)
- 30-testing.md (testing strategy)
- 40-brownfield.md (safety constraints)
- 50-frontend-angular.md (frontend rules)
- 90-workflow.md (execution workflow)

These instructions are the authoritative source for:
- coding standards
- architecture rules
- testing requirements
- workflow and validation

Do not override or ignore these instructions unless explicitly told.
