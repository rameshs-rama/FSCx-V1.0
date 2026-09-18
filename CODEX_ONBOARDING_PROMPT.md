# First Codex Prompt

Use the following prompt as the first development task.

---

You are taking over development of **FSCx V1.0 — RAMA Factory Software**.

Do not write application code yet.

First:

1. Read `AGENTS.md`.
2. Read every file under `docs/product/`.
3. Read every file under `docs/workflows/`.
4. Review `README.md`.
5. Identify contradictions, missing requirements, duplicated requirements, and ambiguous business rules.
6. Produce a **Requirements Traceability Matrix** mapping:
   - Requirement
   - Module
   - Screen
   - Database entity/entities
   - Role/permission
   - Approval requirement
   - Validation rule
   - Test case
7. Propose the database domain model.
8. Propose the repository/application folder structure.
9. Propose the local Docker development architecture.
10. Propose the phased implementation plan.
11. Identify what must be decided before Phase 1 begins.

Constraints:

- Keep the system simple.
- Do not introduce unnecessary architecture.
- Do not infer permissions that are not documented.
- Do not let frontend visibility substitute for backend authorization.
- Do not implement the whole FRD in one pass.
- Where documents conflict, identify the conflict instead of silently choosing one interpretation.
- Preserve auditability and transaction integrity.

After the review, stop and wait for approval before implementing Phase 1.

---

## Recommended Phase 1 after approval

Foundation only:

- Next.js / TypeScript project
- Docker development environment
- PostgreSQL
- schema migration setup
- authentication foundation
- User Management
- roles
- permissions
- approval levels
- audit logging
- base layout/navigation
- automated test foundation

Do not start Production, Inventory, QC, Dispatch, Complaints, or CAPA until Phase 1 is reviewed.
