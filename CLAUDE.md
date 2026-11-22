# codex

Personal Codex instructions and Claude Code interoperability.

## Commands

No project commands are defined. Claude can be consulted with `claude -p`; keep the call
read-only, as described in `.agents/skills/claude-bridge/SKILL.md`.

## Layout

- `AGENTS.md`: Codex role and review instructions.
- `.agents/skills/`: Codex skills, each linked user-wide into `~/.agents/skills` (README):
  `code-review`, `adr-critique`, `root-cause`, `security-audit`, `docs-writer`,
  `git-flow`, `claude-bridge` (read-only Claude consultation).
- `plugins/claude-bridge/`: notes on future Codex plugin packaging.
- `logs/`: Claude CLI exchange log.

## Conventions

- Codex may use the full Git flow only in this repo; see `AGENTS.md` and the `git-flow`
  skill.
- The skill links in `~/.agents/skills` point into this checkout, so Codex loads whatever
  branch is checked out: leave the repo on `main` when done.
