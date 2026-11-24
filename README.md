# codex

Personal Codex workflow and Claude Code interoperability notes.

Claude Code is installed locally as `claude`. This repo documents the Codex-side workflow
for asking Claude for a read-only second opinion. The sibling `../claude` repo documents
the reverse direction: Claude calls Codex through `codex exec` and `codex review`.

## Setup

Make the skills load in every repo, not only this one:
```
mkdir -p ~/.agents/skills
for d in ~/projects/codex/.agents/skills/*/; do
  n=$(basename "$d")
  if [ -e ~/.agents/skills/"$n" ]; then echo "skip $n: already exists"; else ln -s "$d" ~/.agents/skills/"$n"; fi
done
```
Each skill gets its own link, so other skills in `~/.agents/skills` stay untouched. Run it
again after adding a skill here.
Codex loads whatever branch is checked out here, so keep the repo on `main`.

## Skills

| Skill | For |
|---|---|
| `code-review` | read-only review of a branch or PR, verified findings |
| `adr-critique` | critique of ADR drafts before the owner approves them |
| `root-cause` | independent root-cause analysis of a bug before Claude fixes it |
| `security-audit` | second auditor next to Claude, read-only, secrets redacted |
| `docs-writer` | docs in its own worktree; Claude commits them |
| `git-flow` | branch/commit/PR flow, only in this repo |
| `claude-bridge` | ask Claude for a read-only second opinion |

## Use the bridge

From a project directory, ask Codex to use the `claude-bridge` skill. It invokes Claude
through `claude -p` with read-only tools and returns the reply for Codex to assess. Verify
Claude's findings against the project before acting.

## Context file in every repo

The `codex-context` skill makes Codex keep an `AGENTS.md` in each repo it works in: its
role there, the repo's state, and a "Pending for Codex" list to resume from. Linked into
`~/.agents/skills` like the other skills (loop above), so it applies everywhere; the global
`~/.codex/AGENTS.md` tells Codex to always follow it. When Codex is unavailable, Claude writes the pending items for it.

## MCP and plugins

No custom MCP server is required for the bridge. The Claude CLI is the communication
endpoint, and invoking it through the shell keeps the exchange visible. Exposing Claude's
general tools to Codex through MCP would grant broader capabilities than needed.

The skill works directly from `.agents/skills`. `plugins/claude-bridge` records the
packaging location for a future Codex plugin; no plugin is installed by this repo yet.
