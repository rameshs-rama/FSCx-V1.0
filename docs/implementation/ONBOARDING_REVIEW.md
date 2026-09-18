# Onboarding review and Phase 1 approval package

Reviewed: 2026-09-18. Source baseline: `80a1417` on `main`.

Status: preparation completed; Phase 1 is proposed, not approved or implemented.

## Outcome

The repository contains product and workflow specifications, with no runnable application, database schema, package manifest, or Docker configuration. The initial delivery should be a localhost foundation: authentication, User Management, configurable permissions and approval definitions, audit logging, and a simple role-aware layout.

The current user request makes localhost the delivery target. Hostinger and AWS remain future options from the source documents; no external hosting is part of this work.

All eight files in `docs/product/`, all seven files in `docs/workflows/`, `AGENTS.md`, `README.md`, `CODEX_ONBOARDING_PROMPT.md`, the existing phase plan, and template/UI guidance were reviewed. The onboarding prompt explicitly says: "After the review, stop and wait for approval before implementing Phase 1."

## Review deliverables

- [Requirements traceability](REQUIREMENTS_TRACEABILITY.md): all 62 numbered FRD requirements plus supplemental requirements from unnumbered sections and workflows.
- [Architecture and domain model](ARCHITECTURE_PROPOSAL.md): entity ownership, integrity boundaries, authorization, and proposed folder structure.
- [Localhost preparation](LOCALHOST_PLAN.md): observed machine readiness, Compose topology, setup sequence, and verification.
- [Phase 1 scope and acceptance](PHASE_1_SCOPE.md): concrete first delivery and tests.
- [Phased implementation plan](PHASE_PLAN.md): original sequence with proposed dependency and acceptance gates.

These documents propose implementation choices. They do not change accepted business decisions or grant permissions. Approved choices must subsequently be recorded in `docs/product/DECISIONS.md` and reflected in the affected product documents.

## Conflicts and gaps

| ID | Finding and source | Impact | Proposed handling / decision owner |
|---|---|---|---|
| R-01 | `AGENTS.md` requires single entry and Excel/CSV upload wherever operational data is entered; D-003 says "where practical", Packaging says "where enabled", and template guidance lists only four modules. QC/CAPA import scope is unspecified. | Bulk coverage cannot be inferred from the four template names. | Owner to confirm coverage before each operational module. Retain universal coverage as an obligation until explicitly narrowed; do not call a module complete without the required import path. |
| R-02 | FR-PK-004 permits a configured packaging exception, while AGENTS rule 11 and QC rules prohibit dispatch readiness before QC release. | A packaging exception could be misread as a QC bypass. | Keep dispatch readiness gated by QC release. Request an explicit exception workflow before implementing any pre-release packaging behavior. No exception is enabled in Phase 1. |
| R-03 | Senior Management defaults to read-only, while the approval matrix suggests high-threshold approval by Senior Management. | Suggested approver examples are not baseline grants. | Keep the default read-only. Grant business approval separately only after the owner configures it. |
| R-04 | Multi-role assignment is conditional; brand/market scoping and approval levels lack combination semantics. | Flattening scopes could leak cross-market access. | Phase 1 proposal: allow multiple explicitly assigned roles; evaluate each permission with its own role-assignment scope, never a Cartesian union of unrelated brands/markets. See P1-02 below. |
| R-05 | Import examples count valid/error rows but do not say whether a mixed file commits valid rows or blocks entirely. | Partial acceptance, retries, and duplicate handling would otherwise differ between modules. | Recommend all-or-nothing confirmation per upload, with downloadable errors and correction/re-upload. Owner decision before operational imports. |
| R-06 | Approval thresholds are examples with no values; self-approval, sequential levels, approver absence, rule overlap, and policy versioning are unspecified. | An approval engine could silently approve an unconfigured action. | Phase 1 proposal: versioned definitions, no seeded business thresholds, and no posting without an applicable configured policy. Deny self-approval by default. Operational activation requires explicit rules. |
| R-07 | Production actual, rejected/rework, inspected, and packed quantities lack precise conservation rules, units, and rounding. Component compatibility and BOM rules are unspecified. | Duplicate receipts or overstated usable stock are possible. | Decide gross/net output, split quantities, precision, component conversion, and pack requirements before production/ledger posting. Do not assume a complete set is simply `min(inner, outer)`. |
| R-08 | Stock categories mix physical stage (SFG/FG), quality (hold/rework), packaging, and reservation. | One status field cannot safely represent all dimensions. | Proposed independent state dimensions and a movement ledger; validate transitions with business owners before Phase 4. |
| R-09 | Partial allocation is policy-dependent; partial shipments, cancellation, release revocation, and allocation before packaging are unresolved. | Demand, reservation, and shipment quantities could diverge. | Define these policies before Dispatch. QC/packaging gates remain mandatory for physical dispatch. |
| R-10 | Complaint batch is optional, but CAPA classification relies on production evidence. Effective date vs effective batch precedence and equality boundaries are missing. | Unknown batches could be wrongly counted as legacy or recurrence. | Propose an unclassified review state, not an invented seventh final classification. QC must resolve evidence; never classify solely by complaint date. Confirm boundary rules before CAPA classification. |
| R-11 | CAPA effectiveness needs evidence, but observation windows, minimum exposure, ownership, re-opening, and source acronym RFD are undefined. | Internal validation could be presented as field effectiveness. | Define by severity/failure mode before CAPA. Record first shipment/exposure separately; no "zero recurrence" effectiveness claim before market exposure. Clarify RFD terminology. |
| R-12 | Authentication, bootstrap administrator, password reset, session expiry, and MFA rollout are not specified. | Cannot deliver a usable secure login without a local policy. | Proposed defaults in P1-01 and P1-03. No email service dependency for localhost. |
| R-13 | No exact audit visibility, retention, attachment limits, export permission, or backup retention rules. | Role visibility alone does not authorize export or audit access. | Explicit separate grants; minimum local proposals below. Final retention policy is a pre-live-use decision. |
| R-14 | Procurement is named in Stores responsibilities, but supplier/procurement FRD scope is conditional. | A procurement module would expand the agreed core. | Keep supplier/material links in the future domain proposal; defer purchase-order/accounting workflows pending requirements. |
| R-15 | Production precedes Inventory and QC in the phase plan. | Phase 3 cannot truthfully demonstrate end-to-end posting without downstream modules. | Design posting contracts in Phase 3; gate integrated production acceptance on Phases 4 and 5. No temporary editable stock balances. |
| R-16 | No opening stock format, actual SKU catalogue, initial users, warehouse map, or approved UI images are supplied. | Real-data go-live cannot yet be validated. | Bootstrap only an explicitly configured local administrator in Phase 1; gather masters/opening migration data before operational rollout. Demo data must be isolated and labeled. |
| R-17 | "Yesterday", shifts, backdating, low-stock/dispatch risk, and report cutoffs lack definitions. | Dashboard totals may disagree even over correct data. | Propose Asia/Kolkata business calendar with UTC timestamps; confirm movement/report definitions and permitted backdating before affected modules. |

