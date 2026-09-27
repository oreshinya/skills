---
name: ore-code-review
description: Reviews code by running custom problem-finding agents in parallel, then reports the narrowed-down findings with suggested fixes. Trigger when the user asks for a review, e.g. "review this" or "code review".
---

- Follow the user's specification for the review target. If none is given, use the changes on the current branch (including uncommitted changes). If that is also empty, decide the scope from context.
- Launch the following custom agents in parallel, passing each of them the review target.
  - `ore-code-review:bug-finder`
  - `ore-code-review:complexity-issue-finder`
  - `ore-code-review:rule-violation-finder`
  - `ore-code-review:performance-issue-finder`
  - `ore-code-review:security-issue-finder`
- Verify each returned finding against the code yourself, and withdraw false findings and any that are optional to fix (fine to address or ignore). Merge findings that point to the same problem into one.
- For each remaining finding, work out how to address it.
- For each finding, output the following to the user: the target file (and line), a description of the problem, and how to address it. Do not output withdrawn findings.
