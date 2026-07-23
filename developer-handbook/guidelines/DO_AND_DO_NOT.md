# Do And Do Not Guide For Claude Vibe Coding

This guide is for using the `.claude/` gateway in fresh or existing projects. It combines this repository's rules with current Claude Code guidance: give Claude clear context, let it explore before coding, provide verification checks, keep instructions concise, manage context aggressively, and use separate review/verification steps for higher-risk work.

## Prime Rules

Do:

- Start every fresh or unclear project from zero.
- Read `.claude/README.md`, `.claude/PROJECT-CONTEXT.md`, and applicable `.claude/rules/` files first.
- Ask the developer what to build, why it matters, how it should work, what is out of scope, and how success will be verified.
- Keep asking until stack, database, deployment, validation, security, and success criteria are clear.
- Store only confirmed non-secret facts in `.claude/PROJECT-CONTEXT.md`.
- Use the SQL Server-backed Schema MCP server named `schema` before any database/schema/table/column work.
- Validate work with commands, tests, screenshots, MCP metadata, or other developer-confirmed checks.
- Show evidence: commands run, results, files changed, MCP tools used, and remaining risks.
- Check `.claude/rules/hard-stops.md` for the non-negotiable stops before acting, and run non-trivial work through the gated lifecycle in `.claude/rules/phased-workflow.md` (Discovery, Design, Implementation, Integration).
- Apply the conditional rules when their trigger matches: UI (`ui-ux-quality.md`), a service interface (`api-design.md`), cross-boundary calls (`resilience.md`), stored data (`data-governance.md`), a production service (`operations-incident.md`), and shipped LLM/agent behavior (`ai-agent-governance.md`).
- Expect the gateway to auto-record an improvement feedback log to `.claude/feedback-logs/` when the same request goes unmet about three times, per `.claude/rules/feedback-logging.md`.

Do not:

- Do not assume a language, framework, runtime, package manager, hosting provider, source control provider, CI/CD system, deployment model, architecture, validation command, or security model.
- Do not write code when requirements are unclear.
- Do not update project context from guesses.
- Do not store secrets, tokens, passwords, private keys, full connection strings, `.env` values, customer records, or production row data.
- Do not claim work is complete, safe, or bug free without validation evidence.
- Do not invent database tables, columns, relationships, indexes, constraints, enum values, or schema details.
- Do not write SQL DDL directly.
- Do not bypass Schema MCP because it is inconvenient or unavailable.

## Intake

Do:

- Ask focused questions before implementation.
- Ask about the exact task, target users, business rules, edge cases, constraints, and acceptance criteria.
- Ask what stack is approved and what versions are required.
- Ask what source control, CI/CD, hosting, deployment, rollback, and monitoring flow exists.
- Ask which validation commands can run and where they can run.
- Ask what data is sensitive and what must never be logged or exposed.
- Ask the team to update the sprint number and list that sprint's tasks, grouped by cluster, module, and feature, in `.claude/PROJECT-CONTEXT.md` as a checkbox list; ClickUp requires signing in, so Claude cannot open the link.
- Update `.claude/PROJECT-CONTEXT.md` after the developer confirms non-secret facts.

Do not:

- Do not treat folder names, old files, package files, screenshots, or prior projects as confirmation.
- Do not stop asking just because the question list is long.
- Do not let urgency override clarification.
- Do not ask for secret values; ask for placeholders.
- Do not proceed from partial answers if the missing detail affects implementation, database, deployment, security, or validation.

## Prompting

Do:

- Give Claude precise scope and concrete success criteria.
- Reference real files, docs, screenshots, errors, and examples when available.
- Ask Claude to inspect the current codebase before proposing changes.
- Ask for a plan for multi-file, risky, unfamiliar, or ambiguous work.
- Ask Claude to implement only after the plan and unanswered questions are resolved.
- Ask Claude to run checks and iterate until they pass.

Good prompt shape:

