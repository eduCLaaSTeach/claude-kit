# Database Task

Use for database-backed features, schema questions, reports, imports, exports, mappings, or query changes.

Input: `[DATABASE_TASK_OR_DATA_CONCEPT]`

Process:

1. Read `.claude/docs/MCP-USAGE.md`, `.claude/rules/schema-mcp.md`, `.claude/rules/knowledge-base.md`, and `.claude/PROJECT-CONTEXT.md`.
2. Confirm to the developer that the MCP usage document was read and summarize the four key rules.
3. Verify the SQL Server-backed Schema MCP server `schema` is reachable before schema-dependent work.
4. Use `mcp__schema__find_existing_tables_for_concept` before proposing new storage.
5. Use `mcp__schema__list_tables` and `mcp__schema__describe_table` before referencing any table or column.
6. Stop if MCP is unreachable, unauthorized, or missing required scope.
7. After validation, follow the knowledge-base gate in `.claude/rules/knowledge-base.md`: prompt the developer to create/update the knowledge base now or defer, and write files only on explicit approval. A table-dictionary update documenting an actual schema change travels in the same change unit as the schema change.

Return:

- MCP file read confirmation
- Four-rule summary
- MCP tools used
- Tables/concepts verified
- Proposed path or blocker
- Validation plan
- Knowledge-base/table-dictionary follow-up
