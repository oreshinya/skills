---
name: ore-impl
description: Changes code or configuration. Trigger whenever you do implementation work, even within another skill's procedure.
---

- Delegate edits that involve a substantial amount of file reading and writing or a lot of trial and error to a `general-purpose` subagent with `effort: medium`, and make edits you know will be small directly.
- When delegating, pass the full plan as-is, without summarizing it.
- Whether you edit directly or delegate, run the relevant verification such as lint and tests, and fix issues until they pass.
- Then run what you changed for real and confirm it works as intended. If something needed for that is missing, do not install it yourself; report what is missing.
