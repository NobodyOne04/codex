---
name: codex-context
description: Create or update AGENTS.md, Codex's context file, in every repo you work in. Use at the start of every task inside a git repo (missing AGENTS.md → create it), whenever what you learned makes it wrong, and before you stop with work left unfinished (add it under "Pending for Codex").
---

# Codex context file

Every repo the owner has keeps an `AGENTS.md` at its root: what Codex needs to pick up
work there without rediscovering it. Claude keeps `CLAUDE.md`; this is the Codex side.

## Why it must point to CLAUDE.md
Your config reads `CLAUDE.md` only as a *fallback*, when a repo has no `AGENTS.md`. Once
`AGENTS.md` exists you stop seeing `CLAUDE.md` automatically. So `AGENTS.md` always opens
by telling you to read `CLAUDE.md`, and never copies its commands, layout or conventions.

## When
1. **Start of every task in a repo** (`git rev-parse --show-toplevel`): no `AGENTS.md`?
   Create it from the template below, from what you can read (`CLAUDE.md`, `README.md`,
   `docs/adr/`, `git log`, open PRs via `gh pr list`).
2. **During the task**: a fact in it turned out wrong or out of date → fix it.
3. **Before you stop with something unfinished** (review interrupted, usage limit, a
   question for the owner): add it under `## Pending for Codex` with the exact command to
   resume. Remove items that are done.

## How (your rights still apply)
- In `~/projects/codex`: edit and commit it through your own Git flow (`AGENTS.md` there
  is your role file; keep its structure, add a `## State` / `## Pending for Codex` only if
  useful).
- Any other repo: it's a docs change. Write it only in a `--worktree` session (it's a
  `*.md` file, so allowed). Never commit, branch or push there. Tell Claude (or the owner)
  the file is ready; Claude commits it through `github-flow`.
- Read-only session (e.g. Claude called you with `-s read-only`): don't write. Put the
  full proposed file in your reply under a `## Proposed AGENTS.md` heading so Claude can
  commit it.
- If Claude wrote or changed it while you were unavailable, read it first and follow its
  `Pending for Codex` list before anything else.

## Template
```markdown
# <repo>: Codex context

Read `CLAUDE.md` first: commands, layout and conventions live there. This file replaces
Codex's `CLAUDE.md` fallback, so it only adds what Codex needs. Global rules:
`~/.codex/AGENTS.md`.

## Your role here
<reviewer / auditor / docs, plus anything repo-specific: phase, what not to touch>

## State (as of YYYY-MM-DD)
<repo visibility and protection, open PRs, what's in flight>

## Pending for Codex
- <task>: `<exact command>`        (or: nothing)

## Notes
<review guidance: deliberate decisions not to re-report, known limits, where findings go>
```

## Rules
- Short: under ~60 lines. Facts, not history; dates on anything that goes stale.
- No secrets, tokens or personal data. Name where a secret lives, never its value.
- Never contradict `CLAUDE.md` or the ADRs; if they look wrong, report it as a finding.
- Don't touch `CLAUDE.md` or `*.adr` files.
