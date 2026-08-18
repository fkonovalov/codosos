# codosos

A small kit of must-have agent skills. Works in Claude Code (as a plugin)
and in any agent that reads the open [Agent Skills](https://agentskills.io)
format (Codex CLI, Cursor, OpenCode, and others) via `npx skills`.

## Skills

### /bro

The agent answered in dense jargon. `/bro` makes it say the same thing like a human.

**Example:**

> **Agent:** The latency regression correlates with N+1 query amplification in the ORM layer.
>
> **You:** `/bro`
>
> **Agent:** The page is slow because we hit the database once per item instead of once for all of them.

### /tldr

Too long, didn't read. `/tldr` compresses the agent's last wall of text (or anything you point it at) into something you can read in 15 seconds.

**Example:**

> **Agent:** *(40 lines about a failed deploy)*
>
> **You:** `/tldr`
>
> **Agent:** Deploy failed on migration 042. Already rolled back, prod is fine. Fix: rename the index, re-run.

### /handoff

Done for today? `/handoff` saves everything the next session needs into `<repo>/.handoffs/`: state, decisions, next steps. Close the laptop.

### /next-steps

Fresh session, zero context. `/next-steps` reads the latest handoff and tells you what to do now. A short numbered list, no recap.

**Example:**

> **You:** `/next-steps`
>
> **Agent:** 1. Finish retry logic in `api/client.ts` 2. Fix flaky `auth.test.ts` 3. Open PR against `release/2.4`

`handoff` writes, `next-steps` reads: together your work survives the context window.

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
