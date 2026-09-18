# Phase 1 — Localhost foundation for approval

Status: proposed. Implement only after acceptance of the [onboarding review](ONBOARDING_REVIEW.md) and P1-01 through P1-06, or after recording requested alternatives.

## First usable delivery

Open `http://localhost:3000`, sign in with a locally configured administrator account, manage users and their access, configure approval definitions, and inspect audit history. Persist data in PostgreSQL across restarts. The interface should use clear page titles, obvious primary actions, accessible forms, readable tables and short validation messages.

Operational modules are delivered in later phases. The first screen must not show fabricated production, stock or complaint numbers. Users without an implemented permitted workspace get a clear explanation and a way to contact their administrator.

## Deliverable checklist

- Next.js/TypeScript application with locked dependencies, lint/type-check/test/build scripts and a documented environment example.
- Docker Compose application, PostgreSQL and one-shot migrations; loopback-only web endpoint and persistent data.
- Login, logout, password hashing, database sessions, expiry, rate limiting, reset and deactivation behavior under the approved local policy.
- One-time bootstrap administrator command without committed default credentials; prevent accidental removal of the last active administrative access path.
- User list, add/edit user, activate/deactivate and role/scope assignment screens. Department is a controlled value.
- Role/permission configuration with explicit module/action grants and scope checks on server reads and mutations.
- Versioned approval-definition screens: draft, validate, publish and retire a definition. Business execution is added with its owning module; no live operational approval queues yet.
- Audit history with time/actor/action/subject filters and explicit read permission; redact secrets and protect runtime DB immutability.
- Shared layout/navigation derived from implemented permissions; loading, empty, unavailable, denied and invalid-form states.
- Local file-storage interface and configuration contract; actual operational evidence uploads arrive with their modules.
- Isolated automated-test database, representative test role fixtures, backup/restore commands and a tested localhost runbook.

Phase 1 reference data includes role/permission definitions, departments and minimal brand/market scope references. Full SKU/product/warehouse masters remain Phase 2. Named employees in the requirements are responsibility examples; do not create real accounts or assign them credentials without initial-user information.

## Required checks

| Area | Acceptance evidence |
|---|---|
| Reproducibility | Fresh install, lockfile install, migration from empty DB, successful restart without data loss, production build. |
| Tooling | Lint, TypeScript check, unit tests, integration tests and browser smoke tests all pass. Build is not a substitute for lint. |
| Authentication | Correct/incorrect password, rate limit, expiry, logout, reset, deactivated user and session replay after deactivation tested. No plaintext credential/token in audit or application logs. |
| Authorization | Direct endpoint denial; view vs write distinction; missing grant denied; scopes checked on list/detail/mutation; multi-role grants cannot create unintended scope combinations. |
| Administration | Create/edit/activate/deactivate audited; duplicate normalized usernames rejected; stale edits detected; last active administrative path protected. |
| Approval definitions | Invalid/overlapping conditions and invalid stages rejected; versions retained; undefined policies do not grant authority; role name alone does not imply business approval. |
| Audit | Safe before/after records; mutation and audit atomic; failed attempts attributable without leaking credentials; runtime DB role cannot update/delete audit events. |
| User experience | Login, user creation and access change usable by keyboard; responsive layout; clear empty/error/denied states; explicit confirmation before permission changes. |
| Local operation | Loopback-only publication, DB not exposed on LAN, health/readiness checks, useful DB-down behavior, migration failure blocks readiness. |
| Recovery | Back up and restore users/access/settings/audit into an isolated database; demonstrate the file-volume backup contract. |

Meaningful tests include concurrent administrative changes that could otherwise remove all administrators, permission revocation during an active session, a failed audit insert rolling back its associated mutation, and database-level attempts to change audit records. Use a real PostgreSQL test database for transactional and privilege checks.

## Handover record to produce after implementation

Record exact dependency/runtime versions, actual start/stop/migration/bootstrap commands, the reachable local URL, successful check outputs, backup location/restore procedure, and any known limitations. Initial credentials must be set through the bootstrap flow rather than published in the repository or task response.

Phase 1 is complete only after these checks pass and the user reviews the working foundation. Production, Inventory, QC, Packaging, Dispatch, Complaints and CAPA are not implicitly approved by approving this phase.
