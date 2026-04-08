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
