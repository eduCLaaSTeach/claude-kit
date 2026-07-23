# How To Use This Claude Gateway

This repository contains a portable `.claude/` gateway folder. Copy `.claude/` into the root of any fresh or existing project, then start Claude from that project.

The gateway is intentionally blank and dynamic for application stack, hosting, source control, CI/CD, runtime, package manager, deployment method, architecture, validation command, and security model. For database schema work, the final source of truth is the SQL Server-backed Schema MCP server named `schema`.

## First Prompt For A New Project

Use this prompt after copying `.claude/` into a project:

```text
This is a fresh or unknown project. Use .claude as the project gateway.

Read .claude/README.md and .claude/PROJECT-CONTEXT.md.
Read all applicable .claude/rules files.
Do not assume any tech stack, hosting provider, deployment model, source control provider, CI/CD pipeline, validation command, architecture, or runtime.
For database/schema work, always use the SQL Server-backed Schema MCP server named schema before referencing tables or columns.
Ask me every required question before doing any work.
Keep asking until the task, stack, database, deployment, validation, security, and success criteria are clear.
Do not write code yet.
Do not store secrets.
After I confirm answers, update .claude/PROJECT-CONTEXT.md with confirmed non-secret facts only.
```

## Gateway Commands

Invoke these as slash commands (for example `/schema-browse`) or by asking Claude to use the matching file in `.claude/commands/`:

- `fresh-project-start` - bootstrap a blank or unknown repository.
- `project-intake` - repeatable intake to confirm stack, database, deployment, and validation facts.
- `schema-browse` - inspect existing database metadata through Schema MCP.
- `schema-reuse-plan` - check for reusable tables before proposing new storage.
- `mcp-health-check` - confirm the `schema` MCP server is connected and authorized.
- `mcp-token-refresh` - refresh the schema MCP credential when its short-lived token expires.
- `database-task` - run database-backed work under the Schema MCP rules.
- `document-code` - add or audit structured documentation for a file or scope.
- `deployment-check` - review deployment readiness before delivering deployment work.
- `verified-closeout` - final validation, the gated knowledge-base update, and reporting before calling work done.

The bundled `ui-ux-design` skill is mandatory for UI work — it carries the approved design system (see UI And UX Work below).

## Rules Overview

Claude reads `.claude/rules/` before acting. Always-apply rules include project intake, fresh-project gateway, hard-stops, enterprise governance, production readiness, secret handling, knowledge base, code documentation, phased workflow, git branching/release, deployment, and Schema MCP. Conditional rules apply only when their trigger matches:

- `ui-ux-quality.md` - the project has a user interface.
- `api-design.md` - a service interface is exposed or consumed.
- `resilience.md` - code calls a dependency across a process or network boundary.
- `data-governance.md` - data is stored beyond transient request state.
- `operations-incident.md` - a production service runs.
- `ai-agent-governance.md` - the project ships runtime LLM or agent behavior.

## What Claude Must Ask

Claude should ask enough questions to understand:

- what needs to be built, changed, fixed, reviewed, deployed, or investigated
- why the work is needed and what success means
- approved language, framework, runtime, frontend, backend, package manager, and architecture
- database scope, SQL Server schema source of truth, Schema MCP `schema` connectivity, and whether database-backed work is in scope
- source control provider, CI/CD pipeline, artifact flow, hosting target, deployment method, rollback, and monitoring
- environment variables using placeholders only
- security, authentication, authorization, data sensitivity, and compliance constraints
- validation commands and where they can run
- documentation and knowledge-base expectations
- the current sprint number and every sprint task, grouped by cluster, module, and feature, written into `.claude/PROJECT-CONTEXT.md` as a checkbox list (`- [ ]` / `- [x]`) — updated at the start of each sprint, since ClickUp requires signing in and Claude cannot open the link, so anything not written there does not exist for Claude
- for a UI, the App Definition: the app name, title-bar name, navigation tree (the features you place under the four fixed clusters — Workspace, Compliance, Application Administration, System Administration; use only the subset you need, and you cannot add another cluster), and the brand-assets destination path (`<BRAND_ASSETS_PATH>` — where the logo/favicon files live in the project; always asked, never chosen). The theme, tokens, fonts, and the asset files themselves are approved in the bundled `ui-ux-design` skill and are never asked.
- whether conditional areas apply: a service interface, cross-boundary calls, stored data with retention or privacy needs, a running production service, or shipped LLM/agent behavior

