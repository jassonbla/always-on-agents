# always-on-agents

A harness for long-running coding agents with a persona, such as a CTO, CFO, or CPO bot.

This repository is the harness. It is Markdown. There are no scripts and no hooks.
Clone it and start your agent in the root. The agent reads `AGENTS.md`, and `AGENTS.md` explains the harness.

The design goal: the files that the agent loads at every session start stay constant for months.
All information that grows goes into directories that the bot searches on demand.

## Scope

This harness does two things:

1. It keeps the memory of the bot correct and searchable for months.
2. It keeps the persona of the bot constant. The bot does not change its own rules or persona. A human approves all changes.

This harness does not control how the bot starts. Schedulers, webhooks, event inboxes, outboxes, and the agent runtime are not part of the harness. Tune them for your deployment.

## The problem

A bot that runs for months with only one instruction file writes everything into that file.
Critical rules, minor rules, and one-time "remember this" notes collect in one file.
The file grows, the rules lose their priority, and the behavior of the bot drifts.

## Principles

1. **Constant root, growing directories.** `AGENTS.md`, `PERSONA.md`, and `MEMORY.md` have line limits. History and references go into `journal/`, `memory/`, and `references/`.
2. **Every kind of information has one destination.** The write routing in `AGENTS.md` tells the bot where each kind goes. A bot with only one writable file puts everything into that file.
3. **The bot does not edit its own rules.** A request to change a rule becomes a file in `proposals/`. A human approves it.
4. **Daily consolidation.** The daily restart is the point where the bot moves durable facts from the journal into memory files.
5. **Controlled language.** All instruction files use ASD-STE100 Simplified Technical English, so the tone and the interpretation of rules do not drift between sessions and between models.
6. **Any agent, any model.** The harness assumes only three abilities: the agent loads one entry file, reads and writes files, and runs shell commands.

## Layout

```
AGENTS.md              Operating rules and write routing. The single source. The same for all bots.
PERSONA.md             Identity, role, response language. The only file that differs for each bot.
MEMORY.md              Index only. One line for each memory file.
memory/<topic>.md      Semantic memory. One topic for each file. Updated in place.
journal/<date>.md      Episodic memory. Append only.
references/            Source documents, with INDEX.md.
proposals/             Bot requests to change a constant file.
procedures/            Procedures that the bot reads before a task, with INDEX.md.

CLAUDE.md              Adapter for Claude Code. It imports AGENTS.md, PERSONA.md, and MEMORY.md.
.claude/settings.json  Adapter for Claude Code. Permissions for the growing directories.
```

| Loaded at every session start | Read on demand |
|---|---|
| `AGENTS.md`, `PERSONA.md`, `MEMORY.md` | `memory/`, `journal/`, `references/`, `procedures/` |

## Create a bot

```bash
git clone https://github.com/jassonbla/always-on-agents.git cto-bot && cd cto-bot
```

Then start your agent in `cto-bot`.

`PERSONA.md` contains placeholders, so the workspace starts in setup mode.
The agent tells you that the persona is not defined and helps you complete `PERSONA.md`.
In setup mode, the agent does not write to the journal or to memory.
When `PERSONA.md` has no placeholder, the workspace is in bot mode and the agent is the persona.

## Agent products

| Product | Entry file | Notes |
|---|---|---|
| Claude Code | `CLAUDE.md` | Tested. The adapter imports the three root files, so they are always in context. |
| Products that read `AGENTS.md` | `AGENTS.md` | Not tested. The agent reads `PERSONA.md` and `MEMORY.md` because `AGENTS.md` tells it to. |
| Other products | Configure the product to load `AGENTS.md` | Not tested. |

Permissions are different for each product. This repository contains only the Claude Code adapter.
For another product, permit writes to `MEMORY.md`, `memory/`, `journal/`, `references/`, `proposals/`, and `procedures/`.

Claude Code notes:

- Start `claude` interactively one time and accept the trust dialog. Claude Code ignores `.claude/settings.json` permissions until the workspace is trusted.
- Always start `claude` in the workspace root, because the permission paths are relative to it.

## Daily cycle

1. Restart the session one time each day in the bot directory.
2. Before its first answer, the bot reads the last 150 lines of the journal of today and of the latest older journal file.
3. The bot does `procedures/consolidate-memory.md` on older journal files that have no `## Consolidated` line.
4. The procedure promotes the facts that pass the test "true and useful one month from now".
5. During the day, the bot appends events to `journal/<date>.md`.

## Maintain a bot

- Keep the bot workspace in git. Commit regularly, so each change to a constant file is visible in the history.
- Read `proposals/` regularly. Apply or delete each proposal.
- To change a constant file, edit it yourself and commit.
- To develop the harness, work in this repository. Its `PERSONA.md` has placeholders, so the agent is in setup mode and can edit each file.

## Limits of this design

The rules are instructions. The harness does not block an edit that breaks a rule.
Models obey instructions to different degrees. A weaker model can break the rules more frequently.
The line limits in `AGENTS.md` and the verification step in `procedures/consolidate-memory.md` are the control.
`git diff` on the root files is the audit.

## License

MIT. See `LICENSE`.
