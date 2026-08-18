# codosos

This repository is a kit of agent skills in the open SKILL.md format.
Each skill lives in `skills/<name>/SKILL.md`; frontmatter `name` matches its
directory. `.claude-plugin/` holds the Claude Code plugin and marketplace
manifests; the plugin source is the repo root.

When editing skills, keep frontmatter portable: `name` and `description` are
the universal fields. Claude-only fields (`disable-model-invocation`,
`argument-hint`) are tolerated by other agents but ignored there.

Per-agent manifests (`.claude-plugin/`, `.codex-plugin/`, `.cursor-plugin/`,
`.agents/plugins/`) all point at the same `skills/` directory. On release, run
`scripts/bump-version.sh <X.Y.Z>` to sync versions everywhere (declared in
`.version-bump.json`) and add a `CHANGELOG.md` entry. Lint shell changes with
`scripts/lint-shell.sh`.
