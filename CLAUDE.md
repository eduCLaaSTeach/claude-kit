# Claude Code Entry Point

This repository is a portable Claude gateway. Start here, then follow the gateway files in `.claude/`.

## Required Startup

1. Read `.claude/README.md`.
2. Read `.claude/PROJECT-CONTEXT.md`.
3. Read all applicable files in `.claude/rules/` before planning or editing.
4. Fresh or unknown projects: start from `.claude/commands/fresh-project-start.md`.
5. Human guides live in `developer-handbook/`, indexed by `developer-handbook/README.md` and grouped into folders: `getting-started/HOW_TO_USE.md` (usage) and `getting-started/WORKING_WITH_CLAUDE.md` (team mindset); `prompts/GIT_PROMPTS.md` (Git/GitHub prompts) and `prompts/DAY_HANDOFF_PROMPT.md` (session handoff prompts: mid-session, end-of-day, and developer-to-developer, each paired with its resume prompt); `guidelines/DO_AND_DO_NOT.md` (behavior) and `guidelines/APPROVED_PLUGINS.md` (approved plugins); `reference/PROJECT-CONTEXT-UNDERSTANDING.md` (intake field guide).

## Non-Negotiable Rules

- Do not assume the app stack, hosting, source control, CI/CD, runtime, deployment, architecture, validation commands, security model, or user intent. Ask until requirements and success criteria are clear.
- Store only confirmed non-secret facts in `.claude/PROJECT-CONTEXT.md`. Never store secrets, tokens, passwords, private keys, full connection strings, `.env` values, customer records, or production row data.
- Before any database/schema/table/column/report/import/export/mapping/persistence work, use the SQL Server Schema MCP server named `schema`. Do not invent tables, columns, relationships, indexes, constraints, or enum values. Do not write DDL directly.
- Build every UI from the approved design system per `.claude/rules/ui-ux-quality.md` and the bundled `ui-ux-design` skill: layout, both themes, colors, fonts, logos, and favicons are approved constants in `.claude/skills/ui-ux-design/reference/design-tokens.md` with assets in `.claude/skills/ui-ux-design/assets/`. Use them by default; never re-derive the theme; deviate only when the developer asks. Ask the developer where brand assets go (`<BRAND_ASSETS_PATH>` in PROJECT-CONTEXT), never decide it.
- Document code with docstrings per `.claude/rules/code-documentation.md`: doc-comment every public or non-trivial module, class, interface, abstract class, enum, function, and method; document significant variables; explain non-obvious logic in the language's idiomatic style, no redundant noise.
- Record automatic feedback per `.claude/rules/feedback-logging.md`: when a developer presses the same unmet request about three times in a session (repeated intent, not literal prompt), write a secret-free feedback log to `.claude/feedback-logs/`, capture the developer identity, and push it, without asking permission.
- Do not claim completion until validation has run, or the limitation and exact follow-up steps are documented.

## Final Response Requirements

Always report: summary of changes; files changed; validation run and results; MCP tools used, if any; database/schema impact; deployment impact; knowledge-base/table-dictionary status; documentation/docstring coverage; security impact; risks, assumptions, and manual follow-up.
