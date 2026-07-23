# Claude AI Agent

> Gateway version: v2026.8.1 (22 July 2026). Full history: `.claude/CHANGELOG.md`.

This folder is the complete Claude-only AI-agent gateway for a repository. Copy this `.claude/` folder into a blank project and start from zero. It holds required project context, rules, commands, subagents, skills, and MCP usage guidance.

## Read Order For Claude

1. `.claude/PROJECT-CONTEXT.md`.
2. `.claude/rules/hard-stops.md` first, for the conditions that require Claude to stop and ask.
3. Core rules: `fresh-project-gateway.md`, `project-intake.md`, `deployment.md`, `enterprise-governance.md`, `production-readiness.md`, `secret-handling.md`, `feedback-logging.md`. Feedback logging runs every session in the background (log when a developer presses the same unmet request about three times). Add task-specific rules:
   - code work: `code-documentation.md`
   - Git branching / PR / release / tag work: `git-branching-release.md`
   - any UI: `ui-ux-quality.md` (the authoritative UI standard) plus the bundled `ui-ux-design` skill at `.claude/skills/ui-ux-design/` (values in `reference/design-tokens.md`, assets in `assets/`). Every app reproduces the same look and feel; only the app name, title-bar name, navigation tree, brand-assets path, UI stack, feature toggles, entities, roles, and domain copy vary per project, and the in-project asset destination is asked, never chosen.
   - implementation / multi-phase work: `phased-workflow.md`
   - a service interface is exposed or consumed: `api-design.md`
   - a call crosses a process or network boundary: `resilience.md`
   - data is stored beyond transient request state: `data-governance.md`
   - a production service runs: `operations-incident.md`
   - runtime LLM or agent behavior ships: `ai-agent-governance.md`
4. Database/schema work: read the target repo `./docs/MCP-USAGE.md` if it exists, else `.claude/docs/MCP-USAGE.md`; then read `.claude/rules/schema-mcp.md` in full.
5. Deployment work: read `.claude/rules/deployment.md` before touching layout, entry points, assets, deploy scripts, hosting settings, infrastructure, or release instructions.
6. Read existing repo docs and knowledge-base files before editing; for cross-session continuity see `.claude/rules/knowledge-base.md`.
7. Use `.claude/commands/` for fresh-project start, intake, MCP health checks, MCP token refresh, schema planning, deployment checks, database tasks, and verified closeout.

## What This Folder Contains

- `PROJECT-CONTEXT.md`: required non-secret project, stack, deployment, and database context.
- `settings.json`: safe defaults for Claude Code permissions.
- `docs/MCP-USAGE.md`: bundled Schema MCP usage requirements.
- `feedback-logs/`: automatic improvement feedback (see `rules/feedback-logging.md`).
- `rules/`: core rules plus conditional rules that apply only when their gate matches (`api-design.md`, `resilience.md`, `data-governance.md`, `operations-incident.md`, `ai-agent-governance.md`).
- `commands/`: reusable prompts for intake, MCP health checks, MCP token refresh, schema browse/reuse, database work, code documentation, deployment checks, and verified closeout; `fresh-project-start.md` for a blank repo.
- `agents/`: specialized subagents for review, database planning, security, testing, and release readiness.
- `skills/ui-ux-design/`: the approved design system - component guidelines, the token specification (`reference/design-tokens.md`), and the CLaaS2SaaS brand asset pack (`assets/`).

## Key Workflows

- Startup: on a fresh project, start from `.claude/commands/fresh-project-start.md`. Ask what to build, how, which stack/versions, hosting/deployment, database/MCP details, and how success is validated. Keep asking until the task is clear enough to implement, validate, explain, and deploy safely. Durable non-secret answers go in `.claude/PROJECT-CONTEXT.md` after confirmation. Secrets never go in this folder.
- Clarification: never assume unclear requirements, context, deployment, schema, credentials, validation, or intent. This is a hard stop - do not implement, edit, run commands, update context, write deployment steps, touch schema, or claim completion until unclear points are answered.
- Schema MCP: mandatory for schema and database-backed code. Confirm the MCP usage doc was read and summarize the four rules (no DDL; no invented names; check existing tables first; stop if the server is unreachable). Confirm the `schema` server and its `mcp__schema__*` tools are available before use; if missing, disconnected, unauthorized, or expired, stop and alert the developer. See `rules/schema-mcp.md`.
- Deployment: no default model. Ask what source control, CI/CD, hosting, deployment target, runtime, build, artifact flow, data services, background processing, and operations apply. Provider- and stack-specific rules apply only after the developer confirms them. See `rules/deployment.md`.
- Knowledge base: read `PROJECT-CONTEXT.md` and repo knowledge-base files before editing. After each completed implementation (validation run or limitation documented), prompt the developer to create/update the knowledge base now or defer; write only after explicit approval. It is living documentation, kept in sync behind the same gate. See `rules/knowledge-base.md`.
- Documentation: when writing/editing code, add docstrings per `rules/code-documentation.md`; use `commands/document-code.md` to add or audit documentation.