```text
Read .claude/README.md and .claude/PROJECT-CONTEXT.md.
This task is: <TASK>.
Do not assume missing stack/deployment/database details.
Ask me any required questions first.
After clarification, inspect the relevant files, propose a short plan, implement, run <VALIDATION>, and report evidence.
```

Do not:

- Do not say only "make it better" when you already know what better means.
- Do not combine unrelated tasks in one long session.
- Do not hide acceptance criteria in vague wording.
- Do not ask for implementation and then add critical constraints after the work is done.
- Do not let Claude continue down a wrong path; stop and correct early.

## Exploration And Planning

Do:

- Explore first when the codebase, architecture, or task is unclear.
- Keep exploration scoped to the likely area of work.
- Use subagents for broad research or independent review so the main context stays clean.
- Ask for a short implementation plan before risky or multi-file edits.
- Confirm the plan before coding when the blast radius is high.

Do not:

- Do not let Claude read hundreds of files without a clear search goal.
- Do not ask for a plan when the change is tiny and fully specified.
- Do not accept a plan that lacks validation steps.
- Do not use planning as a substitute for developer confirmation.

## Coding

Do:

- Follow the existing project patterns after they are confirmed.
- Keep changes small and reviewable.
- Prefer root-cause fixes over surface patches.
- Preserve existing architecture unless the developer approves a redesign.
- Add or update tests when behavior changes.
- Document code with structured docstrings per `.claude/rules/code-documentation.md`: doc-comment functions, methods, classes, interfaces, abstract classes, enums, and modules, document significant variables, and explain non-obvious logic using the project's idiomatic doc-comment style.
- Keep deployment, dependency, schema, authentication, authorization, and infrastructure changes behind explicit approval.

Do not:

- Do not introduce dependencies, services, queues, caches, build tools, infrastructure, or deployment requirements without approval.
- Do not rewrite unrelated code.
- Do not change formatting broadly unless formatting is the task.
- Do not remove user changes or revert files unless explicitly asked.
- Do not hardcode environment-specific URLs, credentials, tenant IDs, provider details, or secrets.
- Do not leave public or non-trivial functions, methods, classes, interfaces, abstract classes, enums, or modules undocumented.
- Do not add noisy comments that merely restate self-evident code, and do not leave stale or misleading documentation.

## UI And UX

Applies only when the project has a user interface. `.claude/rules/ui-ux-quality.md` is the authoritative standard; the bundled `.claude/skills/ui-ux-design/` skill is the approved design system — written guidelines whose theme, colors, fonts, logos, and favicons are constants (all values in `reference/design-tokens.md`, brand images in `assets/`), identical in every application.

Do:

- Apply the UI standard exactly as written every time: the same archetype for the same kind of screen, the same primitive for the same control, the same status-role meaning, and the same icon for the same action; only the app identity, navigation, entities, and domain content vary. Following the approved rules deterministically is what keeps output consistent.
- Extend the one master shell: full-height left sidebar, a slim top bar over the canvas, and a visible divider plus edge shadow between the chrome and the canvas.
- Keep all standing navigation in the sidebar under the four fixed clusters (Workspace, Compliance, Application Administration, System Administration; use only the ones the app needs), config-driven and role-filtered, at most three accordion levels deep; use in-canvas tabs only for depth overflow beyond three levels or for a record's facets.
- Take the app-specific values from the App Definition in `.claude/PROJECT-CONTEXT.md` (app name, title-bar name, navigation tree, brand-assets path), or ask the developer.
- Build from the fixed token values written in the skill's `reference/design-tokens.md`, implemented in the project's own stack, in both themes, and use the bundled per-theme logos and favicons as supplied.
- Ask the developer where the brand asset files go in the project (`<BRAND_ASSETS_PATH>`); never pick the location — the developer may also copy the files manually and record the path.
- Design the empty, loading, and error states for every data-driven view, not just success.
- Meet WCAG AA: full keyboard access, a visible focus indicator, and respected reduced-motion.

Do not:

