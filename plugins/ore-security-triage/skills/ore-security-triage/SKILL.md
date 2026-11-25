---
name: ore-security-triage
description: Fact-checks security review findings against the code and infrastructure, and sorts them by what a small team should do about them.
argument-hint: <which security reviews to triage, in your own words>
disable-model-invocation: true
---

For each finding in the given security reviews, establish the facts that decide its category. Look them up yourself in the code, IaC, chat, public docs, and past records. Turn any live environment you can't reach from here into read-only scripts for the user to run. Ask the user only what is still unknown. Then, without guessing, sort the finding into a category below.

## Criteria

- **Direct hole**: a defect through which someone acting beyond the access we gave them does real harm to us, our users, or others. Taking a feature down or running up costs counts only when one attacker can do it from one source, and is at most "Start, but no rush" unless a few requests suffice.
- **Precondition**: something the attacker can't arrange alone, such as a victim's unusual action, a privileged account few hold, or writing an exploit for a CVE with no public one.

- **Start right away**: a direct hole with no precondition, unless its harm reaches only a few or is light.
- **Start, but no rush**: any other direct hole.
- **Start if you have time**: not a direct hole, but worth doing.
- **Won't start**: everything else.

- Never let fix cost affect the category.
- A missing defense layer is not a hole while another one holds. A password alone doesn't hold where guessing is unlimited, or on accounts that control all users' data or our infrastructure.
- A CVE is a hole only when untrusted input reaches the vulnerable code. Presume it does for one in CISA KEV or with a public PoC.
- Infrastructure lowers the category only if it reliably stops the attack.
- Never put a finding in "Won't start" on unverified grounds.
- Keep fixes for "Start right away" and "Start, but no rush" to the smallest change that removes the hole. If that takes long for a "Start right away" hole, put a quick interim measure in "Start right away" and the fix in "Start, but no rush".

## Safety

Only read live environments. Don't run code from the repositories. Never fetch or use secret values.

## Output

Write two Markdown files in the user's language: "Start right away" and "Start, but no rush" as the action list, "Start if you have time" and "Won't start" as reference. Order by risk. For each item, give its exact location (file and line, or cloud resource), what can happen, the evidence, and the steps to take, so a reader can act on it alone. Keep it terse.
