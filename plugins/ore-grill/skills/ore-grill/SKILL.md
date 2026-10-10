---
name: ore-grill
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it. Treat an existing design as a starting point, not a constraint: reworking it is on the table when the goal justifies the cost.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask exactly one question per round: the frontier question whose answer reshapes the tree the most (the most upstream, highest-impact one). Number questions consecutively across rounds and give your recommended answer. Then wait for the user's answer before the next round.

Format a round like so:

```
❓ **Q<n>** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact you can look up, locally or online, dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; meanwhile ask a frontier question that doesn't depend on it. The exception is a fact a decision needs that only a heavy investigation can produce, e.g. one that can't proceed without the user's help or that requires building something: pause the grilling, propose the investigation, and once the user agrees, finish it before the next round. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
