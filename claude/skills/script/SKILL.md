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
5. Ask the user only for a decision that research cannot answer: desired
   behavior, scope, acceptance, authority, destructive action, or a material
   security/privacy tradeoff. Use the question format below.
6. Choose the smallest approach that meets the accepted requirement: reuse
   existing code, then standard library/native features, then installed
   dependencies. Add no speculative abstraction, compatibility layer,
   framework, configuration, dependency, or test suite.
7. Create `plans/` if needed and save the contract. Use
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
