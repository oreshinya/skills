---
name: ore-pr-patrol
description: Patrols open PRs in the configured repositories once, posting code review findings and a triage verdict to each PR. Meant to be run periodically, e.g. `/loop 30m /ore-pr-patrol:ore-pr-patrol`.
disable-model-invocation: true
---

- Read the targets from `~/.claude/ore-pr-patrol.json` (`{"targets": ["owner", "owner/repo"]}`; an owner alone means all of its repositories). If the file is missing, ask the user for the targets and create it.
- List the open, non-draft PRs in the targets, excluding PRs authored by the current GitHub user.
- Skip a PR if its patrol summary comment already records the PR's current head SHA. Otherwise:
  - Check out the PR in a temporary directory, and run `ore-code-review:ore-code-review` and `ore-pr-triage:ore-pr-triage` on it there.
  - Post the code review findings as inline comments in one review. Put findings on lines outside the diff in the review body, and don't repeat findings already posted on the PR. Make the review an approval if the verdict is **skip** and there are no findings; otherwise only comment, never request changes.
  - If it doesn't approve and a previous patrol approval stands, dismiss that approval with a short reason. Never touch approvals not made by patrol.
  - Create or update the single patrol summary comment with the verdict, its reasons, and a line `Triaged at commit <head SHA>`.
  - Label every review, comment, and dismissal message as an automated review by ore-pr-patrol.
  - If the verdict is **must-review**, send a push notification naming the PR.
- Finally, summarize to the user which PRs were processed and their verdicts.
