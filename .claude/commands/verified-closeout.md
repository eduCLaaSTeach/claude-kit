# Verified Closeout

Use after implementation and validation are complete.

Input: `[TASK_SUMMARY]`

Process:

1. Confirm what changed and which files were touched.
2. Confirm validation commands and results.
3. Confirm the developer has verified and validated that the task works.
4. Ask whether the knowledge base should be created/updated now or deferred. Do not write knowledge-base files without explicit approval; if deferred, skip to the return and report the deferral.
5. Ask whether the work is a solution, module, or feature and where it sits in the hierarchy (a solution has multiple modules; each module has multiple features). Do not classify it yourself.
6. Read existing knowledge-base files relevant to the task, including the README index.
7. Write or update the solution/module/feature write-up per `.claude/rules/knowledge-base.md`. A feature write-up covers logical flows and workflows, data models and tables, frontend, backend, API routing, and system architecture.
8. Update the master knowledge base when the task affects shared behavior, multiple solutions, or repo-wide conventions.
9. Update the table dictionary when database/table/schema knowledge changed.
10. Update the README index so every knowledge-base file, including any new one, is linked.
11. Do not record secrets, production row data, tokens, or unverified assumptions.

Return:

- Validation status
- Developer verification and approval status (approved or deferred)
- Classification confirmed (solution, module, or feature) and its place in the hierarchy
- Knowledge-base files updated
- README index status
- Table dictionary updates made
- Master knowledge-base updates made
- Remaining risks or follow-up
