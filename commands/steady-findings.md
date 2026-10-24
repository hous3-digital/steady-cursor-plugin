---
name: steady-findings
description: List Steady findings, overdue first. Optional severity or status filter in the arguments.
---

# Steady findings

Follow the Steady skill. The arguments are an optional filter, such as a severity, a status, or a pentest code.

1. Call `list_findings` with `severity` or `status` when the argument names one. If it names a pentest code, list findings and keep the rows for that pentest.
2. Put overdue findings first.
3. One line per finding: code, title, severity label, status label, asset, pentest code, deadline label.
