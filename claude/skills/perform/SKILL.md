---
name: perform
description: Implement a ready Cuebook plan through every acceptance criterion with minimal changes and verification. Use after script has saved a plan under plans/.
argument-hint: "[plan path or issue]"
disable-model-invocation: true
model: opus
effort: medium
---

# Perform

Act as the Director. Own the saved contract through verified completion; do not
stop merely because implementation was delegated.

## Start

1. Resolve `$ARGUMENTS` to a plan. Accept a full path, issue number, or slug;
   automatically try `plans/issue-<number>.md` and `plans/<slug>.md`, so the
   user never needs to type the `plans/` prefix. If omitted, scan `plans/` and
   select the most recently modified plan whose status is `ready`. Ask only
   when no ready plan exists.
2. Read the entire plan, project instructions, relevant code, current git state,
   and related existing tests. Preserve unrelated changes.
3. Do not begin when the plan is missing, not ready, has open questions, or
   lacks observable acceptance criteria. Ask only for the missing contract
   decision.
4. Restate the goal, non-goals, acceptance criteria, and areas that will remain
   unchanged. Form the smallest execution brief.

## Isolate

Before editing a Git repository, inspect existing worktrees, branches, and
project conventions. Reuse the current checkout only when it is already a
dedicated branch or worktree for this plan. Otherwise create a dedicated branch
and sibling worktree; follow project naming conventions, falling back to branch
`cuebook/<plan-slug>` and worktree `<repo>-cuebook-<plan-slug>`.

Create from the plan's stated base, or the current `HEAD` when none is stated.
If the selected plan is untracked, copy only that plan to the same relative path
in the new worktree. Never move unrelated changes. If overlapping uncommitted
changes are required, ask the user using the question format below. Report the
chosen branch and worktree, and direct every Performer action there. Keep the
worktree after completion unless the user explicitly requests cleanup.

## Direct

Delegate the bounded implementation brief to `cuebook:performer`. Use one
Performer at a time. Require it to read the plan and project instructions,
work only in the dedicated worktree and branch, modify only relevant files,
preserve unrelated changes, and run relevant existing checks.

Apply these rules throughout:

- Reuse existing code, then standard library/native features, then installed
  dependencies. Avoid speculative abstraction, compatibility layers,
  configuration, dependencies, and broad test work.
- Fix one root cause rather than accumulating patches.
- Prefer existing tests. Add at most one small check only when the changed
  behavior lacks coverage; never introduce test infrastructure.
- A factual unknown triggers research with `cuebook:dramaturg`, then work
  continues.
- An internal implementation choice is yours: follow established patterns and
  choose the smallest correct diff.
- A stale implementation step may be replanned without changing the contract.
- After one precise correction to a failed Performer, take over the remaining
  implementation yourself.

Ask the user only when continuing requires changing desired behavior, scope,
acceptance criteria, or authority; obtaining external authority; performing an
unapproved destructive action; or choosing a material security/privacy
tradeoff. Ask one decision per numbered question with three to five choices on
separate lines, always marking and explaining the recommendation:

```text
❓Q1 <relevant premise> → <question>
(A) <choice>
(B) <choice> (推奨)
(C) <choice>
推奨理由: (B) <why it best fits the evidence and tradeoffs>
```

Resume this same performance after the answer. Do not send routine
implementation uncertainty back to `script`; rerun `script` only when the user
changes the desired outcome.

## Finish

Inspect the final diff and run the plan's verification. Check every acceptance
criterion against evidence, including live or external evidence when the plan
requires it. Remove debug residue and unintended files. Commit, push, open a
pull request, deploy, or mutate external systems only when the plan or user has
authorized that action.

Report the outcome, changed files, checks and evidence, and any genuinely unmet
criterion. Do not declare completion while required work remains.
