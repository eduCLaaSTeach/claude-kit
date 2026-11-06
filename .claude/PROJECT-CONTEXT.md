# Project Context

Durable, non-secret project facts Claude reads before making changes. Field meanings, examples, and who answers each live in `00-read-first/03-reference/PROJECT-CONTEXT-UNDERSTANDING.md`. Leave unknown fields as `<ASK_DEVELOPER>`; never guess.

Never store secrets here. Use placeholders such as `<HOSTING_RESOURCE>`, `<DATABASE_NAME>`, `<DATABASE_USER>`, and `<CLICKUP_URL>`.

## Required Intake Status

- Solution: `<ASK_DEVELOPER>`
- Module: `<ASK_DEVELOPER>`
- Sub-module: `<ASK_DEVELOPER>` (or `none`)
- ClickUp URL: [ClickUp Workspace](<CLICKUP_URL>)
- Business purpose: `<ASK_DEVELOPER>`
- Primary users: `<ASK_DEVELOPER>`
- Application type: `<ASK_DEVELOPER>`
- Backend stack and version: none in this repository yet (it holds only the Claude Code gateway kit files, no application code, `composer.json` or `package.json`); `<ASK_DEVELOPER>` for the planned stack
- Frontend stack and version: none in this repository yet (no application code or `package.json`); `<ASK_DEVELOPER>` for the planned stack
- Database engine and version: `<ASK_DEVELOPER>`
- Package manager/runtime versions: `<ASK_DEVELOPER>`
- Authentication model (how users sign in; this application provides authorization per feature either way): `<ASK_DEVELOPER>`
- Data sensitivity/PII: `<ASK_DEVELOPER>`

## Team

Who works on this project. Claude reads this to label the EOD status report in `doc/eod/`, per `.claude/rules/eod-reporting.md`. Ask for it during intake and keep it current; a developer who is not listed here cannot be logged.

Role codes: `TL` Technical Lead, `PL` Project Lead, `LD` Lead Developer, `TD` Tech Developer, `TA` Tech Associate.

| Name | Email | Role | Work start | Work end | Timezone | From | To |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `<ASK_DEVELOPER>` | `<ASK_DEVELOPER>` | `<TL/PL/LD/TD/TA>` | `<09:00>` | `<18:00>` | `<GMT+6>` | `<17 August 2026>` | |

- Name is what the developer types when Claude asks whose session this is, so use the name they will actually give.
- Work start, work end, and timezone are the developer's declared daily working window, not a measurement of any day. Claude never records hours worked, session counts, or activity.
- From and To are the developer's period on this project. Leave To empty while they are still on it, and fill it the day they leave or move to another project.
- A developer gets a section in a day's EOD report only when that date falls inside their From and To window, so someone who left in July stops appearing in August while every report they were part of stays exactly as it was.
- Row order is section order in every daily EOD report. Keep it stable.

## Solution Planning And Task Tracking

Requirements, the phase plan, task status, and the persistent core repo live under `doc/`, per `.claude/rules/solution-development-kit.md`. This section points at them and records the two planning approvals. Task status is never kept here; `doc/tasktracking/` is the one place it lives.

- Requirement source: `doc/solutiondocument/`, read recursively; any folder named `Archive` is ignored unless an active source references it or a human re-approves it
- Phase plan location: `doc/phasedocument/` (`MASTER_PHASE_PLAN.md`, plus one file per phase in `phases/`, at most five)
- Standards baseline the plan was built against: `doc/phasedocument/PROJECT_STANDARDS_BASELINE.md`
- `doc/phasedocument/02_Gap_Conflict_Report.md` approval (who approved it and when; not approved until a human says so explicitly): not approved
- `doc/phasedocument/MASTER_PHASE_PLAN.md` approval (who approved it and when; not approved until a human says so explicitly): not approved
- Task source of truth: `doc/tasktracking/TASK_REGISTER.md` for every task and its status, `doc/tasktracking/TASK_BOARD.md` for progress, `doc/tasktracking/TRACEABILITY_MATRIX.md` for requirement to task traceability
- Task event log: `doc/tasktracking/task-events.jsonl`, append-only, written through `scripts/task-event.sh`
- Per-task data design evidence: `doc/tasktracking/data-design/`
- Persistent core repo: `doc/corerepo/`, the compact living project memory, maintained by `core-repo-maintainer` on accepted or verified state changes; it maps to the requirements and snapshots task status, and owns neither
- Infrastructure readiness record: `doc/corerepo/02_INFRA_READINESS.md`, maintained by `infra-readiness-agent` in Phase 1 and whenever infrastructure materially changes; never READY without evidence

