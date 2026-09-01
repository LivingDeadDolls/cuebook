# Cuebook

Cuebook separates software work into two acts: a researched specification and
its minimal, verified implementation. It ships equivalent skills for Claude
Code and Codex while routing each role to an intentional model and effort.

## Commands

| Act | Claude Code | Codex | Result |
| --- | --- | --- | --- |
| Script | `/cuebook:script` | `$cuebook:script` | Saves a contract to `plans/<issue-or-slug>.md` |
| Perform | `/cuebook:perform` | `$cuebook:perform` | Implements a saved contract through acceptance |

Pass an issue, request, or plan path with the command. `script` researches
facts before asking questions and never edits product code. `perform` owns the
goal through completion; it pauses only for decisions that require user
authority.

## Model routing

| Runtime | Role | Model | Effort |
| --- | --- | --- | --- |
| Claude Code | Script director | Fable 5.1 | medium |
| Claude Code | Dramaturg (research) | Opus 5 | medium |
| Claude Code | Perform director | Opus 5 | medium |
| Claude Code | Performer | Sonnet 5 | high |
| Codex | Script director | GPT-5.6 Sol | high |
| Codex | Dramaturg (research) | GPT-5.6 Terra | medium |
| Codex | Perform director | GPT-5.6 Terra | high |
| Codex | Performer | GPT-5.6 Luna | xhigh |

Model aliases must be available in the host. Claude Code applies its routing
from skill and agent frontmatter. Codex applies it when each skill delegates.

## Install

### Claude Code

Add this repository as a plugin marketplace, then install `cuebook`:

```text
/plugin marketplace add LivingDeadDolls/cuebook
/plugin install cuebook@cuebook
```

### Codex

```sh
codex plugin marketplace add LivingDeadDolls/cuebook --ref main
codex plugin add cuebook@cuebook
```

## Contract

Plans live in `plans/` inside the project being worked on. A plan fixes the
goal, non-goals, acceptance criteria, and authority boundaries. During
implementation, factual unknowns are researched and implementation details may
be replanned without rewriting that contract. Run `script` again only when the
desired outcome changes.

## License

[MIT](LICENSE)
