# skills

Custom skills and agents for [Claude Code](https://code.claude.com/docs), distributed as Claude Code plugins.

## Installation

Add this repository as a marketplace, then install the plugins you want:

```
/plugin marketplace add oreshinya/skills
/plugin install <plugin>@oreshinya
```

Installed skills and agents are available as `<plugin>:<name>`.

## Plugins

### ore-skills

General-purpose workflow skills, plus the agents they depend on.

| Skill | Description |
| --- | --- |
| `ore-code-review` | Runs problem-finding agents in parallel and reports the narrowed-down findings with suggested fixes |
| `ore-deps` | Updates project dependencies to the latest stable versions |
| `ore-doc` | Writes a settled plan out as a plain-text plan document |
| `ore-grill` | Relentlessly interrogates you about a plan, decision, or idea |
| `ore-impl` | Turns a settled plan into an implementation |

| Agent | Used by |
| --- | --- |
| `bug-finder`, `complexity-issue-finder`, `rule-violation-finder`, `performance-issue-finder`, `security-issue-finder` | `ore-code-review` |
| `implementer` | `ore-impl` |

## License

[MIT](LICENSE)