## Duplicated requirements

Ledger immutability, single/bulk parity, server-side authorization, QC release, dispatch separation, complaint ticket requirement, and batch-based CAPA logic recur across AGENTS, FRD, business rules, decisions, audit notes, and workflows. These are reinforcing requirements, not separate features. Implement each rule in one module service used by manual entry, import, and API callers. The traceability matrix points each occurrence to shared acceptance tests.

The historical black-particle and POSTreat descriptions are reference scenarios, not evidence of today's manufacturing or market state. Do not seed them as live resolved CAPAs.

## Decisions required before Phase 1 implementation

Approval can accept this package as proposed or name changes. The proposals below are explicit requests, not silently adopted business rules.

| ID | Proposed default for approval | Why needed now |
|---|---|---|
| P1-01 | Local accounts with username/password, securely hashed passwords, revocable database sessions, administrator-assisted resets, and a one-time bootstrap command that prompts for the initial administrator credentials. No shared/demo passwords or public registration. | Establish authentication and first access without email integration. |
| P1-02 | Configurable roles; multiple role assignments allowed; deny by default; action-level grants and brand/market scopes evaluated together per assignment. Administrator gets user/access/settings administration and explicit audit-read permission, but no automatic operational approval. Other role grants remain limited to documented capabilities; conditional grants start disabled. | Establish permission composition and initial administrative access. |
| P1-03 | For the local foundation, password login with an 8-hour absolute session limit, logout/revocation, rate-limited login, and forced credential change after administrator reset. Build an MFA extension point; privileged MFA must be decided and tested before live operational use. | Set measurable local security behavior without calling MFA delivered. |
| P1-04 | Versioned approval-policy definitions and configuration screens; no seeded numeric business thresholds. Explicit ordered stages, no self-approval, fail closed for missing/ambiguous rules. Actual business approvals are added/tested with their modules. | Avoid inventing stock, dispatch, or CAPA authority. |
| P1-05 | Next.js + TypeScript + PostgreSQL + Prisma, one application/service and one database in Docker Compose. Browser endpoint `http://localhost:3000`, loopback-only publication. Asia/Kolkata business calendar; UTC event timestamps. | Fix the foundation and localhost target. |
| P1-06 | Local upload storage behind an adapter; append-only audit access for the application; repeatable database and file backup/restore commands. Keep all development audit records; agree retention and schedule before live use. | Preserve portability and recovery from the beginning. |

Docker/WSL setup is also required on this machine for the proposed route. See the [machine check](LOCALHOST_PLAN.md). An operating-system installation or restart is a separate prerequisite; no such changes were made during preparation.

## Decisions that can wait

Import acceptance policy and file/row limits must be finalized before the first operational import. Quantities, component compatibility, QC sampling, packaging checklists, approval thresholds, partial dispatch, duplicate-reference scope, unknown-batch classification, CAPA observation windows, procurement scope, and opening-stock migration must be resolved before their respective phases. They do not need to block the isolated authentication/User Management foundation, provided no unfinished business controls are exposed as working features.

## Current verification

- Repository cloned to `C:\Users\Admin\Downloads\Factory Software`.
- Source requirements and workflows read; 62 numbered FRD requirements identified.
- Documentation validation passed: all 62 numbered requirements mapped exactly once, 24 supplemental rows, no broken local links or malformed tables across the seven preparation documents, and no tracked-diff whitespace errors.
- Node.js `v22.23.2` and npm `12.0.2` detected.
- Docker and PostgreSQL tools/services not detected by the checks recorded in the localhost plan; WSL reports it is not installed.
- No app has been started and no database has been created.
- Application lint, type checks, build, and tests are not yet available because this baseline contains no application code.

Next action: obtain approval for Phase 1 and the proposed defaults, then implement the scoped foundation and demonstrate the tested localhost result.
