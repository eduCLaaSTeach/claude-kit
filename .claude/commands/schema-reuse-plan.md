# Schema Reuse Plan

Use before building a feature that may need persistence.

Input: `[FEATURE_OR_DATA_CONCEPT]`

Process:

1. Read `.claude/PROJECT-CONTEXT.md`, `.claude/docs/MCP-USAGE.md`, `.claude/rules/schema-mcp.md`, and `.claude/rules/knowledge-base.md`.
2. Verify the SQL Server-backed Schema MCP server `schema` is reachable.
3. Use `mcp__schema__find_existing_tables_for_concept` with a concise business description.
4. Group matches by the confidence or ranking Schema MCP returns. If no ranking exists, say so explicitly.
5. For likely candidates, use `mcp__schema__describe_table` before recommending usage.
6. Explain reuse options, tradeoffs, and missing fields, then stop and ask the developer which option to use.
7. After implementation is verified, follow the knowledge-base gate in `.claude/rules/knowledge-base.md`: prompt the developer to create/update the knowledge base now or defer, and update the table dictionary and relevant files only on explicit approval (a table-dictionary update for an actual schema change travels with that schema change).

Do not use `mcp__schema__propose_table_change` until the developer approves the new table or column shape.

Return:

- Concept searched
- Tables considered
- Recommended reuse path
- Proposed new schema only if needed
- Questions for developer approval
- Knowledge-base/table-dictionary update plan
