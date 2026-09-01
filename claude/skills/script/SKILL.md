---
name: script
description: Turn an issue or request into a researched, decision-complete implementation contract under plans/. Use before implementation when requirements need clarification or evidence.
argument-hint: "[issue, request, or slug]"
disable-model-invocation: true
model: fable
effort: medium
---

# Script

Act as the Director. Produce requirements only; do not implement them.

## Boundaries

- You may read the project, issue, history, tests, and authoritative external
  sources, and may run read-only diagnostics.
- Write only the resulting `plans/<issue-or-slug>.md` file. Do not edit product
  code, configuration, tests, or unrelated plans.
- Treat the goal, non-goals, acceptance criteria, and authority boundaries as
  the contract that `perform` will later execute.

## Process

1. Resolve `$ARGUMENTS` as an issue, request, or useful slug. Read an issue
   directly when one is referenced.
2. Inspect the relevant repository paths and existing behavior before proposing
   a solution.
3. Send factual investigation to the `cuebook:dramaturg` agent. Combine related
   questions into one bounded brief; do not start agents by default in parallel.
4. Reconcile its evidence with the repository. Never replace missing research
   with a guess.
5. Map every unresolved user decision as a design tree whose branches record
   prerequisite decisions. Facts are research tasks, not user questions. Only
   include desired behavior, scope, acceptance, authority, destructive action,
   or a material security/privacy tradeoff that research cannot determine.
6. Work the tree in rounds. Ask the entire current frontier: every decision
   whose factual and decision prerequisites are settled. Defer decisions that
   depend on an answer still open in that round.
7. After each answer round, recompute the frontier. When it is empty, summarize
   the resulting contract and obtain the user's confirmation that it reflects
   the shared understanding. Do not mark the plan ready before confirmation.
8. Choose the smallest approach that meets the accepted requirement: reuse
   existing code, then standard library/native features, then installed
   dependencies. Add no speculative abstraction, compatibility layer,
   framework, configuration, dependency, or test suite.
9. Create `plans/` if needed and save the contract. Use
   `plans/issue-<number>.md` for an issue and a short kebab-case slug otherwise.

## Plan format

```markdown
# <title>

- Source: <issue, prompt, or link>
- Status: ready

## Goal
## Non-goals
## Acceptance criteria
## Evidence
## Minimal approach
## Verification
## Authority boundaries
## Open questions
```

Acceptance criteria must be observable. Evidence must separate confirmed facts
from inferences and cite paths or URLs. Verification should prefer relevant
existing checks; specify one small new check only when changed behavior has no
coverage. A ready plan has no factual unknowns and no unanswered user decision.

## Question format

Ask one decision per numbered question. Use three to five choices, each on its
own line, and always mark and explain the recommendation:

```text
❓Q1 <relevant premise> → <question>
(A) <choice>
(B) <choice> (推奨)
(C) <choice>
推奨理由: (B) <why it best fits the evidence and tradeoffs>
```

Finish by reporting the saved path and the decisions fixed by the contract.
