---
name: ore-pr-triage
description: Judges whether a given PR needs careful human review, a quick skim, or can be skipped entirely. Trigger when the user asks you to triage a PR, or right after a PR is created.
---

- Resolve the PR reference from the user's input: a PR number, `owner/repo#number`, or a full URL. If only a number is given, resolve the owner/repo from the current directory's git remote.
- Look at the PR's diff, changed files, and the target repository's CLAUDE.md (if any).
- Classify the PR as one of **must-review**, **skim**, or **skip**.
  - **must-review**, regardless of anything else, if the change touches:
    - Authentication/authorization, payments/billing, data migrations, or persisted schema formats.
    - Public API compatibility.
    - Production infrastructure or deployment configuration (CI/CD, IaC).
    - Anything the target repository's CLAUDE.md marks as sensitive or critical.
  - **skip**, unless the above applies, if the change is limited to:
    - Lockfile-only updates, generated code or mocks, pure renames, formatting-only changes, or typo/non-functional fixes.
  - Otherwise, weigh diff size, complexity, number of files/modules touched, and whether tests were added or updated to decide between **skim** and **must-review**. When uncertain, default to **must-review**.
  - Never factor in who authored the change.
- Report the verdict and the specific criteria that drove it, concisely.