- Do not invent, re-derive, or re-skin the theme, palette, surfaces, fonts, logos, or favicon; they are approved.
- Do not invent the app-specific values either (app name, title-bar name, navigation tree); the developer supplies them.
- Do not invent your own variant of a pattern the standard defines, reinterpret it, or repurpose a status role's meaning or color — never a one-off.
- Do not move standing navigation to the top bar, add a fourth accordion level, replace the sidebar with top tabs or a bare hamburger, or add/invent/rename a navigation cluster beyond the four fixed ones (Workspace, Compliance, Application Administration, System Administration; using only a subset is fine).
- Do not put action buttons in the top bar; a primary action (create/"New") belongs in the page header of the archetype, and the top bar stays app name plus notifications and profile only.
- Do not hardcode colors or use system fonts; use the approved tokens, Montserrat headings, and Source Sans 3 body.
- Do not ship a blank screen or a lone spinner where a skeleton, empty state, or placeholder belongs.

## Database And Schema MCP

Do:

- Treat the SQL Server-backed Schema MCP server named `schema` as the only approved database metadata source of truth.
- Before database-backed work, read `.claude/docs/MCP-USAGE.md` and `.claude/rules/schema-mcp.md`.
- Confirm the four schema rules back to the developer: do not write DDL; do not invent table or column names; always check existing tables before proposing new ones; stop if the MCP server is unreachable.
- Use `mcp__schema__find_existing_tables_for_concept` before proposing new storage.
- Use `mcp__schema__list_tables` when table existence needs confirmation.
- Use `mcp__schema__describe_table` before referencing any table or column.
- Use `mcp__schema__propose_table_change` only after developer approval.
- Use `mcp__schema__list_pending_proposals` and `mcp__schema__get_proposal` when proposal status matters.
- Confirm the `schema` server is connected with `.claude/commands/mcp-health-check.md` before schema work.
- Refresh the schema credential with `.claude/commands/mcp-token-refresh.md` when a short-lived token expires (the TTL is a confirmed fact in `.claude/PROJECT-CONTEXT.md`); prefer a self-refreshing headers helper so the token renews on reconnect.

Do not:

- Do not use another database MCP path for schema truth in this gateway.
- Do not request production row data.
- Do not run arbitrary SQL through MCP.
- Do not create migrations or DDL as a shortcut.
- Do not write code against a proposed table until the table physically exists and is verified through Schema MCP.
- Do not guess column names from old code, naming patterns, memory, or screenshots.
- Do not print tokens, Authorization headers, or a token-bearing command line, and do not hardcode the schema MCP URL; keep it a developer-supplied value in `.claude/PROJECT-CONTEXT.md`.

## Deployment

Do:

- Treat deployment as unknown until the developer confirms it.
- Ask for source control, CI/CD, hosting, deployment method, runtime, build, artifact, environment placeholders, rollback, logs, monitoring, and smoke tests.
- Treat source control, CI/CD, build, artifact storage, hosting, database, and runtime as separate choices.
- Use provider-specific instructions only after the provider and workflow are confirmed.
- List the manual server-side or control-panel steps the developer must perform when a change needs them (environment variables, cron, SSL, DNS, database, permissions), provider-neutral until the provider is confirmed.
- Ask before production-impacting actions.

Do not:

- Do not assume any hosting provider, deployment target, source control provider, CI/CD system, container model, cloud service, local workflow, build tool, worker model, or server access.
- Do not deploy, upload, publish, migrate, edit production environment values, or change hosting settings without explicit approval.
- Do not put real environment values or credentials in docs or chat.
- Do not provide final deployment steps while deployment details are still unclear.

## Git, Branching, And Release

Follow `.claude/rules/git-branching-release.md`. All Git and GitHub actions are allowed without per-action confirmation, but state the plan first.

Do:

- Work on short-lived `feature/*`, `bugfix/*`, `release/*`, or `hotfix/*` branches and promote through `DEV`, `QA`, `STAG`, then `PROD`.
- After pushing a working branch, hand the Pull Request to the developer: show the PR URL and the ordered next steps.
- Tag production releases from `PROD` with CalVer `vYYYY.R.P`, and create a matching release where the project expects one.
- Apply the project's confirmed workflow knobs from `.claude/PROJECT-CONTEXT.md` (signing, merge strategy, branch deletion, reviewers).
- Reach for the copy-ready prompts in `developer-handbook/prompts/GIT_PROMPTS.md` for common sync, stash, branch, PR, promotion, and release workflows.

