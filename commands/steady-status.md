---
name: steady-status
description: Summarize Steady pentests that are waiting, scheduled, in progress, or in retest.
---

# Steady status

Follow the Steady skill.

1. Call `list_pentests`.
2. Group engagements by `situation`.
3. For anything in progress or in retest, include the window, `stageLabel`, and open findings by severity.
4. Name overdue findings only after `list_findings` confirms them. Do not invent counts.
