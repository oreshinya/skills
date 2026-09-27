---
name: ore-parking-lot
description: Parks a thought that is unrelated to what's currently being discussed, so it doesn't derail the current topic, then raises it later at a natural break.
disable-model-invocation: true
---

Keep an ordered queue of parked thoughts in this conversation's context.

- **Add**: on invocation with content, append it to the queue verbatim, acknowledge in one line, then continue the answer already in progress unchanged.
- **Raise**: once the topic in focus settles, bring up the oldest queued item yourself as the next topic and drop it from the queue.
- **Remind**: while the queue is non-empty, end every response with a numbered list of it, in the language the user is using in the conversation (translate the heading accordingly), e.g.:

  ```
  ---
  Parked topics
  1. <item>
  2. <item>
  ```

  Renumber the list from 1 each time you show it. Keep this list separate from any other pending-items list in the conversation — they track different things.
