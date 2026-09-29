---
name: ore-pr-patrol
description: Reviews and triages the PRs in the given GitHub org that request your review. Meant to be run periodically, e.g. `/loop 30m /ore-pr-patrol:ore-pr-patrol <org>`.
argument-hint: <org>
---

- Take one org from the arguments: `$ARGUMENTS`. If none or more than one is given, stop with an error.
- Start everything you post on a PR, including the review body, with `🤖 AI review (ore-pr-patrol)`.
  - Call the threads you start this way "your threads".
- List the PRs in the org that meet all of these:
  - The PR is open and ready for review (not a draft).
  - You are directly requested as a reviewer.
  - The PR doesn't have the `review:must` label.
  - You don't have a pending review on it.
- Skip the PRs in repositories missing either the `review:must` or the `review:skip` label.
- For each remaining PR:
  - Run `ore-pr-triage:ore-pr-triage` on the PR. Call its result (`review:must` or `review:skip`) "the verdict".
  - Check out the PR in a temporary directory.
  - If the PR already has your threads from an earlier run, treat each as settled if either holds:
    - The current code fixes it.
    - A reply explains why no fix is needed, and you agree.
  - Run `ore-code-review:ore-code-review` on the PR's changes in the checkout.
  - Create a review:
    - Add each new finding (one not in your threads) as an inline comment on its line. For a line outside the diff, use the most related line in the diff and name the actual location.
    - Answer each reply on your threads that you disagree with and haven't answered yet, explaining why the finding still stands.
  - If the verdict is **review:skip**, submit the review:
    - Approve if both hold:
      - There are no new findings.
      - All your threads are settled.
    - Otherwise, request changes.
  - If the verdict is **review:must**, leave the review pending.
  - Set the verdict as a label on the PR. Remove the other verdict label if present.
- Finally, report to the user:
  - Which PRs were processed and their verdicts.
  - Which repositories lack the labels, asking the user to create them.
