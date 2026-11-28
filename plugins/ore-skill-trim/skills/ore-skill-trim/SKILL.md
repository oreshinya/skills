---
name: ore-skill-trim
description: Trims a skill's body down to the instructions that change how an agent acts. Trigger when asked to clean up or trim a skill.
---

- Rewrite the skill's body into a tree of only the elements that change how an agent acts.
  - Rules for the rewrite
    - Take the reader to be the agent that uses the skill.
    - With a diff, trim only the elements it added or changed.
      - Trim unchanged elements only where the change made them unnecessary.
    - Replace anything written more specifically than its purpose with that purpose if the agent could achieve the purpose from the situation without it.
      - Keep it if it was written that way deliberately.
      - Mark it undecided if the body does not settle whether it was deliberate.
  - Steps for the rewrite
    - Outline and trim the body with a subagent that has no context from the conversation.
      - Give the subagent only the body, the file's uncommitted `git diff` if there is one, the rules for the rewrite, and these steps.
        - Outline the body with `ore-outline:ore-outline`.
        - Trim each element of the outline by the rules for the rewrite.
    - Decide each undecided element from the conversation where the skill was written or updated.
      - Treat an element the user asked for as deliberate.
      - Treat an element an agent added on its own as not deliberate.
      - Ask the user about an element that is neither.
