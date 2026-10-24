# Steady MCP reference

Server: `https://app.estady.com.br/api/mcp`

Tools appear under the Steady server. In a plugin install the server id is prefixed with the plugin name. Call the tools by the names below.

## list_pentests

No arguments.

Each item:

| Field | Meaning |
| --- | --- |
| `code` | Pentest code, `PT-003` |
| `target` | Asset name |
| `kindLabel` | Surface, such as web app or REST API |
| `window.start`, `window.end` | Engagement dates |
| `phase` | `requested`, `scheduled`, `execution`, `retest`, `completed` |
| `situation` | Short status phrase |
| `stageLabel` | Longer status phrase |
| `knowledgeLevel` | Such as `gray_box` |
| `engagementMode` | Such as `managed` |
| `env` | Such as `staging` |
| `findings.total`, `findings.open`, `findings.bySeverity` | Counts on that pentest |

## get_pentest

Argument: `code`.

Adds `progressPct`, `scope` (`surfaces`, `focus`, `outOfScope`, `rules`, `roles`), `team` (`name`, `role`), `findingRows` (same shape as finding list items), `request`, `adjustment`, `agentWork`, `badge`. Null and empty arrays stay empty.

## list_findings

Arguments, both optional arrays of strings: `severity`, `status`.

Response: `items`, `total`, `open`, and `policy`. `policy` maps severity to remediation days (`critical`, `high`, `medium`, `low`).

Each item: `id`, `code`, `title`, `severity`, `severityLabel`, `cvssScore`, `status`, `statusLabel`, `dueAt`, `overdue`, `daysLeft`, `deadlineLabel`, `detectedAt`, `pentest.code`, `asset.id`, `asset.name`.

Severity: `critical`, `high`, `medium`, `low`, `informational`.

Status values seen: `open`, `in_remediation`, `awaiting_retest`.

## get_finding

Argument: `code`.

Adds `cvssVector`, `cwe`, `owasp`, `description`, `impact`, `recommendation`, `steps` (ordered strings), `evidence`, `history`, `conversation`, `riskAcceptance`, `allowedActions`.

Evidence item: `id`, `kind` (such as `http_trace` or `file`), `title`, `content`, `masked`, `hasFile`. When `masked` is true, `content` is hidden on purpose.

Conversation author: `name`, `side` (`client` or `steady`).

`allowedActions` can include `comment`, `start_remediation`, `request_retest`, `accept_risk`. Only `comment` has an MCP tool.

## get_report

Argument: optional `id`.

Without `id`, returns a list. With `id`, returns one object. Fields: `id`, `pentest.code`, `pentest.target`, `template` (`executive`, `technical_full`), `templateLabel`, `status` (`in_progress`, `published`), `publishedAt`, `pageCount`.

## add_comment

Arguments: `code`, `body`. Posts into the finding conversation. Confirm with a fresh `get_finding`.
