# RAMA Factory Software — Codex Instructions

## Mission

Build a simple, reliable factory operations application for Rama Pure Water Pvt. Ltd. and Phoenix-branded products manufactured through the same factory operation.

The system must cover:

- User Management
- Product / SKU Masters
- Production Planning and Entry
- Inventory / Stores
- Quality Control
- Packaging
- Dispatch
- Customer Complaints
- CAPA
- Reports / Exports
- Senior Management Control Tower
- Audit Trail

## Mandatory reading order

Before implementing or changing a feature, read:

1. `docs/product/PROJECT_CONTEXT.md`
2. `docs/product/FRD.md`
3. `docs/product/BUSINESS_RULES.md`
4. `docs/product/USER_ROLES.md`
5. `docs/product/APPROVAL_MATRIX.md`
6. `docs/product/DECISIONS.md`
7. The relevant workflow under `docs/workflows/`

Where requirements conflict, do not guess. Record the conflict and ask for a decision before implementing the conflicting behavior.

## UX principles

The factory application must be extremely easy to operate.

Prefer:

- large, obvious action buttons
- short forms
- dropdowns instead of free text
- clear status badges
- role-specific screens
- guided workflows
- readable tables
- clear error messages
- download-template buttons beside bulk uploads
- preview-before-submit for imports

Avoid:

- deep menus
- technical ERP terminology
- excessive fields
- duplicate data entry
- hidden workflows
- screens showing modules the user cannot use
- manual calculations users could get wrong

## Non-negotiable product rules

1. Support both **single entry** and **Excel/CSV bulk upload** where operational data is entered.
2. Customer complaints are entered/uploaded by Customer Support. The MVP must not automatically read complaint emails.
3. Ticket number is mandatory for complaint intake.
4. Inventory balances are transaction-derived. Users must not directly overwrite historical balances.
5. Corrections use adjustment transactions with reason, user, timestamp, and approval where configured.
6. Every sensitive action must be auditable.
7. Approval rules must be configurable through User Management / system settings.
8. Users see only modules and actions relevant to their role.
9. Filter-set quantity is not manually entered by the factory. Inner and outer chambers are tracked independently.
10. Quality complaints must be classified using manufacturing batch and CAPA effective date, not complaint date alone.
11. QC release must occur before stock can become dispatch-ready.
12. Stock allocation and physical dispatch confirmation are separate actions.
13. Bulk import must validate, preview, report errors, and never silently discard rows.
14. A failed row in a bulk upload must not partially corrupt valid inventory or production history.
15. Historical transactions should be immutable except through auditable correction mechanisms.

## Architecture baseline

Use a modular monolith.

Preferred stack:

- Next.js
- TypeScript
- PostgreSQL
- Docker / Docker Compose
- Prisma or Drizzle ORM
- Server-side authorization enforcement
- Object/file storage abstraction for attachments

Development path:

1. Localhost / Docker
2. Hostinger VPS MVP
3. AWS or equivalent later

Do not introduce microservices, Kubernetes, Kafka, distributed queues, or other infrastructure unless a measured requirement justifies them.

## Engineering quality

Before marking a module complete:

- run lint
- run type checking
- run automated tests
- test role permissions
- test approval boundaries
- test validation
- test audit logging
- test empty/error states
- test single-entry flow
- test bulk-upload flow when applicable
- verify against FRD acceptance criteria

Never mark work complete with failing tests.

## Change discipline

When a business decision changes:

1. Update the relevant document in `docs/product/`.
2. Add or amend a decision in `docs/product/DECISIONS.md`.
3. Update implementation and tests.
4. Commit the documentation change together with the code change when practical.

The repository, not chat history, is the durable source of truth.
