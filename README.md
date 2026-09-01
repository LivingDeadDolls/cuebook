# Cuebook

[English](README.md) | [日本語](README.ja.md)

Cuebook separates software work into two acts: a researched specification and
its minimal, verified implementation. It ships equivalent skills for Claude
Code and Codex while routing each role to an intentional model and effort.

## Quick install

Install Cuebook for each runtime you use. These commands run in your terminal
and install the plugin for your user account.

### Claude Code

Requires the `claude` CLI to be installed and signed in.

```sh
claude plugin marketplace add LivingDeadDolls/cuebook && \
  claude plugin install cuebook@cuebook --scope user
```

Verify the installation:

```sh
claude plugin list
```

### Codex

Requires the `codex` CLI to be installed and signed in.

```sh
codex plugin marketplace add LivingDeadDolls/cuebook --ref main && \
  codex plugin add cuebook@cuebook
```

Verify the installation:

```sh
codex plugin list
```

## Use

Open Claude Code or Codex in the project you want to work on, then enter one of
these commands in its chat:

| Act | Claude Code | Codex | Result |
| --- | --- | --- | --- |
| Script | `/cuebook:script #123` | `$cuebook:script #123` | Saves `plans/issue-123.md` |
| Perform | `/cuebook:perform plans/issue-123.md` | `$cuebook:perform plans/issue-123.md` | Implements and verifies the plan |

You can pass a written request instead of an issue number. `script` researches
facts before asking questions and never edits product code. `perform` owns the
goal through completion and pauses only for decisions requiring user authority.

## Update

### Claude Code

```sh
claude plugin marketplace update cuebook && \
  claude plugin update cuebook@cuebook --scope user
```

### Codex

```sh
codex plugin marketplace upgrade cuebook && \
  codex plugin remove cuebook@cuebook && \
  codex plugin add cuebook@cuebook
```

## Uninstall

### Claude Code

```sh
claude plugin uninstall cuebook@cuebook --scope user
claude plugin marketplace remove cuebook
```

### Codex

```sh
codex plugin remove cuebook@cuebook
codex plugin marketplace remove cuebook
```

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

## Contract

Plans live in `plans/` inside the project being worked on. A plan fixes the
goal, non-goals, acceptance criteria, and authority boundaries. During
implementation, factual unknowns are researched and implementation details may
be replanned without rewriting that contract. Run `script` again only when the
desired outcome changes.

## License

[MIT](LICENSE)
