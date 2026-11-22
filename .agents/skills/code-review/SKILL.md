---
name: code-review
description: Review a branch, diff or PR read-only and report verified findings. Use whenever Claude or the owner asks for a review, runs `codex review`, or says "check this change/branch/PR".
---

# Code review

You review; Claude fixes. Never edit, stage or commit anything in the reviewed repo.

## Scope
- Review exactly the change you were asked about. A branch or PR: its commits,
  `git diff <base>...HEAD` and `git log <base>..HEAD` (`origin/main` unless told otherwise).
  Uncommitted work included, or the caller names a diff: use that (`git diff <base>` covers
  the working tree too).
- Read the code the change touches, not only the diff: callers, tests, config, docs it
  makes wrong. A bug the change exposes counts; old bugs it doesn't touch are out of scope
  (mention one only if it's serious, marked `pre-existing`).
- Read the repo's `CLAUDE.md`, `AGENTS.md` and accepted ADRs in `docs/adr/`: a change that
  breaks one of their rules is a finding.

## What to look for, in this order
1. Correctness: wrong logic, edge cases (empty, nil, zero, unicode, large input), error
   paths, races, off-by-one, wrong defaults.
2. Data loss and security: secrets, injection, auth checks, unsafe input at trust
   boundaries, destructive commands without a guard.
3. Contracts: callers, tests, fixtures, docs and config the change broke or forgot.
4. Missing tests for new risky logic.
5. Needless code: duplication, dead code, abstractions nobody uses.
Skip style nits and personal taste.

## Verify before reporting
- Each finding needs a concrete failure: the input or state, and the wrong result.
- Check it in the code. If you can't confirm it, say "unverified" or drop it.
- Never print secret values: name the file and line, redact the value.

## Output
One line per finding, most severe first:
```
[P1] path/to/file.go:42: <problem> (<what breaks, for which input>) confidence: high
```
- `P1` breaks behaviour or loses/exposes data; `P2` likely bug or broken contract;
  `P3` minor but real.
- Then one line of overall verdict.
- Nothing found: `nothing found`, plus what you checked.
