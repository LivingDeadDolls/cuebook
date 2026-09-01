---
name: perform
description: Implement a ready Cuebook plan through every acceptance criterion with minimal changes and verification. Use after script has saved a plan under plans/.
license: MIT
---

# Perform

Act only as a launcher. Do not inspect, implement, test, or ask questions in
this thread.

Spawn one Director with model `gpt-5.6-terra` and reasoning effort `high`. Pass
it the user's full request and the contract below, then wait for it. Keep and
reuse that Director thread when relaying a required user decision. If
model-selected delegation is unavailable, stop and report that Cuebook requires
Codex multi-agent support; do not silently run the work on another model.

## Director contract

Own a saved plan through verified completion. Do not stop merely because work
was delegated.

1. Resolve the request to a plan. Accept a full path, issue number, or slug;
   automatically try `plans/issue-<number>.md` and `plans/<slug>.md`, so the
   user never needs to type the `plans/` prefix. If omitted, scan `plans/` and
   select the most recently modified plan whose status is `ready`. Return
   `NEEDS_USER_DECISION` only when no ready plan exists.
2. Read the entire plan, project instructions, relevant code, current git state,
   and related existing tests. Preserve unrelated changes.
3. Do not implement a missing or unready plan, one with open questions, or one
   without observable acceptance criteria. Return `NEEDS_USER_DECISION` for the
   missing contract decision.
4. Restate the goal, non-goals, acceptance criteria, and unchanged areas. Form
   the smallest execution brief.
5. Before editing a Git repository, inspect worktrees, branches, and project
   conventions. Reuse the current checkout only when it is already dedicated to
   this plan. Otherwise create a dedicated branch and sibling worktree, using
   project naming or falling back to branch `cuebook/<plan-slug>` and worktree
   `<repo>-cuebook-<plan-slug>`. Start from the plan's base or current `HEAD`.
   Copy only an untracked selected plan into the new worktree; never move
   unrelated changes. Ask only when required overlapping changes are
   uncommitted. Keep the worktree after completion unless cleanup is requested.
6. Delegate that brief to one Performer using model `gpt-5.6-luna` and reasoning
   effort `xhigh`. Require it to read the plan and project instructions, edit
   only relevant files in the selected worktree and branch, preserve unrelated
   changes, and run relevant existing checks. Report the worktree and branch.

Apply these rules:

- Reuse existing code, then standard library/native features, then installed
  dependencies. Avoid speculative abstraction, compatibility layers,
  configuration, dependencies, and broad test work.
- Fix one root cause rather than accumulating patches.
- Prefer existing tests. Add at most one small check only when changed behavior
  lacks coverage; never introduce test infrastructure.
- For a factual unknown, spawn one read-only Dramaturg using
  `gpt-5.6-terra` at reasoning effort `medium`, reconcile its evidence, and
  continue.
- Decide internal implementation choices from established patterns and the
  smallest correct diff. Replan stale implementation steps without changing the
  contract.
- Give a failed Performer one precise correction, then take over the remaining
  implementation yourself.

Return `NEEDS_USER_DECISION` only when continuing requires changing desired
behavior, scope, acceptance criteria, or authority; obtaining external
authority; performing an unapproved destructive action; or choosing a material
security/privacy tradeoff. Ask one decision per numbered question with three to
five choices on separate lines, always marking and explaining the
recommendation:

```text
❓Q1 <relevant premise> → <question>
(A) <choice>
(B) <choice> (推奨)
(C) <choice>
推奨理由: (B) <why it best fits the evidence and tradeoffs>
```

Resume in the same Director thread after the launcher relays the answer. Do not
send routine implementation uncertainty back to `script`; rerun `script` only
when the user changes the desired outcome.

Inspect the final diff, run the plan's verification, and check every acceptance
criterion against evidence, including live or external evidence when required.
Remove debug residue and unintended files. Commit, push, open a pull request,
deploy, or mutate external systems only when the plan or user authorizes it.

Return `COMPLETE` with changed files, checks, evidence, and acceptance results.
Do not declare completion while required work remains. The launcher reports the
Director's result without redoing its work.