If any answer is missing, vague, conflicting, risky, or stale, Claude must stop and ask again before acting.

Every intake field, what it means, and what a good answer looks like is documented with examples in `developer-handbook/reference/PROJECT-CONTEXT-UNDERSTANDING.md`.

## What To Store

Store only confirmed non-secret project facts in:

```text
.claude/PROJECT-CONTEXT.md
```

Never store secrets, passwords, tokens, private keys, full connection strings, `.env` values, customer records, or production row data. Use placeholders instead:

```text
<APP_URL>
<DATABASE_NAME>
<DATABASE_USER>
<HOSTING_RESOURCE>
<MCP_SERVER_NAME>
<MCP_ALIAS>
<SCHEMA_MCP_URL>
<TOKEN>
<SECRET_VALUE>
```

## Database And MCP Work

Before any database-backed or schema-related work, Claude must:

1. Read `.claude/docs/MCP-USAGE.md`.
2. Read `.claude/rules/schema-mcp.md`.
3. Verify the SQL Server-backed Schema MCP server named `schema` is reachable.
4. Confirm the MCP usage rules back to the developer.
5. Verify tables and columns through `mcp__schema__list_tables` and `mcp__schema__describe_table`.
6. Stop if MCP is missing, unreachable, unauthorized, or not approved for the needed work.
7. Refresh the credential with `.claude/commands/mcp-token-refresh.md` when the short-lived token expires; the auth model, token command, and TTL are confirmed facts in `.claude/PROJECT-CONTEXT.md`. Prefer a self-refreshing headers helper so the token renews on reconnect, and never print the token or hardcode the MCP URL.

Expected Schema MCP tools:

- `mcp__schema__list_tables`
- `mcp__schema__describe_table`
- `mcp__schema__find_existing_tables_for_concept`
- `mcp__schema__list_pending_proposals`
- `mcp__schema__propose_table_change`
- `mcp__schema__get_proposal`

Useful schema prompts:

```text
Use .claude/commands/schema-browse.md and show me all database tables through Schema MCP.
```

```text
Use .claude/commands/schema-browse.md and describe <SCHEMA.TABLE> through Schema MCP.
```

```text
Use .claude/commands/schema-reuse-plan.md for this data concept: <CONCEPT>.
```

Claude must not invent table names, column names, relationships, indexes, constraints, schema details, or DDL.

## UI And UX Work

When the project has a user interface, `.claude/rules/ui-ux-quality.md` is the authoritative standard, and the bundled `.claude/skills/ui-ux-design/` skill is the approved design system, delivered as written guidelines: the layout, both themes, the color combination, fonts, and icon style are specified value-by-value in the skill (all tokens in `reference/design-tokens.md`), and the brand assets (`assets/`: four per-theme logos and two `.ico` favicons) are the only files meant to be copied into the project. Every application reproduces the same look and feel.

The developer supplies only the app-specific values in `.claude/PROJECT-CONTEXT.md`: the app name, the browser title-bar name, the sidebar navigation tree, and the brand-assets destination path (`<BRAND_ASSETS_PATH>` — where the logo/favicon files live in the project, per the confirmed stack; Claude asks for it and never chooses the location, or the developer copies the files manually and records the path). Claude must not invent these — and must not ask about or vary the theme, colors, fonts, logos, or favicon, which are the approved standard.

Claude builds every screen from the one master shell (full-height sidebar, slim top bar over the canvas), keeps standing navigation in the sidebar under the four fixed clusters (Workspace, Compliance, Application Administration, System Administration — using only the ones the app needs, never adding another) at most three accordion levels deep, implements the approved token values in the project's own stack and styling system in both themes, swaps the per-theme logos and favicon with the theme switcher, and designs the empty, loading, and error states for every data-driven view. Claude applies the UI standard exactly the same way every time — the same archetypes, component primitives, status-role meanings, and iconography — and only the app identity, navigation tree, entities, and domain content vary; Claude never invents a one-off pattern.

