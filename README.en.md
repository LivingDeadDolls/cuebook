# Cuebook

[日本語](README.md) | English

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
| Perform | `/cuebook:perform` | `$cuebook:perform` | Implements the newest ready plan |

You can pass a written request instead of an issue number. To perform a
specific plan, pass only its issue number or slug; the `plans/` prefix is
optional. `script` researches facts before asking questions and never edits
product code. `perform` creates a dedicated branch and worktree by default,
then owns the goal through verified completion.

## Customize model routing

Cuebook keeps routing in plain Markdown so you can edit it without another
configuration layer. Use model names and effort levels supported by your host.

### Claude Code

| Role | File | Edit |
| --- | --- | --- |
| Script Director | `claude/skills/script/SKILL.md` | Frontmatter `model` and `effort` |
| Dramaturg | `agents/dramaturg.md` | Frontmatter `model` and `effort` |
| Perform Director | `claude/skills/perform/SKILL.md` | Frontmatter `model` and `effort` |
| Performer | `agents/performer.md` | Frontmatter `model` and `effort` |

### Codex

| Role | File | Edit |
| --- | --- | --- |
| Script Director and Dramaturg | `skills/script/SKILL.md` | Model and reasoning effort in the spawn instructions |
| Perform Director, Dramaturg, and Performer | `skills/perform/SKILL.md` | Model and reasoning effort in the spawn instructions |

For persistent customization, edit a clone or fork rather than the installed
plugin cache, which updates may replace. From a fresh clone:

```sh
git clone https://github.com/LivingDeadDolls/cuebook.git
cd cuebook
# Edit the files listed above, then install either or both runtimes.
claude plugin marketplace add "$PWD"
claude plugin install cuebook@cuebook --scope user
codex plugin marketplace add "$PWD"
codex plugin add cuebook@cuebook
```

If Cuebook is already installed from GitHub, uninstall it first before adding
the customized local marketplace.

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

## Default model routing

| Runtime | Role | Model | Effort |
| --- | --- | --- | --- |
| Claude Code | Script Director | Fable 5.1 | medium |
| Claude Code | Dramaturg (research) | Opus 5 | medium |
| Claude Code | Perform Director | Opus 5 | medium |
| Claude Code | Performer | Sonnet 5 | high |
| Codex | Script Director | GPT-5.6 Sol | high |
| Codex | Dramaturg (research) | GPT-5.6 Terra | medium |
| Codex | Perform Director | GPT-5.6 Terra | high |
| Codex | Performer | GPT-5.6 Luna | xhigh |

## Contract

Plans live in `plans/` inside the project being worked on. A plan fixes the
goal, non-goals, acceptance criteria, and authority boundaries. During
implementation, factual unknowns are researched and implementation details may
be replanned without rewriting that contract. `perform` uses a dedicated branch
and worktree unless the current checkout is already dedicated to the plan. Run
`script` again only when the desired outcome changes.

## License

[MIT](LICENSE)
