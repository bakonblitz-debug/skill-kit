# skill-kit

Tools for writing your own [Claude Code](https://claude.com/claude-code) skills, as a plugin: list every installed plugin's skills, turn run logs for skill scripts on and off, lint the skill rules, and create a new skill from a one-line explanation.

Works in plain Claude Code. No other harness needed. Scripts are Python 3.9+, standard library only.

## Install

```
/plugin marketplace add bakonblitz-debug/skill-kit
/plugin install skill-kit@skill-kit
```

Or from a local clone (edits are live in the next session):

```
git clone https://github.com/bakonblitz-debug/skill-kit
/plugin marketplace add /path/to/skill-kit
/plugin install skill-kit@skill-kit
```

## Commands

| Command | What it does |
|---|---|
| `/skill-kit:skills list` | Every installed plugin's skills, one table per plugin, with whether each plugin is on here. |
| `/skill-kit:skills log [on\|off\|tail] [name]` | Run logs for skill scripts: off by default, kept per machine in `~/.claude/skill-logs/`. |
| `/skill-kit:skills lint [plugin dir ...]` | Checks the skill rules below; exits 1 on a failure. No directory = every plugin you installed from a local folder. |
| `/skill-kit:create "<what it should do>"` | Creates a new skill in the plugin whose subject fits: SKILL.md, a tested Python script where the skill does work, the listing, the lint. |
| `/skill-kit:list` | This plugin's commands, generated from the skill files. |

## The skill rules (what the lint checks)

- Every plugin has a `list` skill, so its commands are discoverable.
- Skill code is Python (deterministic, testable, cheaper than having the model compose shell each time); `scripts/py.sh` picks a working Python 3.9+, and is the one shell file allowed.
- Every skill script has `--selftest` and logs through `scripts/skilllog.py` (a no-op while logging is off).
- A SKILL.md holds no shell logic (pipes, `$( )`, loops): that belongs in a script.
- A description opens with quoted trigger phrases, then one sentence (a warning only).
- A plugin marked public (an empty `.claude-plugin/PUBLIC` file) carries no one workspace's specifics: home paths, machine names, local tools or model names, or owner pronouns. A line ending in `public-ok` is exempt.

Claude's own plugin copies (`~/.claude/plugins/`) are never linted or edited: they are replaced on update.

## Develop

```
git config core.hooksPath .githooks
```

The pre-commit hook runs the lint on this plugin and `.githooks/commit-guard.py` (lost exec bits, CRLF). `bash scripts/py.sh scripts/skill.py --selftest` tests the lint and logs.

## License

MIT, see [LICENSE](LICENSE).
