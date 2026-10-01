# Project Context

Durable, non-secret project facts Claude reads before making changes. Field meanings, examples, and who answers each live in `developer-handbook/reference/PROJECT-CONTEXT-UNDERSTANDING.md`. Every value below was read from this repository; where the repository cannot answer a field, the field says so in plain words rather than guessing.

Never store secrets here. Hosts, database names, users, and links that are not known are described in words, never filled with a real value from memory.

This repository is a Claude Code kit (template) repository. It holds the Claude Code gateway kit itself, `CLAUDE.md`, `.claude/`, and `developer-handbook/`, and no application code. The gateway version in it is recorded at the top of `.claude/README.md` and in `.claude/CHANGELOG.md`. Most application fields below are therefore marked not applicable; they get real values only in an application repository that adopts the kit.

## Required Intake Status

- Business purpose: holds the Claude Code gateway kit (`CLAUDE.md`, `.claude/`, `developer-handbook/`) that developers copy into application repositories so Claude Code works under one set of rules, commands, agents, and skills there.
- Primary users: developers who copy the kit into their projects, and the Claude Code sessions that read it.
- Application type: not an application. A kit/template repository made of Markdown rule, command, agent, and skill files, two JSON settings files, one PowerShell script (`.claude/scripts/override-check.ps1`), and a static HTML, CSS, and JavaScript reference layout in `developer-handbook/layout-template/`.
- Backend stack and version: not applicable: this repository holds no application code.
- Frontend stack and version: not applicable: this repository holds no application code. `developer-handbook/layout-template/` is a static reference layout (plain HTML, CSS, and JavaScript, no framework or build step), not a frontend.
- Database engine and version: not used: this repository holds no application code and connects to no database.
- Package manager/runtime versions: none: the repository has no package manifest, lockfile, or runtime version file.
- Authentication model: not applicable: this repository holds no application code.
- Data sensitivity/PII: none: the repository holds documentation and configuration only, with no application data.

## Team

Who works on this project. Claude reads this to label the EOD status report in `docs/eod/`, per `.claude/rules/eod-reporting.md`. Ask for it during intake and keep it current; a developer who is not listed here cannot be logged.

Role codes: `TL` Technical Lead, `PL` Project Lead, `LD` Lead Developer, `TD` Tech Developer, `TA` Tech Associate.

| Name | Email | Role | Work start | Work end | Timezone | From | To |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Mehedi Hassan Nayeem | github.mehedihassan.0955.0602@gmail.com | LD | 07:00 | 15:00 | GMT+6 | 23 July 2026 | |

- Name is what the developer types when Claude asks whose session this is, so use the name they will actually give.
- Work start, work end, and timezone are the developer's declared daily working window, not a measurement of any day. Claude never records hours worked, session counts, or activity.
- From and To are the developer's period on this project. Leave To empty while they are still on it, and fill it the day they leave or move to another project.
- A developer gets a section in a day's EOD report only when that date falls inside their From and To window, so someone who left in July stops appearing in August while every report they were part of stays exactly as it was.
- Row order is section order in every daily EOD report. Keep it stable.

## Current Sprint Tasks

No sprint tasks are tracked in this repository yet.

## Scope And Non-Goals

- Declared non-goals / out-of-scope feature classes: application code of any kind. This repository carries the kit only.
- Deferred work pushed to a later phase: none recorded in the repository.

## Orientation Map

The one-page snapshot Claude keeps for whoever owns this project, per `.claude/rules/orientation-map.md`. It answers Position, Surface, and Open, is rebuilt from the repository when a trigger fires, and holds no fact of its own. Ask for these during intake; the page is not built until the path and its reader are confirmed.

