---
name: steady-comment
description: Post a comment on a Steady finding after the user has given the code and the exact text.
---

# Comment on a Steady finding

Follow the Steady skill. The user must have given both the finding code and the comment text.

1. If either is missing, ask for it. Do not draft and post in the same step.
2. Call `get_finding`. Stop if `comment` is not in `allowedActions`.
3. Call `add_comment` with the code and the user's text.
4. Call `get_finding` again and confirm the text is in `conversation` before reporting that it was posted.
