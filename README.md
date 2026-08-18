# codosos

A small kit of must-have agent skills. Works in Claude Code (as a plugin)
and in any agent that reads the open [Agent Skills](https://agentskills.io)
format (Codex CLI, Cursor, OpenCode, and others) via `npx skills`.

## Skills

| Skill | What it does |
|---|---|
| `bro` | Restates the last message in plain human language, no jargon. Manual only: `/bro`. |
| `tldr` | Compresses a wall of text into a tight summary. |
| `handoff` | Saves a handoff document to `<repo>/.handoffs/` so a fresh session can pick up the work. Manual only: `/handoff`. |
| `next-steps` | Reads the latest handoff and lists concrete next actions. |

`handoff` and `next-steps` are a pair: one writes, the other reads.

## Install

### Claude Code

```bash
claude plugin marketplace add https://github.com/fkonovalov/codosos.git
claude plugin install codosos@sos
```

Skills appear as `/codosos:bro`, `/codosos:tldr`, etc. Bare `/bro` also works
while the name is unambiguous. Scope it to a single project with
`claude plugin install codosos@sos --scope project` from the repo root.

Or bootstrap declaratively in `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "sos": { "source": { "source": "url", "url": "https://github.com/fkonovalov/codosos.git" } }
  },
  "enabledPlugins": { "codosos@sos": true }
}
```

### Other agents (Codex CLI, Cursor, ...)

```bash
npx skills add https://github.com/fkonovalov/codosos
```

Pick target agents interactively, or pass them explicitly:
`npx skills add <url> --skill '*' -a codex -g -y`. In Codex, invoke with
`$bro`, `$tldr`, and so on.

## Update

- Claude Code: `claude plugin marketplace update sos`
- npx skills: `npx skills update -g`

## Release

```bash
scripts/bump-version.sh 0.2.0   # syncs version across all manifests
scripts/bump-version.sh --check # detect version drift
```

Add a `CHANGELOG.md` entry per release. Lint shell scripts with
`scripts/lint-shell.sh` (requires shellcheck).

Besides `.claude-plugin/`, the repo ships manifests for other agents:
`.codex-plugin/`, `.cursor-plugin/`, and `.agents/plugins/marketplace.json`.
All of them point at the same `skills/` directory; keep versions in sync via
the bump script (`.version-bump.json` lists every versioned file).
