# Project Context

Durable, non-secret project facts Claude reads before making changes. Field meanings, examples, and who answers each live in `developer-handbook/reference/PROJECT-CONTEXT-UNDERSTANDING.md`. Leave unknown fields as `<ASK_DEVELOPER>`; never guess.

Never store secrets here. Use placeholders such as `<HOSTING_RESOURCE>`, `<DATABASE_NAME>`, `<DATABASE_USER>`, `<MCP_ALIAS>`, and `<CLICKUP_URL>`.

## Required Intake Status

- Solution Cluster: `<ASK_DEVELOPER>`
- Module: `<ASK_DEVELOPER>`
- ClickUp URL: [ClickUp Workspace](<CLICKUP_URL>)
- Business purpose: `<ASK_DEVELOPER>`
- Primary users: `<ASK_DEVELOPER>`
- Application type: `<ASK_DEVELOPER>`
- Backend stack and version: `<ASK_DEVELOPER>`
- Frontend stack and version: `<ASK_DEVELOPER>`
- Database engine and version: `<ASK_DEVELOPER>`
- Package manager/runtime versions: `<ASK_DEVELOPER>`
- Authentication model: `<ASK_DEVELOPER>`
- Data sensitivity/PII: `<ASK_DEVELOPER>`

## Current Sprint Tasks

### Sprint: `<ASK_DEVELOPER>`

#### Cluster: `<ASK_DEVELOPER>`

##### Module: `<ASK_DEVELOPER>`

###### Feature: `<ASK_DEVELOPER>`

- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`
- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`

###### Feature: `<ASK_DEVELOPER>`

- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`
- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`

##### Module: `<ASK_DEVELOPER>`

###### Feature: `<ASK_DEVELOPER>`

- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`
- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`

###### Feature: `<ASK_DEVELOPER>`

- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`
- [ ] `<ASK_DEVELOPER>`
  - Validation commands: `<ASK_DEVELOPER>`
  - Success criteria: `<ASK_DEVELOPER>`

## Scope And Non-Goals

- Declared non-goals / out-of-scope feature classes: `<ASK_DEVELOPER>`
- Deferred work pushed to a later phase: `<ASK_DEVELOPER>`

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
- Tagline: `<ASK_DEVELOPER>` `[optional]`
- Brand-assets destination path: `<BRAND_ASSETS_PATH>` = `<ASK_DEVELOPER>`

### UI stack (per project)

- UI stack: `<ASK_DEVELOPER>`
- Charting library: `<ASK_DEVELOPER>`

### Feature toggles (confirmed defaults)

- Customizable dashboard: On
- Notifications: On (in-app, top-bar bell)
- SSO / external IdP: On - Microsoft Entra ID, direct or via the ECC -> SCC module (confirm which path per app)
- Recycle bin (soft-delete + restore): On
- Audit log: On

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

### Roles & tenancy

| Role | UI-standard tier | Record scope |
| --- | --- | --- |
| Administrator | admin | ALL records; the only role that permanently deletes |
| Collaborator | team/collaborator | OWN records plus records in their Business Unit |
| Contributor | self/contributor | OWN or directly assigned records only |
| Viewer | self-view/read-only | Read-only; scope OWN, TEAM, or Business Unit per app: `<ASK_DEVELOPER>` |
| Custom | per app, optional | Define scope and cluster grants before use: `<ASK_DEVELOPER>` |

Cluster grants per role:

| Cluster | Roles granted |
| --- | --- |
| Workspace | Viewer, Contributor, Collaborator, Administrator |
| Compliance | Collaborator, Administrator |
| Application Administration | Administrator only |
| System Administration | Administrator only |

- Tenancy: single-tenant
- Team/org hierarchy: Administrator (org root) -> Business Unit -> Team

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
  - Model routing / adapter + fallback: `<ASK_DEVELOPER>`
  - Max-tokens / temperature defaults: `<ASK_DEVELOPER>`
  - Per-agent token budget + cost ceiling: `<ASK_DEVELOPER>`
  - Golden-set location + pass-score bar: `<ASK_DEVELOPER>`
  - Eval-as-merge-gate expectation: `<ASK_DEVELOPER>`

## Migrations And Data Change Workflow

- File-based migration tool: `<ASK_DEVELOPER>`
- Migration file naming convention: `<ASK_DEVELOPER>`

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

## API / Interface Conventions

- Interface paradigm: `<ASK_DEVELOPER>`
- Resource / operation naming style: `<ASK_DEVELOPER>`
- Error contract format: `<ASK_DEVELOPER>`
- Version scheme: `<ASK_DEVELOPER>`
- Standard header names: `<ASK_DEVELOPER>`
- Pagination style and maximum page-size cap: `<ASK_DEVELOPER>`
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
- Source control provider: `<ASK_DEVELOPER>`
- GitHub repository URL: [GitHub Repository](<GITHUB_REPO_URL>)
- CI/CD or deployment pipeline: `<ASK_DEVELOPER>`
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
- Deployment smoke test: `<ASK_DEVELOPER>`
- Known validation limitations: `<ASK_DEVELOPER>`

## Schema MCP (Database Metadata Source Of Truth)

- Database/schema source of truth: SQL Server metadata through Schema MCP `schema`
- SQL Server schema name: `<ASK_DEVELOPER>`
- Table name prefix convention: `<ASK_DEVELOPER>`
- Table/column naming casing convention: `<ASK_DEVELOPER>`
- Resulting table name pattern: `[<SCHEMA>].<PREFIX><table_base_name>`
- Database/schema MCP server name and alias: `schema`
- Database/schema MCP tools: `mcp__schema__list_tables`, `mcp__schema__describe_table`, `mcp__schema__find_existing_tables_for_concept`, `mcp__schema__list_pending_proposals`, `mcp__schema__propose_table_change`, `mcp__schema__get_proposal`
- Connection, transport, URL, and credentials: in Claude Code's MCP configuration only; never recorded or printed here
- Auth and token refresh: per `.claude/docs/MCP-USAGE.md`; refresh via `.claude/commands/mcp-token-refresh.md`
- Availability: verified by session preflight (`/mcp-health-check`); if unavailable, stop schema work and alert the developer
- Tables/schemas relevant to the current task: discovered and verified live through the MCP tools; never from memory

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
