# Steady for Cursor

Work with [Steady](https://app.estady.com.br) pentests, findings, and reports from Cursor. The plugin connects Cursor to the Steady MCP server and adds a skill, a rule, and commands so the agent reads Steady instead of guessing.

## What you get

- **MCP connection** to `https://app.estady.com.br/api/mcp`.
- **Skill** `steady` — engagements, findings, comments, and report metadata.
- **Commands**: `/steady-status`, `/steady-findings`, `/steady-finding`, `/steady-comment`, `/steady-report`.
- **Rule** that keeps answers tied to tool responses: no invented findings, masked evidence stays masked, and comments are posted only when asked.

## Load it locally

This repo is the plugin. From the repo root:

```bash
agent --plugin-dir .
```

To keep it across windows, copy this directory to `~/.cursor/plugins/local/steady` and reload Cursor. A symlink from that folder to a repo elsewhere is ignored.

If Steady is already configured under Cursor MCP settings, disable that entry while this plugin is enabled so the tools are not listed twice.

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
