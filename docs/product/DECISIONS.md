# Decision Log

## D-001 — Filter set quantity

Factory operators do not manually enter filter-set quantity.

Inner and outer chambers are tracked independently. Any derived complete-system quantity must be calculated by business logic where applicable.

## D-002 — Complaint intake

The MVP does not read customer complaint emails automatically.

Customer Support enters complaints through:

- single complaint entry
- Excel/CSV bulk upload

Ticket number is mandatory.

## D-003 — Single + bulk entry

Operational modules should support both single-entry and bulk upload where practical.

Bulk upload is an alternative input method, not a separate business process. The same validation, permission, approval, and audit rules apply.

## D-004 — Inventory ledger

Inventory is transaction-derived and historical movement is immutable.

Corrections are made through adjustment/reversal transactions rather than editing past history invisibly.

## D-005 — Black particle complaints

Known historical black-particle complaints from pre-fix stock are classified as legacy market exposure unless evidence shows post-CAPA production.

Complaint date is insufficient to determine recurrence.

## D-006 — POSTreat V2 rust

The issue progressed through internal corrective action and validation.

The software must distinguish internal validation from market effectiveness. A field-effectiveness conclusion requires corrected product to have reached the market.

## D-007 — Role-specific UI

Users should see only modules/actions relevant to their role.

Do not show a full ERP menu to every user.

## D-008 — Approval configuration

Approval levels and permissions must be manageable in User Management / Settings rather than hard-coded per employee name.

Named people are initial role mappings only.

## D-009 — Architecture

Use a modular monolith.

Preferred:
- Next.js
- TypeScript
- PostgreSQL
- Docker

Do not use microservices for V1.

## D-010 — Deployment path

Development:
- local system / localhost

MVP:
- Hostinger VPS

Future:
- AWS or equivalent

The application must be portable between environments.

## D-011 — Expected scale

Approximately 20 users with fewer than 10 concurrent users initially.

Optimize for correctness, simplicity, and maintainability rather than high-scale distributed architecture.

## D-012 — Brand/market mapping

RAMA is the India-facing brand context.

Phoenix is used for international markets including UK, USA, and France.

Sample/demo data must not label all international rows as RAMA.

## D-013 — Dispatch separation of duties

Creating dispatch demand, allocating stock, and confirming physical dispatch are distinct actions and can be assigned to different roles.

## D-014 — User management scope

User Management must support:

- user activation/deactivation
- role assignment
- module permission
- action permission
- approval level
- brand/market scope where needed
- audit of changes

## D-015 — Ease of use

The product must favor obvious workflows over feature density.

If a screen requires significant training to perform a routine daily task, redesign it before adding more functionality.
