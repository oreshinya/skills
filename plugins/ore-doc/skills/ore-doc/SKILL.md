---
name: ore-doc
description: Defines the plain-text plan format for work procedures: how to write, execute and edit one. Trigger whenever work involves such a plan: writing up a settled plan or direction (e.g. "put this into a plan" or "turn this into plan text"), or executing, resuming or editing a plan file (`*.ore-doc.txt`).
---

When writing up a settled plan, write it as a work procedure to be executed by an AI in a separate session with no conversation context. Unless told otherwise, write it to `<slug>.ore-doc.txt` directly under the current directory, and report the path.

The document is one tree of steps: the root is the whole job, and the children of a step are how to carry it out, executed from top to bottom. Express structure with indentation (2 spaces) and blank lines only. Do not use bullet markers, Markdown, or code fences. Every line outside a `memo` is a step. A step is something the executor does, written as an imperative; write a command to run as-is. What the product should do is not a step. Write checks as steps too (e.g. "Confirm that X exists").

Whatever is not a step goes under a `memo` line placed as the first child of the step it applies to: the behavior to build, constraints, things not to do, design details such as code and values, file paths, which steps may run in parallel. Write only what the executor would get wrong without it. Write a reason under a decision only when, without it, someone would likely take the decision for a mistake and undo it.

Keep technical terms in their original form, and write each command and path on a single line as-is. Keep lines short, and leave out anything self-evident. Do not write secrets.

A line containing only `>`, never indented, is the progress bookmark: it sits just below the last finished step, and starts just above the root's first child step.

When executing a plan, start at the step below the bookmark. Before you stop, move the bookmark to just below the last finished step.

When editing a plan, follow this format and leave finished steps as they are.

Write it in the language the user is using in the conversation; only the `memo` label stays as-is.

Sample:

```
Add an audit log for admin operations
  memo
    Scope is apps/admin
    Do not store request bodies

  Add the audit_logs table
    memo
      t.references :admin, null: false
      t.string :action, null: false
      t.string :target, null: false
      t.timestamps
    Write the migration
    bin/rails db:migrate
>

  Implement operation recording
    memo
      app/controllers/admin/base_controller.rb
      Record only create, update and destroy, not views
      bin/rubocop and bin/rails test may run in parallel
    Add code that writes one row to audit_logs after each operation
    Write a test that updating a user adds one row to audit_logs
    Write a test that opening a list adds no row to audit_logs
    bin/rubocop
    bin/rails test

  Verify locally
    Update a user in the admin screen
    Confirm that audit_logs has a new row whose action is update

  Create a PR
```
