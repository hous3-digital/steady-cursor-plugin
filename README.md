# Steady

Work with [Steady](https://app.estady.com.br) pentests, findings, and reports from Cursor or Claude Code. The plugin connects the agent to the Steady MCP server and adds a skill and commands so it reads Steady instead of guessing. Cursor also gets a rule.

## What you get

- **MCP connection** to `https://app.estady.com.br/api/mcp`.
- **Skill** `steady` — engagements, findings, comments, and report metadata.
- **Commands**: `/steady-status`, `/steady-findings`, `/steady-finding`, `/steady-comment`, `/steady-report`.
- **Cursor rule** that keeps answers tied to tool responses: no invented findings, masked evidence stays masked, and comments are posted only when asked. Claude Code gets the same limits from the skill.

## Load it locally

This repo is the plugin. Cursor reads `.cursor-plugin/plugin.json` and `mcp.json`. Claude Code reads `.claude-plugin/plugin.json` and `.mcp.json`. Skills and commands are shared.

From the repo root, in Cursor:

```bash
agent --plugin-dir .
```

To keep it across Cursor windows, copy this directory to `~/.cursor/plugins/local/steady` and reload Cursor. A symlink from that folder to a repo elsewhere is ignored.

If Steady is already configured under Cursor MCP settings, disable that entry while this plugin is enabled so the tools are not listed twice.

In Claude Code:

```bash
claude --plugin-dir .
```

Check the manifest with `claude plugin validate .`

In Claude Desktop, **Customize → Plugins → Add → Add marketplace** and enter `hous3-digital/steady-cursor-plugin`. That flow reads `.claude-plugin/marketplace.json` on the repository’s default branch, then lists the Steady plugin. **Upload plugin** is the other path: it takes a zip and does not use the marketplace file.

## Using it

```
/steady-status
```

```
/steady-findings high and overdue
```

```
/steady-finding F-001
```

Comments go to the finding thread, visible to the client and to Steady. `/steady-comment` posts only the text you supply, and only when that finding currently allows a comment.

Reports from `/steady-report` are metadata (template, status, page count). The PDF itself is not returned.

## Tools

| Tool | What it does |
| --- | --- |
| `list_pentests` | Engagements on the account |
| `get_pentest` | One engagement, including its findings and team |
| `list_findings` | Findings, filterable by severity and status |
| `get_finding` | One finding, including evidence and the thread |
| `get_report` | Report metadata |
| `add_comment` | Post a comment on a finding |
