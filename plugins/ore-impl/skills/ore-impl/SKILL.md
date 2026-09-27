---
name: ore-impl
description: Turns a settled plan or direction into an implementation. Trigger when, after a plan has been decided, the user asks for implementation, e.g. "implement it" or "go ahead".
---

- Delegate edits that involve a substantial amount of file reading and writing or a lot of trial and error to the `ore-impl:implementer` subagent, and make edits you know will be small directly.
- When delegating, pass the full plan as-is, without summarizing it.
- Whether you edit directly or delegate, run the relevant verification such as lint and tests, and fix issues until they pass.