## Scope And Non-Goals

- Declared non-goals / out-of-scope feature classes: `<ASK_DEVELOPER>`
- Deferred work pushed to a later phase: `<ASK_DEVELOPER>`

## Orientation Map

The one-page snapshot Claude keeps for whoever owns this project, per `.claude/rules/orientation-map.md`. It answers Position, Surface, and Open, is rebuilt from the repository when a trigger fires, and holds no fact of its own. Ask for these during intake; the page is not built until the path and its reader are confirmed.

- Map file path: `doc/orientation-map.html`
- Who the map is written for (the owner, by role): `<ASK_DEVELOPER>`
- What counts as a milestone being formally accepted here: `<ASK_DEVELOPER>`
- Where the project's decision records live (the plan is in `doc/phasedocument/` and task status in `doc/tasktracking/`): `<ASK_DEVELOPER>`
- Directories that make up the configuration surface: `<ASK_DEVELOPER>`
- Regeneration triggers beyond the rule's defaults: none
- Design identity the page follows: the approved `ui-ux-design` tokens, standalone rather than in the app shell

## System Boundaries

- What the system does: `<ASK_DEVELOPER>`
- What the system does NOT do: `<ASK_DEVELOPER>`
- External integration seams: `<ASK_DEVELOPER>`
- Module / layer map: `<ASK_DEVELOPER>`
- Allowed dependency direction: `<ASK_DEVELOPER>`

## Stack And Dependency Decisions

- Framework & stack choice rationale: `<ASK_DEVELOPER>`
- Approved libraries: `<ASK_DEVELOPER>`
- Banned / disallowed libraries: `<ASK_DEVELOPER>`
- Formatter / linter configuration: `<ASK_DEVELOPER>`
- Dependency version-pinning / lockfile policy: `<ASK_DEVELOPER>`
- Data-access layer / pattern: `<ASK_DEVELOPER>`

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

- App name: `<ASK_DEVELOPER>`
- Browser / document title-bar name: `<ASK_DEVELOPER>`
- App icon: `<ASK_DEVELOPER>` (the icon file of the most specific node the app is, as the catalog's `files.icon` value inside `.claude/skills/ui-ux-design/assets/solution-icons/`, for example `agentic-crm/agentic-leads-generator.svg` or `eos/integration-hub/icon.svg`: its Feature, else its Sub-module, else its Module, else its Solution; the nearest ancestor's icon when its node is not in the catalog; else `none`; asked, never guessed or borrowed - see `.claude/skills/ui-ux-design/reference/solution-icons.md`)
- Tagline: `<ASK_DEVELOPER>` `[optional]`
- Brand-assets destination path: `<BRAND_ASSETS_PATH>` = `<ASK_DEVELOPER>`

### UI stack (per project)

- UI stack: `<ASK_DEVELOPER>`
- Charting library: `<ASK_DEVELOPER>`

### Feature toggles (confirmed defaults)

- Customizable dashboard: On
- Notifications: On (in-app, top-bar bell)
- Authentication: On - per the authentication model recorded above; this application owns every role, permission, and feature grant. See the Authentication And Authorization section below
- Recycle bin (soft-delete + restore): On
- Audit log: On

