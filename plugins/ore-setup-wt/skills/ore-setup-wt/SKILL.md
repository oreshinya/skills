---
name: ore-setup-wt
description: Configures Worktrunk (`wt`) for the current repository so that several worktrees can run at once without colliding on ports, databases, queues or missing local files.
disable-model-invocation: true
---

Make worktrees of this repository usable side by side: a new worktree is ready to run right after creation, and its dev processes do not collide with those of other worktrees. The result is a section for this repository in the Worktrunk user config. Change the repository only to make a colliding value overridable.

Load the `worktrunk:worktrunk` skill for config syntax before writing any config.

## Investigate

Do not read the contents of secret files; their names are enough.

1. Setup: the commands a fresh checkout needs before it runs, such as dependency install, database creation and migration.
2. Local files: the gitignored files it needs that cannot be regenerated, such as env files, certificates and credentials.
3. Entry points: the commands that need the middleware or the local files, such as the dev server, migrations, tests and a console.
4. Collisions: everything two worktrees running at once would share, such as listen ports, URLs derived from those ports, database names, and queue or cache keys in middleware. For each, find the environment variable that overrides it, and confirm in the code that reads it, framework code included, that setting it for the entry point changes that value and nothing else. When there is none, the repository needs a change.
5. Repository-wide scanners: tools that walk the whole repository tree, such as file watchers, linters, test runners and container volume mounts. Check whether each one would pick up a directory under `.claude/worktrees/`.

## Decide

- Middleware (Docker Compose services) is shared by all worktrees. Separate worktrees inside it with a namespace, key prefix or database name, not with extra containers or host ports.
- Derive ports with `hash_port` from the repository and the branch, since other repositories use the same branch names: `{{ (repo ~ '-' ~ branch) | hash_port }}`. Give each additional server its own, e.g. `{{ (repo ~ '-assets-' ~ branch) | hash_port }}`.
- Define one alias per entry point that forwards `{{ args }}`, named `ore-` plus the command's own name: `bin/rails` → `ore-rails`, `pnpm dev` → `ore-dev`. Agents run these aliases outside the sandbox, so define none that runs an arbitrary command.
- Pass per-worktree environment variables only through the aliases. Do not export them; a variable such as a database name may also affect tests.
- In the primary worktree the aliases add no variables, so the repository's defaults apply.
- Each worktree gets its own development database, and its own test database when tests would otherwise share one. On a database server, name it with `sanitize_db`, create it in `pre-start` and drop it in `pre-remove`.
- A database starts empty and is built by the repository's own setup: schema load or migration, then seeds if any. Do not copy data from another worktree.
- Copy only the local files found above. Reinstall dependencies instead of copying them. Do not copy `shell.nix`.
- Include a copied file, hook command, alias, variable or repository change only when you can name what fails without it. Being gitignored does not make a file needed.
- Place worktrees under `.claude/worktrees/` with `worktree-path = ".claude/worktrees/{{ branch | sanitize }}"`. Claude Code agents can enter an existing worktree only there. Choose it only when the scanners found above ignore that directory. Propose adding `.claude/worktrees/` to the user's global gitignore, not the repository's.
- Label each hook command `<verb>-<object>`. The label is how a failed step is reported.
- Where a colliding value has no override, change the repository so that an environment variable overrides it and the current value stays the default.

## Apply

1. Review the plan once on your own. Then present the hooks, the aliases and the repository changes, each with what fails without it. Continue only after the user confirms.
2. Make the repository changes and leave them uncommitted.
3. Write the hooks and aliases under `[projects."<identifier>"]` in the user config. `wt config show` prints the path of the user config and the identifier. Replace the section if it already exists, and leave the rest of the file as is.
4. Preview the result with the dry-run options of `wt hook` and `wt config alias`.
5. Tell the user that worktrees get the repository changes only once these are committed to the branch worktrees are created from.

Shape of the result:

```toml
[projects."github.com/acme/app"]
worktree-path = ".claude/worktrees/{{ branch | sanitize }}"
aliases.ore-rails = "{% if worktree_path != primary_worktree_path %}PORT={{ (repo ~ '-' ~ branch) | hash_port }} DB_NAME=app_dev_{{ branch | sanitize_db }} REDIS_NAMESPACE={{ branch | sanitize }} {% endif %}bin/rails {{ args }}"
aliases.ore-rspec = "{% if worktree_path != primary_worktree_path %}DB_NAME=app_test_{{ branch | sanitize_db }} {% endif %}bin/rspec {{ args }}"

[[projects."github.com/acme/app".pre-start]]
copy-env = "cp {{ primary_worktree_path }}/.env ."
install-deps = "bundle install"

[[projects."github.com/acme/app".pre-start]]
setup-db = "DB_NAME=app_dev_{{ branch | sanitize_db }} bin/rails db:prepare"
setup-test-db = "RAILS_ENV=test DB_NAME=app_test_{{ branch | sanitize_db }} bin/rails db:prepare"

[[projects."github.com/acme/app".pre-remove]]
drop-db = "DB_NAME=app_dev_{{ branch | sanitize_db }} bin/rails db:drop"
drop-test-db = "RAILS_ENV=test DB_NAME=app_test_{{ branch | sanitize_db }} bin/rails db:drop"
```
