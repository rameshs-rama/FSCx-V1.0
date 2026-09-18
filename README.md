# FSCx V1.0 — RAMA Factory Software

Factory operations software for **Rama Pure Water Pvt. Ltd.** covering production, inventory, quality control, packaging, dispatch, customer complaints, CAPA, reporting, and role-based user management.

## Current stage

This repository is initialized for local-first development with ChatGPT Codex.

- Development: localhost / Docker
- MVP deployment: Hostinger VPS
- Future production: AWS or equivalent
- Expected users: ~20 total, normally fewer than 10 concurrent
- Architecture direction: Next.js + TypeScript + PostgreSQL + Docker
- Application style: modular monolith

## Start here

Codex and developers should read in this order:

1. `AGENTS.md`
2. `docs/product/PROJECT_CONTEXT.md`
3. `docs/product/FRD.md`
4. `docs/product/BUSINESS_RULES.md`
5. `docs/product/USER_ROLES.md`
6. `docs/product/APPROVAL_MATRIX.md`
7. `docs/product/DECISIONS.md`
8. Relevant files under `docs/workflows/`
9. `CODEX_ONBOARDING_PROMPT.md`

Do not begin implementation before completing the onboarding review in `CODEX_ONBOARDING_PROMPT.md`.

## Product principle

Keep the application simple enough for a first-time, low-technical-skill factory user to understand with minimal training. Complexity should live in business rules and controls, not in the user interface.
