# Hard Stops - Never Do These

Quick index of the gateway's non-negotiable stops. Each entry is a one-line pointer; the authoritative detail, exceptions, and required response live in the linked rule file. When an entry and its source rule appear to disagree, the source rule wins.

Hitting any of these means stop and ask the developer. Do not work around a stop because it is inconvenient, urgent, or because a tool call failed.

| Hard stop | Source of truth |
| --------- | --------------- |
| Proceed past ambiguity on a guess instead of asking | `.claude/rules/project-intake.md` / `.claude/rules/fresh-project-gateway.md` |
| Add a secret, token, key, credential, or full connection string to any file | `.claude/rules/secret-handling.md` |
| Execute DDL directly, reference unverified tables/columns, or bypass the Schema MCP | `.claude/rules/schema-mcp.md` |
| Write code that depends on an unfulfilled schema proposal | `.claude/rules/schema-mcp.md` |
| Change production deploy, infrastructure, auth, authz, or schema without approval, or merge your own such change unreviewed | `.claude/rules/enterprise-governance.md` / `.claude/rules/deployment.md` |
| Run a production deploy or database migration without explicit approval | `.claude/rules/deployment.md` / `.claude/rules/production-readiness.md` |
| Introduce a new dependency, framework, service, or runtime without sign-off | `.claude/rules/enterprise-governance.md` |
| Ship a prompt or model change without the required eval gate | `.claude/rules/ai-agent-governance.md` |
| Let an agent take an irreversible or high-impact action without human confirmation | `.claude/rules/ai-agent-governance.md` |
| Place real production data or PII in non-production, tests, prompts, or logs | `.claude/rules/data-governance.md` |
| Make an unbounded outbound call (no timeout) or retry a non-idempotent operation | `.claude/rules/resilience.md` |
| Return a success status with an error body, or leak internals (stack traces, SQL, secrets, PII) in an API response | `.claude/rules/api-design.md` |
| Start a service with required configuration missing or unvalidated, or give a secret a hardcoded default | `.claude/rules/production-readiness.md` |
| Claim work complete without showing validation evidence | `.claude/rules/production-readiness.md` |
| Silently reverse a recorded decision instead of surfacing it as an open question | `.claude/rules/knowledge-base.md` |
| Write knowledge-base files before the developer verifies, validates, and explicitly approves the update | `.claude/rules/knowledge-base.md` |
| Collapse phase gates or advance without approval | `.claude/rules/phased-workflow.md` |
| Build a declared non-goal or out-of-scope feature class, even if asked | `.claude/PROJECT-CONTEXT.md` |