## Deployment Work

Before any deployment-related work, Claude must:

1. Read `.claude/rules/deployment.md`.
2. Ask for the real source control, CI/CD, hosting, runtime, artifact, deployment, rollback, and validation details.
3. Avoid provider-specific instructions until the provider and deployment method are confirmed.
4. Use placeholders for environment values.
5. Ask before production-impacting actions.
6. When a change needs them, list the manual server-side or control-panel steps the developer must perform (environment variables, cron or scheduled jobs, SSL/TLS, DNS, database creation, file permissions, extensions, workers, restart), provider-neutral until the provider is confirmed, and say so when none are needed.

Nothing is fixed. Deployment may be any developer-confirmed workflow.

## Branching And Releases

Branch, promotion, and release work follows `.claude/rules/git-branching-release.md`. Working branches are `feature/*`, `bugfix/*`, `release/*`, and `hotfix/*`; environment branches are `DEV`, `QA`, `STAG`, and `PROD`, promoted in that order. After pushing a working branch, Claude shows the Pull Request URL and the ordered next steps rather than merging automatically, unless you ask it to.

Production release tags use CalVer `vYYYY.R.P`, created from `PROD`. All Git and GitHub actions are pre-authorized; Claude states the plan first. Running a production deploy or database migration is a separate action that still needs your explicit approval.

For copy-ready prompts that run these Git workflows (sync, stash, branch, PR, promotion, release), see `developer-handbook/prompts/GIT_PROMPTS.md`.

## Knowledge Base Updates

After each completed implementation, Claude asks you whether to create or update the knowledge base now or defer it. Knowledge-base files are written only after:

1. implementation is complete
2. validation ran, or the limitation is documented
3. you have verified and validated the work and explicitly approved the update

Claude never writes knowledge-base files without your explicit approval; if you defer, it records nothing and notes the deferral in the final report.

When you approve, Claude asks whether the work is a solution, a module, or a feature and where it sits in the hierarchy - a solution has multiple modules, and each module can have multiple features. You classify it; Claude does not. A feature write-up covers logical flows and workflows, data models and tables, frontend, backend, API routing, and system architecture; solution and module READMEs hold the overview and link their children.

The knowledge-base README is the index: every knowledge-base file is linked from it, so you can jump straight to any write-up. The knowledge base is living documentation - as the project is built, Claude updates the affected write-ups and the index (behind the same approval gate) so they never go stale against shipped behavior.

For intake facts, update only `.claude/PROJECT-CONTEXT.md` with confirmed non-secret information.

## Session Handoff

When you want the work to survive a session boundary, send one of the copy-ready prompts in `developer-handbook/prompts/DAY_HANDOFF_PROMPT.md`. Claude rebuilds the state from durable evidence (the working-memory log, Git history, open PRs, feedback logs, the sprint list, and the current session) and writes a timestamped handoff file - done, in progress, pending, blockers, and the single first action to resume - into a shared `handoffs/` folder, so the next session can point to that one file and pick up. Each handoff is its own `<timestamp>_<short-topic>.md` file so many people's handoffs coexist without overwriting each other, and you resume by naming the exact file (there is no "latest" shortcut). Three variants are provided: an end-of-day handoff that covers the whole day, a mid-work handoff for when you just want to continue in a fresh session on the same machine without explaining the task again, and a developer-to-developer handoff for when another developer will pull the work and finish it on their own device (commit and push first, so nothing is stranded on your machine). Claude cannot read past chat sessions, so keep your working-memory log current; the handoff is only as complete as the trace the session left behind.

## Completion Checklist

A task is not complete until Claude reports:

- what changed
- files changed
- validation commands and results
- MCP tools used, if any
- database/schema impact
- deployment impact
- knowledge-base/table-dictionary status
- documentation/docstring coverage for code changes
- security impact
- UI states covered (success, empty, loading, error, small-screen) for UI work
- risks, assumptions, and manual steps, including manual server-side steps the developer must perform

Claude must not claim work is bug free or complete unless validation evidence exists, or the exact validation limitation and follow-up steps are documented.
