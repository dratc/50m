# CORE FOUNDATION — CONDENSED BRIEF (for writing module prompts)

## 1. What the project is
A Business Management Application with 50 business Modules. The "Core" is built first, then the Modules one by one. The Core must run with an empty Modules directory.

## 2. Mandatory architecture and tech stack
- Modular Monolith: one deployable PHP application. No microservices.
- PHP 8.3+, PDO SQLite, HTML5, CSS3, Vanilla JS. No Node.js at runtime.
- One central SQLite database file. All DB access goes through the Core Database Manager (PHP-only).
- Browser JS and external applications must never touch the DB directly.
- New frameworks/packages only with a documented need plus Project Owner approval.
- Nginx / HestiaCP or compatible PHP hosting. Web root = Public/.
- DB, Config and Storage must stay outside the public root.

## 3. What the Core provides (Modules consume it, never rebuild it)
- Bootstrap, Router, Request/Response, Middleware, Config, Logger, Error handler
- Authentication (password_hash/Argon2id), server-side sessions, CSRF
- Users, Roles, Permissions (RBAC, default deny)
- Shared layout, design tokens, navigation, settings
- Audit log, notifications, files, search
- Internal API registry (in-process)
- External API gateway (/api/v1/, tokens, scopes, rate limits)
- Module registry (metadata only)
- Backup/restore, diagnostics, maintenance mode
- Migration engine (run via CLI/maintenance, not on every request)

## 4. Rules for Modules (Sections 14, 15, 16, 30)
- Each Module owns its business logic, workflow, screens, status lifecycle, reports and rules. Modules must not be forced into one generic CRUD workflow.
- Each Module has its own table prefix/namespace. Core tables use the prefix core_.
- No Module may query another Module's private tables. Cross-Module data goes through the owning Module's Internal API only.
- No extra DB files. Use the Core's central connection.
- Module metadata: stable ID, name, version, description, entry route, navigation, required Core version, dependencies, permissions, API actions, migration IDs, enabled state. Duplicate IDs are rejected.
- Never include arbitrary PHP files based on directory names. Disabled Modules must not execute routes or API handlers, but their data is not deleted.
- Permission names: `module.resource.action` (e.g. core.users.view). A permission ID must never be reused for a different meaning.
- Each Internal API action declares: unique name, owner, purpose, input/output schema, permission, read/write type, error codes. Unknown actions are rejected.
- External API endpoints are created only after a security and data-exposure review. An Internal action never becomes public automatically. Scopes are narrow, with read and write separated.
- External API rules: /api/v1/ path, token in the Authorization header (never in the query string), hashed token storage, consistent JSON envelope, request ID, bounded pagination, allowlisted sort/filter, 429 rate limiting, idempotency keys for retryable writes.
- Each Module has its own migrations (immutable IDs, deterministic order). Applied migration files are never edited; fixes go in new migrations.

## 5. Universal checklist (applies to every feature)
Define the responsibility (what it does and does not own). Define the service's public interface and return structure.
Input:
- All input (forms, JSON, headers, cookies, uploads) is untrusted.
- Server-side validation is mandatory. Unknown/malformed values are rejected with stable error codes.
- Never silently coerce invalid input into a valid value.
Security:
- Authenticate first, then authorize on the server (default deny).
- Hiding UI elements is not security.
- CSRF protection on browser state-changing requests.
- Escape output by context.
- Never let user input select include or file paths.
Database:
- Prepared statements; allowlists for dynamic identifiers.
- Related writes in one transaction; keep transactions short (no network calls inside).
- DB-level unique constraints.
- Store timestamps in UTC.
- Paginated lists with a max page size. No `SELECT *`. Indexes only for real query patterns.
- Stale-update handling for concurrent edits.
Output and logging:
- Never expose hashes, tokens, secrets or filesystem paths.
- Audit security-relevant actions.
- Log failures with a request ID; never log passwords or tokens.
Data lifecycle:
- Define retention and deletion rules.
- Invalidate caches when permissions or account state change.
Tests:
- Happy path, invalid input, unauthenticated, unauthorized.
- DB/HTTP integration, DB busy/unavailable, duplicate submit/retry.
- No partial state after failure; unauthorized callers cannot infer protected data.
- Performance verified on realistic data.
Finally: update documentation and PROJECT_STATUS. Nothing is VERIFIED until tests have actually been run and passed.

## 6. Status vocabulary (use only these)
NOT_STARTED, IN_PROGRESS, BLOCKED, IMPLEMENTED_NOT_TESTED, TESTING, FAILED, VERIFIED, DEFERRED, RETIRED
- VERIFIED = the implementation exists and the documented tests actually passed. A file or page existing is not proof.

## 7. AI developer workflow (every task)
1. Read the relevant documentation. 2. Inspect the actual repository. 3. Confirm scope. 4. Identify dependencies and risks. 5. Plan the smallest safe change. 6. Implement. 7. Run PHP syntax checks. 8. Run tests. 9. Inspect the Git diff. 10. Update documentation and status. 11. Report with evidence.
- Don't assume files exist. Don't overwrite existing work.
- Tests not run must be marked `NOT RUN`.
- The report must list: files created/changed, DB changes, API changes, security impact, test results, known issues, remaining work.
- Ask the Project Owner only for genuine business decisions, architecture conflicts, security-sensitive changes or destructive operations. If two specs conflict, report the exact clauses instead of silently picking the easier one.
- No endless planning. Once scope is clear, implement.

## 8. Core acceptance (all must be true before any Module starts)
App starts with an empty Modules directory; first-admin setup works; login/logout; session regeneration and revocation; user CRUD and deactivation; roles/permissions and authorization denial; audit events persist; responsive layout; migrations; Internal API registry; External API token auth + scope enforcement + revocation; backup/restore tested; production errors expose no secrets.

## 9. Open Owner decisions (per the document)
Single vs multi-organization, login identifier (username/email/phone), self-registration vs admin-created accounts, multiple roles per user, password policy, session lifetimes, login throttling, MFA, password recovery channel, audit retention, API rate limits/page size/version support, webhooks, backup frequency/destination/encryption, RTO/RPO.
