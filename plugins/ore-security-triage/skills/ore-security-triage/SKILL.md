---
name: ore-security-triage
description: Fact-checks security review findings against the code and infrastructure, and sorts them by what a small team should do about them.
argument-hint: <which security reviews to triage, in your own words>
disable-model-invocation: true
---

Fact-check each finding in the given security reviews against the code, IaC, and live environment, then sort it into a bucket. Don't judge on guesses: investigate, and ask the user what you can't check.

## Criteria

- **Direct hole**: a defect through which someone acting beyond the access we gave them does real harm to us, our users, or others. Taking a feature down or running up costs counts only when one attacker can do it from one source.
- **Precondition**: something the attacker can't arrange alone, such as a victim's unusual action, a privileged account few hold, or writing an exploit for a CVE with no public one.

1. **Start right away**: a direct hole with no precondition, unless its harm reaches only a few or is light.
2. **Start, but no rush**: any other direct hole.
3. **Start if you have time**: not a direct hole, but worth doing.
4. **Won't start**: everything else.

- Never let fix cost affect the bucket.
- A missing defense layer is not a hole while another one holds. A password alone doesn't hold where guessing is unlimited, or on accounts that control all users' data or our infrastructure.
- A CVE is a hole only when untrusted input reaches the vulnerable code. Presume it does for one in CISA KEV or with a public PoC.
- Infrastructure lowers the bucket only if it reliably stops the attack.
- Never put a finding in bucket 4 on unverified grounds.
- Keep bucket 1 and 2 fixes to the smallest change that removes the hole. If that takes long for a bucket 1 hole, put a quick interim measure in bucket 1 and the fix in bucket 2.

## Safety

Only read live environments. Don't run code from the repositories. Never fetch or use secret values.

## Output

Write two Markdown files in the user's language: buckets 1 and 2 as the action list, buckets 3 and 4 as reference. Order by risk. For each item, give its exact location (file and line, or cloud resource), what can happen, the evidence, and the steps to take, so a reader can act on it alone. Keep it terse.
