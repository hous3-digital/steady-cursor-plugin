---
name: steady
description: Look up Steady pentests, findings, remediation deadlines, and report metadata, and post a comment on a finding when the user asks. Use when the user mentions Steady, a pentest, a finding code, retest, remediation, overdue findings, or a Steady report.
---

# Steady

Read Steady through its MCP tools, then answer from that response. Field names and the values you can pass are in [reference.md](reference.md).

## Before you answer

1. Call the tool that matches the question. Do not answer from memory of an earlier chat if the user is asking what is true now.
2. If a code is ambiguous, list the candidates and ask which one. Finding codes and pentest codes are different namespaces (`F-001` vs `PT-003`).
3. Reply in the user's language. Prefer Steady's own labels over a translation you made up.

## Engagements

Use this when someone asks what is in flight, scheduled, or waiting.

1. Call `list_pentests`.
2. Group by `situation` (or `phase` if you need a stable bucket): waiting on Steady, scheduled, in progress, retest, completed.
3. For one engagement, call `get_pentest` with its `code`. Use `stageLabel`, the window, `progressPct` when present, open findings by severity, and the team. Scope arrays are often empty; an empty array means Steady has no scope lines stored, not that you should infer scope.

## Findings

Use this for triage, a single finding, overdue work, or "what is still open".

1. Call `list_findings`. Pass `severity` and `status` only when the user narrowed the list. Known severity values: `critical`, `high`, `medium`, `low`, `informational`. Known status values: `open`, `in_remediation`, `awaiting_retest`. If a filter returns nothing, say so and show the unfiltered list rather than guessing another enum.
2. Lead with overdue items (`overdue: true`) and higher severity. Include `code`, title, `severityLabel`, `statusLabel`, asset, pentest code, and `deadlineLabel`.
3. The account `policy` object is the remediation window in days per severity. The deadline that matters for a finding is its own `dueAt` / `deadlineLabel`.
4. For one finding, call `get_finding` with its `code`. Summarize description, impact, recommendation, CVSS, CWE, and OWASP only from the payload. Walk `steps` in order. For each evidence item, give `title` and `kind`; include `content` only when it is present and `masked` is false. Skip masked content. Mention `history` and the latest `conversation` turn when they change the status of the question.
5. `riskAcceptance` null means the finding is not recorded as risk-accepted.

## Comments

Post only when the user explicitly asks to comment, on a specific finding, with the wording to send (or wording they have already approved).

1. `get_finding` for that code.
2. Stop if `allowedActions` does not include `comment`. Say the finding does not currently accept a comment.
3. Call `add_comment` with that `code` and the approved `body`.
4. Call `get_finding` again and confirm the body is in `conversation` before telling the user it was posted. Comments are visible to both the client and Steady sides.

`start_remediation`, `request_retest`, and `accept_risk` are app actions. Point the user at Steady for those. Do not describe them as done.

## Reports

1. Call `get_report` with no arguments to list reports.
2. Call `get_report` with an `id` from that list for one report.
3. Report `templateLabel`, `status`, `publishedAt`, `pageCount`, and the pentest code and target. Status `published` with a null body is still metadata only.

## How to present it

Keep the answer short enough to scan.

- One engagement: code, target, kind, window, stage, open findings by severity.
- A list of findings: one line each, overdue first.
- One finding: title and code, severity and CVSS, status and deadline, impact, recommendation, then evidence titles. Offer the reproduction steps only when the user asks how it was shown.
