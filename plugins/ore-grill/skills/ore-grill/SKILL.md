---
name: ore-grill
description: Relentlessly interrogates the user about a plan, decision, or idea. Trigger for stress-testing one's thinking, or on phrases like "grill me" or "poke holes in this".
---

Interview the user relentlessly until you reach a shared understanding. Treat the subject as a **design tree**: each decision branches into the next decisions that hang off it.

Before the first round, investigate the subject: launch subagents to gather the facts obtainable from the environment (related code, files, existing mechanisms, and so on), and wait for their reports. Treat the existing design as a starting point, not a constraint: include reworking it where the goal justifies the cost.

Work through the tree in **rounds**. The **frontier** is the set of decisions whose prerequisite decisions are already settled — that is, the questions you can ask *right now* without guessing at answers you haven't heard yet. In each round, ask **at most 3 questions**, chosen from the current frontier. Prefer the questions whose answers change the shape of the tree the most (the most upstream, highest-impact ones). Number each question and attach your own recommended answer. Then wait for the user's answers before moving on to the next round.

Present each round in the following format.

```
❓ **Q1** - **<question title>**: <question body; may span multiple paragraphs and include options>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body; may span multiple paragraphs and include options>

➡️ <your recommended answer>
```

Every answer the user gives changes the shape of the tree. Once a decision is settled, the frontier moves outward and the questions that depended on it are unlocked. Recompute the frontier and ask the next round. A question whose answer depends on another question that is still unresolved within the same round belongs to a *later* round, not this one.

Establishing **facts** is always your job, never the user's. If a later question needs a fact that the initial investigation did not cover and that can be obtained from the environment (the file system, tools, and so on), launch a subagent to look it up. Never ask the user something you can find out yourself. But do not stop to wait for it: an investigation in progress is an "unresolved prerequisite", and only the questions downstream of it need to wait for the subagent's report. Ask the rest of the frontier right away. **Decisions** belong to the user. Put every decision to the user and wait for the answer.

The session ends when the frontier is empty: every branch of the design tree has been walked and no assumption is left implicit. Do not move on to execution until the user confirms that a shared understanding has been reached.
