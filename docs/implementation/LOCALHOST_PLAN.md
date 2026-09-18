# Localhost preparation plan

Status: proposed, not yet executable. No application, Compose file, database or localhost server exists as of 2026-09-18.

## Machine readiness

Workspace: `C:\Users\Admin\Downloads\Factory Software`.

| Item | Observation | Next step |
|---|---|---|
| Source | Repository cloned; `main`, baseline `80a1417` | Preserve product documents and onboarding instructions. |
| Node.js | `v22.23.2` detected | Verify exact package compatibility when dependencies are pinned. |
| npm | `12.0.2` detected; `npm.cmd` available | Commit lockfile; use reproducible installs. |
| Docker | Not on PATH; standard Docker Desktop executable/CLI paths absent; `com.docker.service` not found | Install/configure Docker Desktop if the proposed route is approved. |
| WSL | `wsl --status` reports WSL is not installed | Set up the supported WSL 2 backend and verify virtualization prerequisites for Docker. |
| PostgreSQL | `psql` not on PATH; no `postgresql*` service detected | Use the Compose database; a separate native PostgreSQL install is not required for that route. |
| Application | No package manifest, application source, migrations, or Docker configuration in source baseline | Implement Phase 1 after review approval. |

These checks do not prove that no custom database/runtime exists anywhere on the machine; they establish that the standard setup is not available to this workspace. Installation has not been attempted.

Current Next.js documentation supports Windows and requires Node.js 20.9 or newer; the detected Node version meets that minimum. Exact compatibility with the selected ORM/authentication versions must still be checked. [Next.js installation](https://nextjs.org/docs/app/getting-started/installation).

Docker Desktop's Windows installation documentation describes supported Windows versions, WSL 2 backend requirements, and virtualization prerequisites. Verify those requirements before installing; a system restart may be required. [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/).

## Proposed Compose services

| Service | Purpose | Host access / persistence |
|---|---|---|
| `app` | Next.js pages, server endpoints and module services | Publish `127.0.0.1:3000:3000`; bind to all interfaces only inside the container. Separate volumes for dependencies/build cache in development. |
| `db` | PostgreSQL | Private Compose network; no host port by default. Named database volume survives container recreation. |
| `migrate` | One-shot migration using elevated migration credentials | Wait for DB health; app starts only after migration success. No web endpoint. |

Use distinct migration/runtime DB users; the runtime user must not update/delete audit events or bypass posted-history protections. Test actual DB privileges, not only application conventions. Environment variables are validated before startup; `.env.example` contains placeholders only. `.env`, credentials, uploads, backups and local artifacts must remain excluded from Git.

The `DATABASE_URL` inside Compose uses the `db` service name. It must not use `localhost`, which would point to the application container itself. Database health uses `pg_isready`; readiness checks migrations and a database query. Liveness reports only that the process is alive. Neither endpoint returns credentials or internal diagnostics.

Local files live in an uploads volume behind an authenticated download route, never a public directory. Database backups and file backups need a coordinated manifest. Protect backup access and test a restore to an isolated database and separate upload location. Daily backup capability is required; final scheduling/retention is decided before live use.

## Expected setup sequence after approval

1. Verify Windows/virtualization prerequisites and install/start the required Docker/WSL runtime. Confirm `docker version` and `docker compose version`.
2. Scaffold the Phase 1 application without overwriting existing requirements or `AGENTS.md`. Resolve compatible supported package/container versions and lock dependencies.
3. Add Compose services, development configuration, persistent volumes, `.env.example`, migrations and separate test settings.
4. Create local environment secrets; start the database and run migrations explicitly. Never silently reset an existing database.
5. Run the bootstrap-admin command interactively; prompt for credentials and store only a password hash. No credentials are committed or included in logs.
6. Start the application at `http://localhost:3000`, confirm login, configure users, and verify role-aware navigation and direct endpoint denial.
7. Run the [Phase 1 checks](PHASE_1_SCOPE.md) and document actual launch/stop/backup commands once they exist.

The intended final developer interface is `docker compose up --build` to start and `docker compose stop` to stop while retaining data. These are target commands, not usable instructions for the current documentation-only repository. Bootstrap is a separate explicit step. A future backup/restore script must never reset data implicitly.

## Development and local operation

Development uses source mounts and hot reload. Repeatable local operation should use a built production image after acceptance, with migrations as a controlled step. Both use the same schema and code. Localhost means access from this computer only; other factory computers cannot access it through their own `localhost` address. LAN access, HTTPS and startup-at-boot would be separately configured if requested.

If Docker cannot be enabled, a native Node.js + native PostgreSQL setup is a possible alternative using the same codebase and migrations. It is not currently available or configured. Do not silently replace PostgreSQL with browser storage, SQLite or an external hosted database.

## Localhost verification before handover

- A fresh setup and a restart both work; data survives application/container restart.
- The application listens on loopback port 3000, and PostgreSQL is not exposed to the LAN.
- Database outage produces a useful unavailable state; readiness fails without leaking secrets.
- Migration failure prevents startup rather than serving against a partial schema.
- Login, logout, expiry, reset and deactivation work against real persisted sessions.
- Direct requests cannot bypass role, scope or approval restrictions.
- Audit writes persist with successful mutations, are redacted, and cannot be changed by the runtime DB user.
- Lint, type check, unit/integration/browser tests and production build pass.
- A backup is restored into an isolated target and inspected successfully.

No items above are claimed as passed during this documentation-only preparation.
