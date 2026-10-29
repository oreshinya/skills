---
name: ore-impl
description: Turns a settled plan or direction into an implementation. Trigger when a settled plan is to be implemented: the user asks for it, e.g. "implement it" or "go ahead", or a skill or a loop hands you a plan to carry out.
---

- Delegate edits that involve a substantial amount of file reading and writing or a lot of trial and error to a `general-purpose` subagent with `effort: medium` (leave `model` unset), and make edits you know will be small directly.
- When delegating, pass the full plan as-is, without summarizing it.
- Whether you edit directly or delegate, run the relevant verification such as lint and tests, and fix issues until they pass.
- Then run what you changed for real and confirm it works as intended. If something needed for that is missing, do not install it yourself; report what is missing.
