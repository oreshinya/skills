---
name: ore-pr-triage
description: Judges whether a given PR needs human review (`review:must`) or can skip it (`review:skip`). Trigger when the user asks you to triage a PR, or right after a PR is created.
---

- Resolve the PR reference from the user's input: a PR number, `owner/repo#number`, or a full URL. If only a number is given, resolve the owner/repo from the current directory's git remote.
- Look at the PR's diff, changed files, and the target repository's CLAUDE.md (if any).
- Classify the PR as **review:must**, regardless of anything else, if the change touches any of these:
  - Authentication or authorization.
  - Payments or billing.
  - Data migrations or persisted schema formats.
  - Public API compatibility.
  - Production infrastructure or deployment configuration (CI/CD, IaC).
  - Agent instruction files, such as CLAUDE.md, AGENTS.md, or anything under `.claude/`.
  - Anything the target repository's CLAUDE.md marks as sensitive or critical.
- Otherwise, classify it as **review:skip** only if every change falls into one of these categories:
  - Documentation or comment-only changes, excluding directive comments.
  - Typo fixes in user-facing text.
  - Formatting-only changes.
  - Pure renames.
  - Regenerated code or mocks.
  - Library updates that change only dependency manifests and lockfiles.
  - Deletions of code or assets that nothing references anymore, confirmed by searching the repository for their names and paths.
- Classify every other PR as **review:must**. When uncertain, choose **review:must**.
- Report the verdict and the criteria that drove it, concisely.
