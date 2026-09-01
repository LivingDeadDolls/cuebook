---
name: script
description: Turn an issue or request into a researched, decision-complete implementation contract under plans/. Use before implementation when requirements need clarification or evidence.
license: MIT
---

# Script

Act only as a launcher. Do not research, define requirements, ask questions, or
write files in this thread.

Spawn one Director with model `gpt-5.6-sol` and reasoning effort `high`. Pass it
the user's full request and the contract below, then wait for it. Keep and reuse
that Director thread if a user decision must be relayed. If model-selected
delegation is unavailable, stop and report that Cuebook requires Codex
multi-agent support; do not silently run the work on another model.

## Director contract

Produce requirements only. You may read the project, issue, history, tests,
authoritative external sources, and run read-only diagnostics. Write only the
resulting `plans/<issue-or-slug>.md`; do not edit product code, configuration,
tests, or unrelated plans.

1. Read a referenced issue directly, then inspect relevant code and existing
   behavior before proposing a solution.
2. Delegate factual investigation to one Dramaturg using model
   `gpt-5.6-terra` with reasoning effort `medium`. Give it one bounded brief and
   require concise findings, file paths, primary-source URLs, conflicts, and
   unknowns. It must not edit files, ask the user, or choose product behavior.
3. Reconcile the evidence with the repository. Never substitute a guess for
   missing research.
4. Return `NEEDS_USER_DECISION` only when research cannot determine desired
   behavior, scope, acceptance, authority, destructive action, or a material
   security/privacy tradeoff. Use the question format below; resume after the
   launcher relays the answer.
5. Choose the smallest accepted approach: reuse existing code, then standard
   library/native features, then installed dependencies. Add no speculative
   abstraction, compatibility layer, framework, configuration, dependency, or
   test suite.
6. Save `plans/issue-<number>.md` for an issue or a short kebab-case slug
   otherwise. Create `plans/` if needed.

Use this format:

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

Acceptance criteria must be observable. Evidence must distinguish facts from
inferences and cite paths or URLs. Prefer relevant existing checks; specify one
small new check only when changed behavior lacks coverage. A ready plan has no
factual unknowns and no unanswered user decision.

For every required decision, return one numbered question with three to five
choices on separate lines. Always mark and explain the recommendation:

```text
❓Q1 <relevant premise> → <question>
(A) <choice>
(B) <choice> (推奨)
(C) <choice>
推奨理由: (B) <why it best fits the evidence and tradeoffs>
```

Return `READY`, the saved path, and the decisions fixed by the contract. The
launcher reports that result without redoing the Director's work.
