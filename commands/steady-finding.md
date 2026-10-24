---
name: steady-finding
description: Open one Steady finding by its code and summarize it from the API.
---

# Steady finding

Follow the Steady skill. The argument is a finding code such as `F-001`.

1. If the argument is missing, ask for the code instead of guessing.
2. Call `get_finding` with that code.
3. Summarize title, severity, CVSS, status, deadline, impact, and recommendation from the payload. List evidence by title. Include evidence content only when it is present and not masked.
