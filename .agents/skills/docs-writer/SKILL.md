---
name: docs-writer
description: Write or update documentation (README, guides, docs/ pages, CLAUDE.md/AGENTS.md text) in a repo, in your own worktree. Use whenever Claude or the owner asks you to write, update or fix docs.
---

# Docs writer

You write docs; Claude checks your diff, commits it and opens the PR. In repos other than
`~/projects/codex` you never commit or push.

Docs in `~/projects/codex` itself: use `git-flow` instead (your branch, changelog line,
commit, PR). Everything below is for other repos.

## Where
- Work only in the worktree you were started in (`codex exec --worktree`).
- Only `*.md` files and files under `docs/`. Never code, config, `*.adr` files, or
  `CHANGELOG.md` (Claude adds its line with the PR). A change that needs code: say so and
  stop.

## How to write
- Read the code and the existing docs first; document what the code really does, with the
  real commands, paths and flags. Run `--help` or read the source to check them.
- Plain, short sentences; the reader is busy. Lead with what they need to do.
- Concrete over general: exact commands in code blocks, real file paths in backticks.
- Match the repo's existing tone and layout; keep `CLAUDE.md`/`AGENTS.md` under ~100 lines.
- Don't invent features, numbers or plans. Unknown: leave a `TODO(owner): <question>`.
- No marketing words, no emoji unless the repo uses them.

## Output
- List the files you changed, one line each with what changed.
- List any `TODO(owner)` you left and any claim you couldn't verify.
- Leave the changes uncommitted in the worktree.
