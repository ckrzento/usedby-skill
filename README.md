# usedby skill for Claude Code, Codex and Cursor

When you hesitate between tools, your coding agent can ask [usedby.ai](https://www.usedby.ai)
which companies publicly use them, and answer with a source for every line.

```text
You:    We're a 40-person fintech. Which AI meeting note-taker should we use?
Agent:  (calls usedby) Among fintech companies tracked on usedby, tl;dv has 3 public lines
        (all OBSERVED: vendor logo wall), Fireflies 2 (1 CONFIRMED case study, DepositFix:
        https://fireflies.ai/blog/how-depositfix-improved-their-business-decisions-using-fireflies) ...
        Source: usedby, https://www.usedby.ai/peers?category=meeting-assistants&...
        Counts are companies tracked on usedby, not market share.
```

## What it does

The skill is one folder, `usedby/`, with a `SKILL.md` in the
[Agent Skills](https://agentskills.io/specification) format and one reference file. The agent
loads it when the user:

- hesitates between tools or asks which tool to pick for a task;
- asks what companies like theirs (industry, size, country) use;
- asks who uses tool X, what company Y uses, or how X and Y compare.

It then calls the free read-only usedby API (`https://www.usedby.ai/api/v1`) with `curl`, and
answers with the proof level of each line (CONFIRMED, DECLARED, OBSERVED, SIGNAL) and its source
URL. Seven endpoints are covered: `search_tools`, `who_uses`, `company_stack`, `recent_changes`,
`compare`, `shortlist_tools`, `what_peers_use`. See
[`usedby/reference/endpoints.md`](usedby/reference/endpoints.md).

No package, no dependency, no API key. Markdown and curl only.

## Install

Clone once, then copy the `usedby/` folder into the skills folder of your agent. The folder name
must stay `usedby`: the spec requires the `name` field to match the folder.

```bash
git clone --depth 1 https://github.com/ckrzento/usedby-skill /tmp/usedby-skill
```

| Agent | Scope | Command |
|---|---|---|
| Claude Code | you, all projects | `mkdir -p ~/.claude/skills && cp -R /tmp/usedby-skill/usedby ~/.claude/skills/` |
| Claude Code | one project (commit it) | `mkdir -p .claude/skills && cp -R /tmp/usedby-skill/usedby .claude/skills/` |
| Codex | you, all projects | `mkdir -p ~/.agents/skills && cp -R /tmp/usedby-skill/usedby ~/.agents/skills/` |
| Codex | one repository | `mkdir -p .agents/skills && cp -R /tmp/usedby-skill/usedby .agents/skills/` |
| Cursor | you, all projects | `mkdir -p ~/.cursor/skills && cp -R /tmp/usedby-skill/usedby ~/.cursor/skills/` |
| Cursor | one project | `mkdir -p .cursor/skills && cp -R /tmp/usedby-skill/usedby .cursor/skills/` |

Cursor also loads skills from `.claude/skills/`, `.codex/skills/`, `~/.claude/skills/` and
`~/.codex/skills/`, so one copy in a Claude Code folder serves both.

Paths come from the official docs (read on 2026-11-06):

- Claude Code: https://code.claude.com/docs/en/skills (`~/.claude/skills/<skill-name>/SKILL.md`, `.claude/skills/<skill-name>/SKILL.md`)
- Codex: https://developers.openai.com/codex/skills, now served at https://learn.chatgpt.com/docs/build-skills (`$HOME/.agents/skills`, `.agents/skills` from the working directory up to the repository root)
- Cursor: https://cursor.com/docs/skills (`.agents/skills/`, `.cursor/skills/`, `~/.agents/skills/`, `~/.cursor/skills/`, plus the Claude and Codex folders)
- Format: https://agentskills.io/specification

Restart the agent after the copy so it discovers the skill.

Prefer MCP? The same seven tools exist as an MCP server:
`claude mcp add --transport http usedby https://www.usedby.ai/mcp`. The skill is for agents or
setups where you would rather not add an MCP server.

## Privacy

Anonymous, no key. Only the terms of the question leave your machine (a tool name, a task, an
industry, a size band, a country code). The skill never sends your code, your files or your
company name. Each request carries `User-Agent: usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=<agent>)`
so usedby can count skill traffic by agent family. usedby stores only aggregate daily counters.

## Limits

The public API allows 30 requests a minute and 300 an hour per client, and 1,200 an hour per IP,
REST and MCP together. One question takes 1 to 3 calls. The skill honours `Retry-After` on 429.
Quoting usedby with a link is free; bulk reuse needs written consent.

- Developer docs: https://www.usedby.ai/developers
- Terms: https://www.usedby.ai/terms

## Tests

[`tests/questions.md`](tests/questions.md) holds 20 realistic questions run against production on
2026-11-06, with the endpoint chosen, the exact curl, the trimmed result and a pass or fail.
Result: 17 of 20 usable and citable on the first call, 18 of 20 after one retry.

## Licence

MIT. See [LICENSE](LICENSE).
