# Gateway Changelog

The version of the Claude gateway kit you have. After you pull, the top row is the current version; read down to see what changed since you last pulled. This file ships inside `.claude/`, so it travels with the kit. The same version is stamped at the top of `.claude/README.md`, and every version has a matching git tag and GitHub Release (full notes on the [GitHub Releases page](https://github.com/CLaaS2SaaS/claude-code-gateway-kit/releases)).

Current release at the top, first release at the bottom. Major releases (the `.0` of each series) are shown in **bold**.

| Version | Released | What changed |
| --- | --- | --- |
| v2026.8.1 | 22 July 2026 | Drop the `Read(...)` deny entries for sensitive files (`.env`, `.mcp.json`, `.claude.json`, `secrets/`, `.pem`, `.key`) from `.claude/settings.json`. The secret-handling rule already forbids reading or printing secrets, so the deny list was redundant. |
| **v2026.8.0** | 21 July 2026 | Add an in-repo version marker so anyone who pulls can tell which version they have: this changelog plus a `> Gateway version:` stamp in `.claude/README.md`, both shipped inside `.claude/`. Reorganize `developer-handbook/` into `getting-started/`, `prompts/`, `guidelines/`, `reference/`, and a top-level `layout-template/`, with a `README.md` index. |
| v2026.7.5 | 21 July 2026 | Put session handoffs in a shared `handoffs/` folder with unique, timestamped filenames so parallel work is not clobbered. |
| v2026.7.4 | 21 July 2026 | Add a developer-to-developer session-handoff flow for moving work from one user/device to another. |
| v2026.7.3 | 21 July 2026 | Slim `PROJECT-CONTEXT.md` to bare fields and make the understanding guide the single home for explanations. |
| v2026.7.2 | 21 July 2026 | Record SQL Server schema and table-naming conventions as per-project facts. |
| v2026.7.1 | 21 July 2026 | Pair the session handoff prompts into two ordered, resumable flows. |
| **v2026.7.0** | 17 July 2026 | Condense the gateway kit and add an end-of-day handoff prompt. |
| **v2026.6.0** | 15 July 2026 | Define the four fixed navigation clusters as the sidebar's approved top tier. |
| v2026.5.7 | 13 July 2026 | Rename the profile-menu theme control's section to a fixed "Appearance" label (the System/Dark/Light segmented control unchanged). |
| v2026.5.6 | 13 July 2026 | Add an optional segmented/tabbed form-modal to the ui-ux-design modal component. |
| v2026.5.5 | 13 July 2026 | Sidebar collapse/expand uses one icon (the i-panel glyph) in both states, matching the reference layout. |
| v2026.5.4 | 7 July 2026 | Prompt to create/update the knowledge base after each implementation, writing only after explicit approval; add a hard stop backing the gate. |
| v2026.5.3 | 3 July 2026 | Record every URL field in `PROJECT-CONTEXT.md` as a markdown link with a placeholder URL. |
| v2026.5.2 | 3 July 2026 | Record the GitHub repository URL in `PROJECT-CONTEXT.md` as a placeholder (plain URL, never token-bearing). |
| v2026.5.1 | 3 July 2026 | Entities in `PROJECT-CONTEXT.md` capture the data shape (tables, columns, child tables, field-to-column mapping), not just the screen. |
| **v2026.5.0** | 2 July 2026 | Replace the one-shot intake fields with a Current Sprint Tasks board (sprint number, tasks under Module/Feature headings). |
| **v2026.4.0** | 2 July 2026 | Adopt the approved CLaaS2SaaS design system as the default UI standard: guidelines, a token spec, and a bundled brand asset pack. |
| v2026.3.8 | 1 July 2026 | Rework the reference profile menu, add a centered profile page, and require applying the UI standard the same way every time. |
| v2026.3.7 | 1 July 2026 | Clarify how sidebar accordion depth is counted and where an in-canvas tab strip comes from. |
| v2026.3.6 | 1 July 2026 | Group team guides under `developer-handbook/`, with new Git-prompt, approved-plugins, and mindset guides. |
| v2026.3.5 | 1 July 2026 | Deployment rule now requires surfacing the manual server-side steps a developer must perform. |
| v2026.3.4 | 1 July 2026 | Add the MCP token-refresh skill and headers-helper auto-refresh guidance. |
| v2026.3.3 | 1 July 2026 | Add flicker-free sidebar collapse and expand to the UI standard. |
| v2026.3.2 | 30 June 2026 | Sidebar navigation now uses accordions (max 3 levels) plus in-canvas tabs. |
| v2026.3.1 | 30 June 2026 | Overlay-swap collapse behavior for the sidebar brand block. |
| **v2026.3.0** | 26 June 2026 | Brand-neutral ui-ux-design skill and UI-standard enrichment. |
| **v2026.2.0** | 24 June 2026 | Expand the gateway with neutral governance rules and a UI standard, add automatic feedback logging, and keep UI defaults as placeholders only. |
| **v2026.1.0** | 19 June 2026 | First release of the Claude Code gateway kit: the portable `.claude` gateway (rules, commands, agents, docs) plus `CLAUDE.md` and usage guides. |