### List behavior (per project)

Every list / index screen sorts and filters, per `.claude/rules/ui-ux-quality.md`. What varies is the naming.

- List query parameter convention: `q`, `sort`, `dir`, `page`, `size`, plus one per facet - replace only if the project already has a convention
- Server-side sort and filter for paginated lists: required
- Page sizes: 5, 10, 20, 30, 50, 70, 100, default 10, in a native dropdown at every breakpoint - fixed by the UI standard, not per project

### Step-by-step form drafts (per project)

Every multi-step form saves a resumable draft on each `Continue`, per `.claude/rules/ui-ux-quality.md`. What varies is where it is stored, not whether it exists.

- Draft storage shape (a separate draft table, or the record's own table with a draft state): `<ASK_DEVELOPER>`
- Where the draft state / status value comes from (the platform's existing status dimension, or a new one): `<ASK_DEVELOPER>`
- Drafts in flight per user per flow (one resumable draft, or several): one
- Fields excluded from a saved draft (credentials, keys, tokens, passwords): all of them, always
- Autosave beyond the per-step save (interval, on blur, on close): `<ASK_DEVELOPER>`

### Navigation tree (config-driven, sidebar-only)

- Workspace: `<ASK_DEVELOPER>`
- Compliance: `<ASK_DEVELOPER>`
- Application Administration: `<ASK_DEVELOPER>`
- System Administration: `<ASK_DEVELOPER>`
- Access policies: `<ASK_DEVELOPER>`

### Entities

#### Entity: `<ASK_DEVELOPER>` (plural: `<ASK_DEVELOPER>`)

- Database table(s): `<ASK_DEVELOPER>`
- Columns: `<ASK_DEVELOPER>`
- Transaction / child table(s): `<ASK_DEVELOPER>`
- Field -> column mapping notes: `<ASK_DEVELOPER>`
- Default list sort (column + direction): `<ASK_DEVELOPER>`
- List filter facets (the dimensions this list is narrowed by): `<ASK_DEVELOPER>`

### Roles & tenancy

| Role | UI-standard tier | Record scope |
| --- | --- | --- |
| System Administrator | system admin | ALL records, plus platform and integration configuration; the only role that reaches System Administration |
| Administrator | admin | ALL records; the role that permanently deletes application records |
| Collaborator | team/collaborator | OWN records plus records in their Business Unit |
| Contributor | self/contributor | OWN or directly assigned records only |
| Viewer | self-view/read-only | Read-only; scope OWN, TEAM, or Business Unit per app: `<ASK_DEVELOPER>` |
| Custom | per app, optional | Define scope and cluster grants before use: `<ASK_DEVELOPER>` |

Cluster grants per role:

| Cluster | Roles granted |
| --- | --- |
| Workspace | Viewer, Contributor, Collaborator, Administrator, System Administrator |
| Compliance | Collaborator, Administrator, System Administrator |
| Application Administration | Administrator, System Administrator |
| System Administration | System Administrator only |

- Roles are named sets of feature grants, not hardcoded cluster lists. The tier decides record scope and the cluster defaults above; the grant decides whether a user reaches an individual feature at all. The cluster grants seed the default roles' grants rather than acting as a second enforcement mechanism beside them. See the Role And Access Model section of `.claude/rules/ui-ux-quality.md`.
- Tenancy: single-tenant
- Team/org hierarchy: Administrator (org root) -> Business Unit -> Team. System Administrator sits above the org root and owns platform configuration rather than records.

## Authentication And Authorization

Authentication follows the authentication model recorded under Required Intake Status above. This application owns every role, permission, and feature grant. Never record a secret here.

### Session And Sign-In

- Local session technology: `<ASK_DEVELOPER>`
- Local session lifetime and expiry behavior: `<ASK_DEVELOPER>`
- Where a sign-in attempt's outcome is recorded (a durable record in the security audit trail; a structured log stream may mirror it, never replace it): `<ASK_DEVELOPER>`
- The closed set of sign-in outcome values, and where it is codified: `<ASK_DEVELOPER>`
- Retention and classification of the sign-in outcome record (the audit retention unless a different one is confirmed): `<ASK_DEVELOPER>`
- Where a failed sign-in lands: `<ASK_DEVELOPER>`
- Local session identifier reissued when the session is created: yes - a value the browser held before sign-in must not survive it

### Authorization (This Application's Own)

- Feature registry storage location: `<ASK_DEVELOPER>`
- Roles, grants, and assignment storage location: `<ASK_DEVELOPER>`
- Shared action vocabulary: `view`, `create`, `edit`, `delete`, `restore`, `purge`, `export`, `approve`, `configure` - confirm and adjust per app, and never invent a per-feature vocabulary
- Default role provisioned on first login: `<ASK_DEVELOPER>`
- Where the configured default role does not resolve: provision the user with no role, record it in the audit row, and verify the role exists at migration or seed time - never refuse the sign-in and never fall back to another role
- Where the default is no access, the page a new user lands on: `<ASK_DEVELOPER>`
- How the framework's own authorization helpers are handled (bridged to the application's identity, or bypassed for explicit policy calls - never left half-wired): `<ASK_DEVELOPER>`
- How the tenant is resolved where the app is multi-tenant (from the application's own store, keyed on the user's id): `<ASK_DEVELOPER>`
- Tier that manages roles, grants, and user assignment: admin and above
- Tier that manages the feature registry: system admin only
- Grant-change audit location (part of the security audit trail): `<ASK_DEVELOPER>`
- Grant ceiling: a role is assigned only by an actor whose own tier is at least as high as that role's tier, and a `(feature, action)` grant is added or removed only by an actor holding that same grant - on their own record exactly as on anybody else's
- A role or grant above the acting administrator's authority: rendered locked and merged back in on write, never filtered out of the form and never stripped by the save
- Every active, granted feature resolves to a real route before it renders, so a cluster heading never appears above nothing
- Grant resolution: resolved once per request, invalidated on any grant or assignment change
- Documented exceptions to the feature-authorization rules: none

## Security And Identity Conventions

- Secret manager / secret store: `<ASK_DEVELOPER>`
- CI credential model: `<ASK_DEVELOPER>`
- Token signing scheme: `<ASK_DEVELOPER>`
- Authorization scope convention: `<ASK_DEVELOPER>`
- Password hashing algorithm: `<ASK_DEVELOPER>`
- Token / session lifetime policy: `<ASK_DEVELOPER>`
- Encryption in transit policy: `<ASK_DEVELOPER>`
- Encryption at rest policy: `<ASK_DEVELOPER>`
- Key / secret rotation schedule: `<ASK_DEVELOPER>`
- Field / column-level encryption targets: `<ASK_DEVELOPER>`
- AI/LLM feature conventions: `<ASK_DEVELOPER>`
  - Prompt store location + versioning: `<ASK_DEVELOPER>`
  - Model routing: one engine with no fallback hop; the default route per step is recorded under AI Model Catalog below
  - Max-tokens / temperature defaults: `<ASK_DEVELOPER>`
  - Per-agent token budget + cost ceiling: `<ASK_DEVELOPER>`
  - Golden-set location + pass-score bar: `<ASK_DEVELOPER>`
  - Eval-as-merge-gate expectation: `<ASK_DEVELOPER>`

## Secure Coding Conventions

The facts `.claude/rules/secure-coding.md` depends on. Mechanisms and names only, never a secret.

- Templating engine and its automatic output escaping: `<ASK_DEVELOPER>`
- HTML sanitizer for rich content (none means rich content renders as text): `<ASK_DEVELOPER>`
- CSRF protection mechanism: `<ASK_DEVELOPER>`
- Standing CSRF exemptions: signature-verified inbound webhook routes; any other is listed under Documented exceptions below
- Session cookie attributes (`HttpOnly`, `Secure` over HTTPS, host-only, and `SameSite=Lax` unless a reason for `Strict` is recorded): `<ASK_DEVELOPER>`
- Content security policy and security headers: `<ASK_DEVELOPER>`
- Origins allowed to read responses cross-origin (none means same origin only): `<ASK_DEVELOPER>`
- Does the server fetch URLs a user or administrator supplies (webhooks, imports, AI model catalog endpoints, connection tests): `<ASK_DEVELOPER>`
  - Internal destinations it may reach, and the environment configuration key that names them: none
- Does the application accept file uploads: `<ASK_DEVELOPER>`
  - Allowed types and maximum size: `<ASK_DEVELOPER>`
  - Storage location and how files are served: `<ASK_DEVELOPER>`
  - Malware scanner: `<ASK_DEVELOPER>`
- Production error display: debug output off; an unexpected error shows a generic message and a correlation id, with detail in the log only
- Security audit trail location (the sign-in outcome record and the grant-change audit are part of it): `<ASK_DEVELOPER>`
- How the audit trail is kept append-only (a grant of insert and select with no update or delete on the store, or by convention only until that grant is confirmed): `<ASK_DEVELOPER>`
- Audited actions beyond the rule's defaults, including any record whose reads are audited: none
- Dependency vulnerability audit tool and command: `<ASK_DEVELOPER>`
- Advisory that blocks adding or updating a dependency: critical, high, or unrated; take the fixed version where one exists, otherwise only with the developer's explicit acceptance recorded under Documented exceptions below
- Compliance frameworks the organization pursues, any recorded mapping of controls to their criteria, and who owns the evidence (code supports a control; it never makes the application compliant): `<ASK_DEVELOPER>`
- Documented exceptions to the secure coding and dependency vulnerability rules (a CSRF exemption, a loosened content security policy, an accepted advisory, an audited action shipped before the trail exists), each with who accepted it and when, the reason, the follow-up and its owner, and when it is revisited: none

## AI Model Catalog (Only If The App Calls A Model At Runtime)

Confirmed defaults, allowed ranges, and allowed shapes. The live values an administrator changes on the AI screens, such as a step's current route, are data in the application and are never copied back here.

- Does the application call an AI model / LLM at runtime: `<ASK_DEVELOPER>`
- Catalog storage location (database table, versioned config file, or settings provider): `<ASK_DEVELOPER>`
- Providers and model ids in use: `<ASK_DEVELOPER>`
- Environment source `{{env.NAME}}` resolves against: `<ASK_DEVELOPER>`
- Project-wide per-call timeout and connect timeout: `<ASK_DEVELOPER>`
- Retry policy and dedupe-key strategy for model calls: `<ASK_DEVELOPER>`
- Cost currency (one project-wide constant) and display rounding precision: `<ASK_DEVELOPER>`
- Who may manage the catalog (role, defaults to System Administrator): `<ASK_DEVELOPER>`
- Default route and allowed records per step (the default route shape, and the catalog records each role may use): `<ASK_DEVELOPER>`
- Where runtime route changes read their eval result (pass or fail, with its timestamp, per step, catalog record, and prompt version): `<ASK_DEVELOPER>`
- Multi-call patterns allowed (dual drafting, cross critique, judge mode patch or rewrite and its edit cap, pre-drafting): `<ASK_DEVELOPER>`
- Repair retry allowed, and how many (default one): `<ASK_DEVELOPER>`
- Model retirement alert lead time: `<ASK_DEVELOPER>`
- Stream keep-alive interval for long AI work: `<ASK_DEVELOPER>`
- Grounding verification level per feature (citation and quote check, stronger checks, or human review): `<ASK_DEVELOPER>`
- Eval attack corpus location and red-team cadence: `<ASK_DEVELOPER>`
- Owner of the AI feedback review queue (holds a tier that reaches Application Administration, where the queue sits in the `AI Oversight` group): `<ASK_DEVELOPER>`
- AI inventory location (default `doc/corerepo/09_AI_COMPONENT_REGISTER.md`): `<ASK_DEVELOPER>`
- AI risk-level scale used in the inventory: `<ASK_DEVELOPER>`
- Documented exceptions to the approved catalog pattern: none

## AI Guardrails (Only If The App Calls A Model At Runtime)

Confirmed defaults, allowed ranges, and allowed shapes. The live values an administrator changes on the guardrail screens, the word lists among them, are data in the application and are never copied back here.

- Required layers, rendered locked on (small-talk shortcut, length cap, rules layer, moderation, relevance, output check): `<ASK_DEVELOPER>`
- Optional layers an administrator may turn off: `<ASK_DEVELOPER>`
- Moderation and relevance checks, the default checker record, and the records allowed for each: `<ASK_DEVELOPER>`
- Strictness levels, and what each level sets: `<ASK_DEVELOPER>`
- Guardrail content category set (such as abusive, injection, off-topic, and spam), and the strike weight per category: `<ASK_DEVELOPER>`
- Fail mode per check, closed or open (a screening refusal always blocks, and injection screening for an agent with write tools always fails closed): `<ASK_DEVELOPER>`
- Streaming moderation mode per feature (buffered or retractable): `<ASK_DEVELOPER>`
- Strike threshold, pause length, and whether the count resets when a pause ends (only content categories count; a kill switch, pause, limit, or length-cap refusal never does): `<ASK_DEVELOPER>`
- Limits (per-user rate, per-record cap, daily budget in calls, tokens, and cost, alert percent, workflow caps on steps, tool calls, output tokens, input size, and concurrent runs): `<ASK_DEVELOPER>`
- Quarantine of text blocked as abusive, with its access and retention: none
- Link allow-list for model output (destinations a model-generated link may point at; a link that traces to a source the application supplied needs no entry): `<ASK_DEVELOPER>`
- Who may add content to the retrieval store, and how an entry is removed and re-indexed: `<ASK_DEVELOPER>`
- Prompt-leak canary: none
- Kill switch owner (holds the tier that reaches the Kill Switches screen), and the non-AI fallback and message per feature: `<ASK_DEVELOPER>`
- AI disclosure and labelling requirement, and any regulatory marking obligation: `<ASK_DEVELOPER>`
- Who may manage guardrail settings (role, defaults to System Administrator): `<ASK_DEVELOPER>`
- Guardrail settings storage shape (typed values, or one row per settings area): `<ASK_DEVELOPER>`
- Guardrail metrics destination and annotate-mode policy (annotate applies only to a check added to an enforcing pipeline or a changed threshold, never to the baseline layers): `<ASK_DEVELOPER>`
- Documented exceptions to the guardrail rules: none

## Migrations And Data Change Workflow

- File-based migration tool: `<ASK_DEVELOPER>`
- Migration file naming convention: `<ASK_DEVELOPER>`

## Analytics And Semantic Model

- Does the solution feed a semantic model, BI layer, or reporting product: `<ASK_DEVELOPER>`
- Semantic model / BI tool and who owns it: `<ASK_DEVELOPER>`
- How reporting reads the data (operational tables, read replica, extract, warehouse): `<ASK_DEVELOPER>`
- Reporting time zone and business-day boundary: `<ASK_DEVELOPER>`
- First day of week and fiscal year start: `<ASK_DEVELOPER>`
- Timestamp storage convention: `<ASK_DEVELOPER>`
- Surrogate / business key convention: `<ASK_DEVELOPER>`
- Conformed dimensions the platform already has (source, status, reason, owner, currency): `<ASK_DEVELOPER>`
- History / event / snapshot retention: `<ASK_DEVELOPER>`
- Where each feature's analytics question set is recorded: `<ASK_DEVELOPER>`
- ERD and data-model deliverable location and format: `<ASK_DEVELOPER>`
- Approved materialized aggregates (and how each is rebuilt): none
- Documented exceptions to the analytics-ready data rules: none

## Quality, Observability, And Operability

- Minimum test coverage bar: `<ASK_DEVELOPER>`
- Gating mechanism for incomplete work in the live path: `<ASK_DEVELOPER>`
- Structured logging format: `<ASK_DEVELOPER>`
- Metrics / tracing destination: `<ASK_DEVELOPER>`
- Health / readiness endpoint conventions: `<ASK_DEVELOPER>`
- Per-environment config keys that differ across environments: `<ASK_DEVELOPER>`

## Resilience Thresholds

- Outbound call timeout(s) per dependency type: none yet
- Retry policy for idempotent operations: none yet
- Circuit-breaker trip / reset thresholds: none yet
- Dead-letter / replay location for failed async work: none yet
- Idempotency / dedupe key strategy: `Idempotency-Key` header documented, not yet enforced

## Data Lifecycle Governance

- Data sensitivity classification scheme: none
- Retention period per data class: 7 years
- Audit / compliance log retention: 7 years
- Abandoned step-by-step form draft retention: `<ASK_DEVELOPER>`
- Classification and retention of AI verdict, strike, pause, feedback, job, and pre-draft rows: `<ASK_DEVELOPER>`
- Applicable privacy regime: none
- Backup retention window: 7 years
- Restore-test cadence: none
- Recovery-point objective (RPO) per environment: none
- Recovery-time objective (RTO) per environment: none
- Failover procedure and disaster communications: none

## Operations And Incident Response

- Service-level objectives (SLOs) per critical service: none yet
- Alert thresholds: none yet
- Incident severity taxonomy and per-severity response time: none yet
- On-call / escalation path: none yet
- Top failure modes and their runbook locations: none yet
- AI incident severity mapping (prompt injection, data leaked through output, runaway cost, a provider incident or model retirement, each to a severity above): `<ASK_DEVELOPER>`

## API / Interface Conventions

- Interface paradigm: `<ASK_DEVELOPER>`
- Resource / operation naming style: `<ASK_DEVELOPER>`
- Error contract format: `<ASK_DEVELOPER>`
- Version scheme: `<ASK_DEVELOPER>`
- Standard header names: `<ASK_DEVELOPER>`
- Pagination style and maximum page-size cap: `<ASK_DEVELOPER>` (the UI standard fixes list page sizes at 5 to 100, so a cap on an interface that feeds a list allows 100)
- Authoritative contract spec tool and location: `<ASK_DEVELOPER>`

## Configuration Contract

- Config example-file path: `<ASK_DEVELOPER>`
- Config library / loader: `<ASK_DEVELOPER>`
- Config precedence order: `<ASK_DEVELOPER>`
- Enumerated config variables (names + placeholders only): `<ASK_DEVELOPER>`

## Git And Review Knobs

- Commit signing policy: not required
- Merge strategy: not fixed (chosen per Pull Request)
- Working-branch lifetime cap: none set
- Rebase cadence: none set
- Required approver count: one superior approval by convention (not branch-protected)
- CODEOWNERS / path-ownership file location: none
- Review first-response SLA window: none set

## Deployment Policy

- Deployment target: `<ASK_DEVELOPER>`
- Deployment method: `<ASK_DEVELOPER>`
- Source control provider: GitHub, repository `eduCLaaSTeach/claude-kit` (from the git remote `origin`)
- GitHub repository URL: [GitHub Repository](https://github.com/eduCLaaSTeach/claude-kit) (from the git remote `origin`)
- CI/CD or deployment pipeline: none in this repository (it has no `.github/workflows`)
- Hosting provider/platform: `<ASK_DEVELOPER>`
- Runtime/build/deploy model: `<ASK_DEVELOPER>`
- Infrastructure-as-code tool and location: `<ASK_DEVELOPER>`
- Local development support: `<ASK_DEVELOPER>`
- Environment site URLs:
  - `DEV`: [Development Site](<DEV_SITE_URL>)
  - `QA`: [QA Site](<QA_SITE_URL>)
  - `STAG`: [Staging Site](<STAG_SITE_URL>)
  - `PROD`: [Production Site](<PROD_SITE_URL>)
- Production deployment action: Claude must not deploy, migrate, publish, upload, change hosting settings, or edit production environment values without explicit developer approval for that exact action.

## Required Deployment Details To Ask For

- Hosting target/provider: `<ASK_DEVELOPER>`
- Source control and pipeline: `<ASK_DEVELOPER>`
- Hosting account/project/resource placeholder: `<ASK_DEVELOPER>`
- Production URL and subdirectory: [Production Site](<PROD_SITE_URL>), subdirectory: `<ASK_DEVELOPER>`
- Web root, artifact path, startup command, service entry point, or container image: `<ASK_DEVELOPER>`
- Runtime/platform version: `<ASK_DEVELOPER>`
- Package manager/build availability on target: `<ASK_DEVELOPER>`
- Background processing/scheduler support: `<ASK_DEVELOPER>`
- Deployment workflow owner and steps: `<ASK_DEVELOPER>`
- Rollback and monitoring/log access: `<ASK_DEVELOPER>`
- Required `.env` keys using placeholders only: `<ASK_DEVELOPER>`

## Required Validation Details To Ask For

- Syntax/type/static checks: `<ASK_DEVELOPER>`
- Lint/format commands: `<ASK_DEVELOPER>`
- Unit/integration/e2e commands: `<ASK_DEVELOPER>`
- Build/package commands: `<ASK_DEVELOPER>`
- Security checks (dependency audit, static security analysis, secret scan) and where they run: `<ASK_DEVELOPER>`
- Deployment smoke test: `<ASK_DEVELOPER>`
- Known validation limitations: `<ASK_DEVELOPER>`

## Database Schema (Source Of Truth)

- Database/schema source of truth (the database's own metadata where approved read access exists, or the committed migrations and schema definition), per `.claude/rules/database-schema.md`: `<ASK_DEVELOPER>`
- Migration tool: as recorded under Migrations And Data Change Workflow above
- Schema name, where the engine uses one: `<ASK_DEVELOPER>`
- Table name prefix convention: `<ASK_DEVELOPER>`
- Table/column naming casing convention: `<ASK_DEVELOPER>`
- Resulting table name pattern: `<ASK_DEVELOPER>`
- Tables/schemas relevant to the current task: verified against the schema source in the current session; never from memory

## Application Database Connection (Only If The App Connects To A Database Directly)

- Does the application connect to a database directly: `<ASK_DEVELOPER>`
- Database engine the application connects to: `<ASK_DEVELOPER>`
- Database host/service placeholder: `<ASK_DEVELOPER>`
- Database name placeholder: `<DATABASE_NAME>`
- Database user placeholder: `<DATABASE_USER>`
- Schema/database owner: `<ASK_DEVELOPER>`
- Where connection settings/secrets live (env keys, placeholders only): `<ASK_DEVELOPER>`

## Knowledge Base Policy

- Claude must read existing knowledge-base files before editing when they exist.
- Claude must read this file before editing and update it with confirmed non-secret facts after developer answers.
- After each completed implementation, Claude must ask the developer whether to create or update the knowledge base now or defer it, and must write knowledge-base files only after the developer has verified, validated, and explicitly approved the update, per `.claude/rules/knowledge-base.md`.
- When approved, Claude must ask whether the work is a solution, a module, or a feature and where it sits in the hierarchy, then update the write-up and the knowledge-base README index.
- Claude must update the table dictionary after confirmed work that verifies or changes table/schema knowledge; when a change alters the schema, the table-dictionary update travels in the same change unit as the schema change.
