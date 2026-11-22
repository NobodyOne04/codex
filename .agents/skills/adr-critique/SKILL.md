---
name: adr-critique
description: 'Critique an ADR draft before the owner sees it: weak trade-offs, missing alternatives, gaps, conflicts with existing ADRs. Use whenever Claude sends an ADR draft (`*.adr`) or asks "critique/check this ADR".'
---

# ADR critique

Claude drafts ADRs, the owner approves them. You critique the draft so the owner gets a
better one. You never approve, reject, edit or write an ADR.

## Read first
- The draft, and every ADR in the repo's `docs/adr/` (index: `docs/adr/README.md`).
- The code or config the ADR is about, enough to check its claims.

## Check
1. **One decision**: does it decide exactly one thing? Several → say how to split.
2. **Problem**: is the problem real and stated with facts, not assumed?
3. **Claims**: are the commands, paths, versions, config keys and behaviours it names
   true? Check them in the repo or with the tool's `--help`.
4. **Alternatives**: is a serious option missing, or dismissed with a weak reason?
5. **Consequences**: costs, risks, failure modes and what gets harder later: are they
   named honestly?
6. **Conflicts**: does it contradict or silently override an accepted ADR? Then it must
   supersede it explicitly.
7. **Rebuildable**: could a developer rebuild the decision from "Technical details"
   without reading the code?
8. **Status**: a draft says `Proposed`; never call a proposal decided.

## Output
One line per point, most important first:
```
[P1] 0008-foo.adr "Technical details": <problem> (<why it matters>) suggestion: <fix>
```
- `P1` wrong fact or conflict with an accepted ADR; `P2` missing alternative, risk or
  detail; `P3` clarity.
- Nothing to add: `nothing found`, plus what you checked.
