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
