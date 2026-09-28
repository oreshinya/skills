# skills

Custom skills and agents for [Claude Code](https://code.claude.com/docs), distributed as Claude Code plugins.

## Installation

Add this repository as a marketplace, then install the plugins you want:

```
/plugin marketplace add oreshinya/skills
/plugin install <plugin>@oreshinya
```

Installed skills and agents are available as `<plugin>:<name>` (for example, `ore-code-review:bug-finder`).

## Plugins

Each skill is its own plugin, so you can install only the ones you need.

| Plugin | Description | Bundled agents |
| --- | --- | --- |
| `ore-code-review` | Runs problem-finding agents in parallel and reports the narrowed-down findings with suggested fixes | `bug-finder`, `complexity-issue-finder`, `rule-violation-finder`, `performance-issue-finder`, `security-issue-finder` |
| `ore-deps` | Updates project dependencies to the latest stable versions | |
| `ore-doc` | Writes a settled plan out as a plain-text plan document | |
| `ore-grill` | Relentlessly interrogates you about a plan, decision, or idea | |
| `ore-impl` | Turns a settled plan into an implementation | `implementer` |
| `ore-parking-lot` | Parks a thought that's unrelated to what's currently being discussed and raises it later at a natural break | |
| `ore-pr-patrol` | Reviews and triages PRs requesting your review in a GitHub org (depends on `ore-code-review` and `ore-pr-triage`) | |
| `ore-pr-triage` | Judges whether a PR needs human review (`review:must`) or can skip it (`review:skip`) | |

## License

[MIT](LICENSE)