Do not:

- Do not create lowercase environment branches or recommend skipping stages such as `DEV -> PROD`.
- Do not force-push a shared or environment branch, or run a production deploy or migration without explicit approval.

## Validation

Do:

- Ask for validation commands during intake.
- Give Claude a check it can run or a manual verification step it can report.
- Prefer deterministic checks: tests, builds, linters, type checks, scripts, screenshots, or MCP metadata verification.
- Have Claude show evidence rather than only saying it passed.
- For high-risk work, use a review subagent or separate review step.

Do not:

- Do not accept "looks good" as validation.
- Do not claim tests passed unless they actually ran.
- Do not ignore failing checks unless the developer explicitly accepts the risk.
- Do not mark a task complete if validation is impossible without documenting why and what follow-up is required.

## Context Management

Do:

- Keep `.claude/PROJECT-CONTEXT.md` concise and factual.
- Use topic-specific rules and commands instead of one huge instruction file.
- Clear or restart sessions between unrelated tasks.
- Correct Claude early if it goes off track.
- After repeated failed corrections, start a fresh session with a better prompt and the learned constraints.

Do not:

- Do not overload project context with tutorials, speculation, logs, or long explanations.
- Do not mix unrelated workstreams in one conversation.
- Do not rely on conversation-only instructions for durable rules.
- Do not store sensitive data in memory, project context, docs, logs, or summaries.

## Review And Closeout

Do:

- Ask, after each completed implementation, whether the knowledge base should be created or updated now, or deferred.
- Wait for the developer to verify, validate, and explicitly approve before writing knowledge-base files.
- Ask whether the documented work is a solution, a module, or a feature (a solution has multiple modules; each module can have multiple features) and let the developer classify it.
- Keep the knowledge-base README as the index, linking every knowledge-base file, and update it with every knowledge-base change.
- Keep knowledge-base write-ups current as the project is built; update a stale write-up as part of the change that made it stale.
- Report files changed.
- Report commands run and results.
- Report Schema MCP tools used when database/schema work is involved.
- Report database/schema impact.
- Report deployment impact.
- Report knowledge-base/table-dictionary status.
- Report documentation/docstring coverage for code changes.
- Report security impact.
- Report UI states covered (success, empty, loading, error, small-screen) for UI work.
- Report assumptions, risks, and manual follow-up, including manual server-side steps the developer must perform.

Do not:

- Do not bury risks under a long success summary.
- Do not omit manual steps the developer must perform.
- Do not write knowledge-base files before the developer has verified, validated, and explicitly approved the update.
- Do not classify work as a solution, module, or feature yourself, and do not invent solution or module names.
- Do not leave a knowledge-base file unlinked from the knowledge-base README index.
- Do not say knowledge base was updated if it was intentionally deferred.
- Do not say no database impact when database-backed behavior or schema metadata was involved.

## Quick Developer Checklist

Before saying "go", confirm:

- The task is clear.
- The stack is confirmed.
- The database scope is confirmed.
- Schema MCP `schema` is reachable if database work is involved.
- Deployment target and method are confirmed if deployment is involved.
- Validation commands are known.
- Secrets are represented only by placeholders.
- The App Definition (app name, title-bar name, navigation tree, brand-assets destination path) is confirmed if the work touches the UI; the theme and the asset files themselves are the approved standard and need no confirmation, but where the assets live in the project (`<BRAND_ASSETS_PATH>`) is always developer-confirmed.
- The sprint number and the current sprint's tasks are listed in `.claude/PROJECT-CONTEXT.md` as a checkbox list, grouped by cluster, module, and feature.
- The branch target and release/promotion path are clear if promoting or releasing.
- Success criteria are explicit.
