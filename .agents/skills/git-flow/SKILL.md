---
name: git-flow
description: "The owner's git flow for your own repo (~/projects/codex): branch, commit, changelog, push, PR, cleanup. Use whenever you change files in ~/projects/codex, or are asked to commit, push or open a PR there. In any other repo, git is read-only for you."
---

# Git flow (only in `~/projects/codex`)

Every other repo: read-only git (`log`, `diff`, `show`, `status`, `blame`). No branch,
commit, checkout, stash, reset, rebase, tag or push there, even though your sandbox would
allow it.

## Never
- Commit, push or merge to `main`. Only the owner merges PRs: no `gh pr merge`.
- `--no-verify` (gitleaks runs on every commit: if it fires, stop and show the owner),
  `git push --force` (only `--force-with-lease`, on your own branch), git config changes,
  `--author`, unsigned commits, `git add -A` / `git add .`.

## Steps: new task
1. `git status --porcelain` must print nothing: no staged, unstaged or untracked work.
   Otherwise stop and tell the owner; never mix their work into your commits.
2. `git switch main && git pull --ff-only`, then `git switch -c <type>/<slug>`:
   `feature/`, `bug/`, `hotfix/`, `minor/`, `refactor/`, `docs/`, `chore/`; 2-5 lowercase
   words with hyphens. One topic per branch.
3. Add a line to `CHANGELOG.md` under `## Unreleased` (`### Added`, `### Fixed`,
   `### Changed`, `### Removed`).
4. Changed a command, the layout or a convention: update `AGENTS.md` / `CLAUDE.md` in the
   same branch.
5. `git add <files>` by name; `git diff --cached --stat` lists only them; then commit with
   a heredoc: `git commit -F - <<'EOF'`.
6. Fixup or WIP commits: squash before the PR. Tree clean again (step 1), then
   `git reset --soft $(git merge-base main HEAD)` and commit again.
7. Before the first push, ask Claude for a review (`claude-bridge`). Claude has no shell,
   so pipe the diff in: `git diff main...HEAD | claude -p ... "Review this diff ..."`
   (no secrets in it). Commit each verified fix (step 5) and ask again until a review needs
   no more changes. Then
   `git diff main...HEAD` must show exactly what you mean to publish.
8. `git push -u origin <branch>` as a command of its own, never chained with `&&`.
9. Open the PR without prompts:
   `gh pr create --base main --title "<commit subject>" --body-file - <<'EOF'` with the
   body below, then `gh pr checks <n>`. Give the owner the link and stop.

## Steps: asked only to push or open a PR
Stay on the current branch: `git branch --show-current` isn't `main`, `git log main..HEAD`
holds the work, and the tree is clean (step 1). Then steps 7-8; step 9 only if a PR was
asked for and none exists yet (`gh pr view` fails). Unsure which branch: ask the owner.

## After the owner merges
`gh pr view <n> --json state,headRefOid` shows `MERGED`, and `headRefOid` equals
`git rev-parse <branch>` (else there's unpushed work: stop and tell the owner), and the
tree is clean (step 1). Then
`git switch main && git pull --ff-only && git branch -D <branch>`.

## Commit message
- `[ADR-0000.N] Area: what changed 🤖`: ≤ ~72 chars, no trailing period, no `feat:`.
  `N` is one more than the last: `git log --all --format=%s | grep -o '\[ADR-0000\.[0-9]*\]'
  | sort -t. -k2 -n | tail -1`. A major change needs an ADR the owner approves first.
- No `Co-Authored-By` lines.
- Body (skip if trivial): why, then a `- ` bullet per change, how you tested; wrap at ~72.

## PR body
```
## Why
## What changed
## Testing
## Claude review
## Changelog
```

## Network
No network in your session: don't retry or work around a failed `git push`/`gh`; give the
owner the exact command.