- Map file path: `docs/orientation-map.html` (the kit default; no map has been built in this repository)
- Who the map is written for (the owner, by role): not yet confirmed; no orientation map has been set up for this repository.
- What counts as a milestone being formally accepted here: not recorded in the repository.
- Where the project's status file, active plan, and decision records live: none exist in the repository; its history is the git log and `.claude/CHANGELOG.md`.
- Directories that make up the configuration surface: `.claude/` (rules, commands, agents, skills, scripts, settings) and the root `CLAUDE.md`.
- Regeneration triggers beyond the rule's defaults: none
- Design identity the page follows: the approved `ui-ux-design` tokens, standalone rather than in the app shell

## System Boundaries

- What the system does: provides Claude Code instructions (the `CLAUDE.md` loader, rules, slash commands, subagents, skills, settings) and human guides (`developer-handbook/`) for copying into other repositories.
- What the system does NOT do: run, build, or deploy anything; it holds no application code.
- External integration seams: none. The kit is consumed by copying its files into another repository.
- Module / layer map: `CLAUDE.md` (entry point and loader), `.claude/rules/` (rules), `.claude/commands/` (slash commands), `.claude/agents/` (subagents), `.claude/skills/` (ui-ux-design, ai-model-integration, semantic-data-model), `.claude/scripts/` (override check), `developer-handbook/` (guides, prompts, reference, layout template).
- Allowed dependency direction: `CLAUDE.md` points into `.claude/`; `developer-handbook/` is reference for humans and is not read by Claude at startup.

## Stack And Dependency Decisions

- Framework & stack choice rationale: not applicable: this repository holds no application code.
- Approved libraries: not applicable: this repository holds no application code.
- Banned / disallowed libraries: not applicable: this repository holds no application code.
- Formatter / linter configuration: none: the repository has no formatter or linter configuration.
- Dependency version-pinning / lockfile policy: not applicable: the repository has no dependencies.
- Data-access layer / pattern: not applicable: this repository holds no application code.

## UI Application Definition

### Approved standard values

- Company name: CLaaS2SaaS
- Company logos, per theme: `logo-full-light.png` / `logo-full-dark.png` (expanded), `logo-short-light.png` / `logo-short-dark.png` (collapsed), in `.claude/skills/ui-ux-design/assets/`
- Favicon, per theme: `favicon-light.ico` / `favicon-dark.ico`
- Design tokens, palette, surfaces: `.claude/skills/ui-ux-design/reference/design-tokens.md`; brand palette Midnight Blue `#193E6B`, Green Gold `#B3A125`, Avocado `#5F8025`, Sunray `#E9AC53`, Violet-Red `#991547`, Jelly Bean `#448E9D`, Cadmium Violet `#7F3F98`
- Fonts: Montserrat headings + Source Sans 3 body
- Density: compact
- Icon style: central inline-SVG registry (24px viewBox, 2px stroke, outline)
- Accessibility target: WCAG AA
- Theme switcher: on (System / Dark / Light, profile menu Appearance section, persisted)
- Sidebar nav filter: on; Sidebar collapsible icon rail: on
- Navigation clusters (top-to-bottom): Workspace, Compliance, Application Administration, System Administration

### App identity (per project)

- App name: not applicable: this repository holds no application.
- Browser / document title-bar name: not applicable: this repository holds no application.
- Tagline: not applicable: this repository holds no application.
- Brand-assets destination path: not applicable: this repository holds no application. The kit's own copies sit in `.claude/skills/ui-ux-design/assets/` and `developer-handbook/layout-template/assets/images/`.

### UI stack (per project)

- UI stack: not applicable: this repository holds no application.
- Charting library: not applicable: this repository holds no application.

### Feature toggles (confirmed defaults)

- Customizable dashboard: not applicable: this repository holds no application.
- Notifications: not applicable: this repository holds no application.
- Authentication: not applicable: this repository holds no application.
- Recycle bin (soft-delete + restore): not applicable: this repository holds no application.
- Audit log: not applicable: this repository holds no application.

### List behavior (per project)

Every list / index screen sorts and filters, per `.claude/rules/ui-ux-quality.md`. What varies is the naming.

