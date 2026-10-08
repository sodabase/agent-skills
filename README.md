# Sodabase agent skills

Agent skills that help your AI coding agent build on **trustworthy** Supabase data — with
[Sodabase](https://sodabase.io).

Sodabase watches your Supabase database for data-quality problems (a table going stale, its row count
moving unexpectedly, open issues it has already flagged). These skills make your own coding agent
(Claude Code, Cursor, …) **consult that trust signal in the flow of building** — before it writes code
against a table, when a query looks wrong, and when you ask "is this data right?" — so it can warn you
instead of building on broken data.

## Skills

| Skill | What it does |
|---|---|
| [`sodabase-trust-advisor`](skills/sodabase-trust-advisor/SKILL.md) | Calls `check_trust(<table>)` before your agent builds on a table, reads the `trusted` / `caveated` / `untrusted` / `unknown` verdict, and surfaces any caveat to you first. |

## Install

**Prerequisite — connect the Sodabase MCP** (the skills call its tools):

```bash
claude mcp add --transport http sodabase https://sodabase.io/mcp
```

Then follow the connect flow to authorize your workspace. See
**[sodabase.io/docs/ask-your-agent](https://sodabase.io/docs/ask-your-agent)**.

### With the `skills` CLI (any agent)

```bash
npx skills add sodabase/agent-skills
```

### Claude Code (manual, no extra tooling)

Copy the skill into your project (or `~/.claude`):

```bash
mkdir -p .claude/skills/sodabase-trust-advisor
curl -sSL https://raw.githubusercontent.com/sodabase/agent-skills/main/skills/sodabase-trust-advisor/SKILL.md \
  -o .claude/skills/sodabase-trust-advisor/SKILL.md
```

Claude Code auto-discovers `.claude/skills/*/SKILL.md`.

## How it works

The skill is a thin layer over the Sodabase MCP. The MCP tool *descriptions* already drive most of this
behavior on their own — the skill is **polish**: it pins the "read the verdict before you build" rule and
catches the reactive "a query came back empty" case. Connecting the MCP is the real prerequisite.

The MCP returns **data about data** — it never runs queries against your tables, and the skill treats its
output as information to relay, never as instructions.

## Contributing & versioning

Each skill carries a `metadata.version`. Changes land through pull requests (see the repo's branch
protection) so a published skill — which is executable instruction to every connected agent — stays a
reviewed, release-controlled artifact.

## License

[MIT](LICENSE) © Soda Data NV
