# Proposed Implementation Phases

This is a planning baseline. Codex should first perform the onboarding audit before coding.

The [2026-09-18 onboarding review](ONBOARDING_REVIEW.md), [traceability matrix](REQUIREMENTS_TRACEABILITY.md), [architecture proposal](ARCHITECTURE_PROPOSAL.md), [localhost plan](LOCALHOST_PLAN.md), and [Phase 1 scope](PHASE_1_SCOPE.md) are now available for approval. This is a documentation-only checkpoint; implementation has not begun.

## Phase 0 — Requirements validation

- read all docs
- traceability matrix
- resolve contradictions
- confirm data model
- confirm permission model
- confirm import templates
- confirm dashboard KPIs

## Phase 1 — Foundation

- Next.js / TypeScript
- Docker Compose
- PostgreSQL
- ORM/migrations
- authentication
- base layout
- User Management
- roles/permissions
- approval model
- audit log
- test foundation

## Phase 2 — Masters

- brand
- market
- SKU/product
- categories
- warehouse/location
- reason codes
- complaint categories
- configurable thresholds/settings

## Phase 3 — Production

- planning/work order baseline
- single production entry
- production bulk upload
- validation
- QC handoff

## Phase 4 — Inventory / Stores

- transaction ledger
- stock states
- adjustment workflow
- current balance
- movement reports
- low-stock alerts

## Phase 5 — QC

- queue
- pass/hold/rework/reject
- attachments
- aging
- NCR/CAPA initiation

## Phase 6 — Packaging

- packaging queue
- pack-out confirmation
- damage/rework
- dispatch readiness

## Phase 7 — Dispatch

- sales demand
- bulk upload
- allocation
- picklist
- release
- physical dispatch confirmation
- risk dashboard

## Phase 8 — Complaints

- single complaint entry
- bulk upload
- ticket tracking
- factory assignment
- batch mapping

## Phase 9 — CAPA

- root cause
- actions
- effective batch/date
- effectiveness monitoring
- closure approval
- recurrence classification

## Phase 10 — Management Dashboards / Reports

- control tower
- exception queues
- exports
- movement trends
- complaint/CAPA surveillance

## Phase 11 — UAT / MVP hardening

- permissions audit
- backup/restore test
- import stress test
- data migration plan
- Hostinger deployment
- user training
- launch checklist

## Proposed dependency and acceptance gates

The phase numbers above are preserved. The following refinements are proposals from the onboarding review, not approval to begin implementation.

| Phase | Entry decision / prerequisite | Exit evidence |
|---|---|---|
| 0 | Review existing source documents | Traceability and proposals prepared; Phase 1 decisions accepted. Current state: review prepared, approval pending. |
| 1 | Accept P1-01 through P1-06 or record alternatives; prepare Docker runtime | Tested localhost login/User Management/permissions/approval definitions/audit; user review of foundation. |
| 2 | Initial catalogue, units, warehouse and master-data ownership | Controlled auditable masters and explicit edit/read grants; scope reference data consistent. |
| 3 | Decide quantity semantics, batch/reference uniqueness, component compatibility and import acceptance | Planning and actuals with single/bulk parity; posting contracts prepared. Integrated production completion remains pending Phases 4–5. |
| 4 | Confirm ledger dimensions, negative-stock policy, opening migration, adjustments/thresholds | Ledger, immutable corrections, atomic source receipts, reconciliation, allocation contract and movement reports tested with PostgreSQL. |
| 5 | Confirm QC quantities, sample/disposition rules, release grants and reason codes | Production-to-ledger-to-QC flow passes; held stock cannot become usable. NCR source link ready; full CAPA workflow waits for Phase 9. |
| 6 | Approve pack specifications, damage handling, bulk scope and any exception workflow | Packaging respects QC gates, conserves quantities and records checks/evidence. |
| 7 | Confirm partial allocation/shipment/cancellation policy and dispatch authority | Separate demand/allocation/release/shipment; concurrent allocations safe; retry posts one issue. |
| 8 | Confirm ticket uniqueness scope, severity/category values, attachments and batch mapping | Manual/bulk intake, CS/QC boundaries, visible review progress without email dependency. |
| 9 | Confirm effective date/batch boundaries, unknown-batch handling, observation and closure rules | Evidence-gated closure and source links; legacy/watch/recurrence classifications tested, including pre-market scenarios. |
| 10 | Approve KPI definitions, date windows, filters/export grants | Control Tower and reports reconcile to operational records; scopes enforced before aggregation. Role queues arrive with their owning modules. |
| 11 | Real master/opening data and representative users available | End-to-end UAT, import failure/retry/load tests, recovery rehearsal, user training and accepted operational controls. Localhost remains current hosting target; external hosting is future work. |

Resolve each phase's open business decisions before implementing the affected behavior. Avoid placeholder controls that imply a policy has been enforced. Every module exit includes lint, type checking, automated tests, role and approval boundaries, validation, audit, empty/error states, single-entry flow and applicable bulk flow.