- List query parameter convention: not applicable: this repository holds no application.
- Server-side sort and filter for paginated lists: not applicable: this repository holds no application.

### Step-by-step form drafts (per project)

Every multi-step form saves a resumable draft on each `Continue`, per `.claude/rules/ui-ux-quality.md`. What varies is where it is stored, not whether it exists.

- Draft storage shape (a separate draft table, or the record's own table with a draft state): not applicable: this repository holds no application.
- Where the draft state / status value comes from (the platform's existing status dimension, or a new one): not applicable: this repository holds no application.
- Drafts in flight per user per flow (one resumable draft, or several): not applicable: this repository holds no application.
- Fields excluded from a saved draft (credentials, keys, tokens, passwords): not applicable: this repository holds no application.
- Autosave beyond the per-step save (interval, on blur, on close): not applicable: this repository holds no application.

### Navigation tree (config-driven, sidebar-only)

- Workspace: not applicable: this repository holds no application.
- Compliance: not applicable: this repository holds no application.
- Application Administration: not applicable: this repository holds no application.
- System Administration: not applicable: this repository holds no application.
- Access policies: not applicable: this repository holds no application.

### Entities

No entities: this repository holds no application code and no database tables.

### Roles & tenancy

Not applicable: this repository holds no application, so it has no roles, record scopes, cluster grants, tenancy, or org hierarchy. The standard role model an application follows is defined in the Role And Access Model section of `.claude/rules/ui-ux-quality.md`.

## Authentication And Authorization

Not applicable: this repository holds no application, so nothing here signs users in or authorizes them. Never record a secret here.

### Session And Sign-In

- Local session technology: not applicable: this repository holds no application.
- Local session lifetime and expiry behavior: not applicable: this repository holds no application.
- Where a sign-in attempt's outcome is recorded: not applicable: this repository holds no application.
- The closed set of sign-in outcome values, and where it is codified: not applicable: this repository holds no application.
- Retention and classification of the sign-in outcome record: not applicable: this repository holds no application.
- Where a failed sign-in lands: not applicable: this repository holds no application.
- Local session identifier reissued when the session is created: not applicable: this repository holds no application.

### Authorization (This Application's Own)

- Feature registry storage location: not applicable: this repository holds no application.
- Roles, grants, and assignment storage location: not applicable: this repository holds no application.
- Shared action vocabulary: not applicable: this repository holds no application.
- Default role provisioned on first login: not applicable: this repository holds no application.
- Where the configured default role does not resolve: not applicable: this repository holds no application.
- Where the default is no access, the page a new user lands on: not applicable: this repository holds no application.
- How the framework's own authorization helpers are handled: not applicable: this repository holds no application.
- How the tenant is resolved where the app is multi-tenant: not applicable: this repository holds no application.
- Tier that manages roles, grants, and user assignment: not applicable: this repository holds no application.
- Tier that manages the feature registry: not applicable: this repository holds no application.
- Grant-change audit location (part of the security audit trail): not applicable: this repository holds no application.
- Grant ceiling: not applicable: this repository holds no application.
- A role or grant above the acting administrator's authority: not applicable: this repository holds no application.
- Every active, granted feature resolves to a real route before it renders: not applicable: this repository holds no application.
- Grant resolution: not applicable: this repository holds no application.
- Documented exceptions to the feature-authorization rules: none

## Security And Identity Conventions

- Secret manager / secret store: none: the repository holds no secrets and no `.env` or secret configuration.
- CI credential model: not applicable: the repository has no CI configuration.
- Token signing scheme: not applicable: this repository holds no application code.
- Authorization scope convention: not applicable: this repository holds no application code.
- Password hashing algorithm: not applicable: this repository holds no application code.
- Token / session lifetime policy: not applicable: this repository holds no application code.
- Encryption in transit policy: not applicable: this repository holds no application code.
- Encryption at rest policy: not applicable: this repository holds no application code.
- Key / secret rotation schedule: not applicable: the repository holds no keys or secrets.
- Field / column-level encryption targets: not applicable: the repository holds no database.
- AI/LLM feature conventions: not applicable: nothing in this repository calls a model at runtime.
  - Prompt store location + versioning: not applicable: no runtime prompts. The human prompt templates in `developer-handbook/prompts/` are versioned with the repository in git.
  - Model routing / adapter + fallback: not applicable: no runtime model calls. Developer-tooling model choice is described in `developer-handbook/guidelines/MODEL-ROUTING.md`.
  - Max-tokens / temperature defaults: not applicable: no runtime model calls.
  - Per-agent token budget + cost ceiling: not applicable: no runtime model calls.
  - Golden-set location + pass-score bar: not applicable: no runtime model calls.
  - Eval-as-merge-gate expectation: not applicable: no runtime model calls.

## Secure Coding Conventions

The facts `.claude/rules/secure-coding.md` depends on. Mechanisms and names only, never a secret.

- Templating engine and its automatic output escaping: not applicable: this repository holds no application code.
- HTML sanitizer for rich content (none means rich content renders as text): not applicable: this repository holds no application code.
- CSRF protection mechanism: not applicable: this repository holds no application code.
- Standing CSRF exemptions: not applicable: this repository holds no application code.
- Session cookie attributes: not applicable: this repository holds no application code.
- Content security policy and security headers: not applicable: this repository holds no application code.
- Origins allowed to read responses cross-origin (none means same origin only): not applicable: this repository holds no application code.
- Does the server fetch URLs a user or administrator supplies: no: there is no server in this repository.
  - Internal destinations it may reach, and the environment configuration key that names them: none
- Does the application accept file uploads: no: there is no application in this repository.
  - Allowed types and maximum size: not applicable: no uploads.
  - Storage location and how files are served: not applicable: no uploads.
  - Malware scanner: not applicable: no uploads.
- Production error display: not applicable: this repository holds no application code.
- Security audit trail location: not applicable: this repository holds no application code.
- How the audit trail is kept append-only: not applicable: this repository holds no application code.
- Audited actions beyond the rule's defaults, including any record whose reads are audited: none
- Dependency vulnerability audit tool and command: not applicable: the repository has no dependencies.
- Advisory that blocks adding or updating a dependency: not applicable: the repository has no dependencies.
- Compliance frameworks the organization pursues, any recorded mapping of controls to their criteria, and who owns the evidence: not recorded in the repository.
- Documented exceptions to the secure coding and dependency vulnerability rules: none

## AI Model Catalog (Only If The App Calls A Model At Runtime)

- Does the application call an AI model / LLM at runtime: no: this repository holds no application code.
- Catalog storage location (database table, versioned config file, or settings provider): not applicable: no runtime model calls.
- Providers and model ids in use: not applicable: no runtime model calls.
- Environment source `{{env.NAME}}` resolves against: not applicable: no runtime model calls.
- Project-wide per-call timeout and connect timeout: not applicable: no runtime model calls.
- Retry policy and dedupe-key strategy for model calls: not applicable: no runtime model calls.
- Cost currency (one project-wide constant) and display rounding precision: not applicable: no runtime model calls.
- Who may manage the catalog (role, defaults to System Administrator): not applicable: no runtime model calls.
- Documented exceptions to the approved catalog pattern: none

## Migrations And Data Change Workflow

- File-based migration tool: none: the repository has no database and no migrations.
- Migration file naming convention: not applicable: the repository has no migrations.

## Analytics And Semantic Model

- Does the solution feed a semantic model, BI layer, or reporting product: no: this repository holds no application or data.
- Semantic model / BI tool and who owns it: not applicable: no analytics in this repository.
- How reporting reads the data (operational tables, read replica, extract, warehouse): not applicable: no data in this repository.
- Reporting time zone and business-day boundary: not applicable: no analytics in this repository.
- First day of week and fiscal year start: not applicable: no analytics in this repository.
- Timestamp storage convention: not applicable: no data in this repository.
- Surrogate / business key convention: not applicable: no data in this repository.
- Conformed dimensions the platform already has (source, status, reason, owner, currency): not applicable: no data in this repository.
- History / event / snapshot retention: not applicable: no data in this repository.
- Where each feature's analytics question set is recorded: not applicable: no analytics in this repository.
- ERD and data-model deliverable location and format: not applicable: no data model in this repository.
- Approved materialized aggregates (and how each is rebuilt): none
- Documented exceptions to the analytics-ready data rules: none

## Quality, Observability, And Operability

- Minimum test coverage bar: not applicable: the repository has no code to test.
- Gating mechanism for incomplete work in the live path: not applicable: nothing in this repository runs live.
- Structured logging format: not applicable: nothing in this repository runs.
- Metrics / tracing destination: not applicable: nothing in this repository runs.
- Health / readiness endpoint conventions: not applicable: nothing in this repository runs.
- Per-environment config keys that differ across environments: none: the repository has no environments.

## Resilience Thresholds

- Outbound call timeout(s) per dependency type: not applicable: this repository makes no outbound calls.
- Retry policy for idempotent operations: not applicable: this repository makes no outbound calls.
- Circuit-breaker trip / reset thresholds: not applicable: this repository makes no outbound calls.
- Dead-letter / replay location for failed async work: not applicable: no async work in this repository.
- Idempotency / dedupe key strategy: not applicable: this repository holds no application code.

## Data Lifecycle Governance

- Data sensitivity classification scheme: none: the repository holds no application data.
- Retention period per data class: not applicable: the repository holds no application data.
- Audit / compliance log retention: not applicable: the repository holds no application data.
- Abandoned step-by-step form draft retention: not applicable: the repository holds no application.
- Applicable privacy regime: not applicable: the repository holds no personal data.
- Backup retention window: not applicable: the repository holds no application data; its history is kept by git and the remote.
- Restore-test cadence: not applicable: the repository holds no application data.
- Recovery-point objective (RPO) per environment: not applicable: the repository has no environments.
- Recovery-time objective (RTO) per environment: not applicable: the repository has no environments.
- Failover procedure and disaster communications: not applicable: the repository has no running service.

## Operations And Incident Response

- Service-level objectives (SLOs) per critical service: not applicable: the repository has no running service.
- Alert thresholds: not applicable: the repository has no running service.
- Incident severity taxonomy and per-severity response time: not applicable: the repository has no running service.
- On-call / escalation path: not applicable: the repository has no running service.
- Top failure modes and their runbook locations: not applicable: the repository has no running service.

## API / Interface Conventions

- Interface paradigm: not applicable: the repository exposes no API.
- Resource / operation naming style: not applicable: the repository exposes no API.
- Error contract format: not applicable: the repository exposes no API.
- Version scheme: not applicable: the repository exposes no API. The kit itself is versioned as `vYYYY.N.N`, recorded in `.claude/CHANGELOG.md`.
- Standard header names: not applicable: the repository exposes no API.
- Pagination style and maximum page-size cap: not applicable: the repository exposes no API.
- Authoritative contract spec tool and location: not applicable: the repository exposes no API.

## Configuration Contract

- Config example-file path: none: the repository has no `.env.example` or other application configuration.
- Config library / loader: not applicable: no application configuration.
- Config precedence order: Claude Code reads `.claude/settings.json` (committed) and `.claude/settings.local.json` (local, gitignored); no application configuration exists.
- Enumerated config variables (names + placeholders only): none.

## Git And Review Knobs

- Commit signing policy: not recorded in the repository; its one commit is unsigned.
- Merge strategy: not recorded in the repository.
- Working-branch lifetime cap: none set
- Rebase cadence: none set
- Required approver count: not recorded in the repository.
- CODEOWNERS / path-ownership file location: none
- Review first-response SLA window: none set

## Deployment Policy

- Deployment target: none: the repository is not deployed. Its contents are copied into other repositories.
- Deployment method: not applicable: nothing is deployed.
- Source control provider: Git, with branches `main` and `chore/claude-gateway-kit`.
- Repository remote: `origin` is `git@gp:eduCLaaSTeach/claude-kit.git`; `gp` is an SSH host alias defined outside the repository, so the hosting provider's web URL is not recorded here.
- CI/CD or deployment pipeline: none: the repository has no pipeline configuration.
- Hosting provider/platform: not applicable: nothing is hosted.
- Runtime/build/deploy model: not applicable: nothing is built or deployed.
- Infrastructure-as-code tool and location: none.
- Local development support: none needed: the repository is edited as plain files.
- Environment site URLs: none: the repository has no DEV, QA, STAG, or PROD site.
- Production deployment action: Claude must not deploy, migrate, publish, upload, change hosting settings, or edit production environment values without explicit developer approval for that exact action.

## Required Deployment Details To Ask For

- Hosting target/provider: not applicable: nothing is deployed.
- Source control and pipeline: Git, remote `origin` as above; no pipeline.
- Hosting account/project/resource placeholder: not applicable: nothing is hosted.
- Production URL and subdirectory: not applicable: there is no production site.
- Web root, artifact path, startup command, service entry point, or container image: not applicable: nothing runs.
- Runtime/platform version: not applicable: nothing runs.
- Package manager/build availability on target: not applicable: nothing is built.
- Background processing/scheduler support: not applicable: nothing runs.
- Deployment workflow owner and steps: not applicable: nothing is deployed.
- Rollback and monitoring/log access: not applicable: nothing is deployed.
- Required `.env` keys using placeholders only: none.

## Required Validation Details To Ask For

- Syntax/type/static checks: none configured. The JSON settings files can be checked by parsing them, for example with `node -e "JSON.parse(require('fs').readFileSync('.claude/settings.json','utf8'))"`.
- Lint/format commands: none configured.
- Unit/integration/e2e commands: none: the repository has no tests.
- Build/package commands: none: nothing is built.
- Security checks (dependency audit, static security analysis, secret scan) and where they run: none configured.
- Deployment smoke test: not applicable: nothing is deployed.
- Known validation limitations: the repository has no automated checks; review is manual.

## Database Schema (Source Of Truth)

- Database/schema source of truth, per `.claude/rules/database-schema.md`: not applicable: the repository has no database and no schema.
- SQL Server schema name: not applicable: no database.
- Table name prefix convention: not applicable: no database.
- Table/column naming casing convention: not applicable: no database.
- Resulting table name pattern: not applicable: no database.
- Tables/schemas relevant to the current task: none: no database.

## Application Database Connection (Only If The App Connects To A Database Directly)

- Does the application connect to a database directly: no: this repository holds no application code.
- Database engine the application connects to: not applicable: no database connection.
- Database host/service placeholder: not applicable: no database connection.
- Database name placeholder: not applicable: no database connection.
- Database user placeholder: not applicable: no database connection.
- Schema/database owner: not applicable: no database connection.
- Where connection settings/secrets live (env keys, placeholders only): not applicable: no database connection.

## Knowledge Base Policy

- Claude must read existing knowledge-base files before editing when they exist.
- Claude must read this file before editing and update it with confirmed non-secret facts after developer answers.
- After each completed implementation, Claude must ask the developer whether to create or update the knowledge base now or defer it, and must write knowledge-base files only after the developer has verified, validated, and explicitly approved the update, per `.claude/rules/knowledge-base.md`.
- When approved, Claude must ask whether the work is a solution, a module, or a feature and where it sits in the hierarchy, then update the write-up and the knowledge-base README index.
- Claude must update the table dictionary after confirmed work that verifies or changes table/schema knowledge; when a change alters the schema, the table-dictionary update travels in the same change unit as the schema change.
