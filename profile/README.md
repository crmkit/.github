<p align="center">
  <img src="https://crmkit.ai/icon-dark.svg" alt="crmkit" width="100" height="100" />
</p>

<h1 align="center">crmkit</h1>

<p align="center">
  <strong>Your CRM is a file your agent reads.</strong>
</p>

<p align="center">
  <a href="https://crmkit.ai">Website</a> · <a href="https://github.com/crmkit/crmkit">Documentation</a> · <a href="https://github.com/crmkit/skills">Skills</a>
</p>

---

crmkit is an **agent-first CRM**, built for AI agents - ChatGPT, Claude, Cursor -
to drive directly. The agent _is_ the interface: it loads a one-page manual and
operates the CRM over plain HTTP. Headless by design (no UI). Responses are
grepable plain text by default; JSON on request.

```bash
# 1. begin login - a 6-digit code is emailed
curl -s -X POST https://api.crmkit.ai/auth/request -d '{"email":"you@example.com"}'

# 2. verify the code to get a token
curl -s -X POST https://api.crmkit.ai/auth/verify -d '{"email":"you@example.com","code":"123456"}'

# 3. operate the CRM
curl -s -X POST https://api.crmkit.ai/contacts \
  -H 'Authorization: Bearer ck_…' \
  -d '{"name":"Jane Doe","email":"jane@acme.com","stage":"lead"}'
```

Plain-text, grepable output - one labeled line per record:

```
contact/c_4u8a…  name="Jane Doe"  email=jane@acme.com  stage=lead  updated=2026-06-04
# 1 contact(s)
```

### Why crmkit exists

A CRM looks like something an agent could just keep in a few markdown files or a
scratch database table. That holds right up until it doesn't. Underneath, a CRM
is a pile of small problems that are each easy to get subtly wrong and painful to
fix once real data has piled up:

- stable IDs that survive renames and re-imports,
- relations between contacts, companies, and deals that stay consistent,
- search, filtering, and pagination that hold up as records grow,
- dedupe, validation, and safe concurrent writes,
- history, audit, and per-tenant limits,
- schema changes you can apply to a live database without losing data.

And a CRM is rarely driven by one agent. Several agents - and the teammates
alongside them - work the same records at the same time, so you need one shared
system of record: consistent reads, safe concurrent writes, and a clear trail of
which agent or member did what. Files and ad-hoc tables fall apart exactly here.

crmkit is that core - one well-maintained, battle-tested foundation that many
agents and deployments share, so you build on solved problems instead of
rediscovering them ad hoc in every project.

### Use cases

crmkit is useful anywhere an agent needs to remember people, organizations,
opportunities, and follow-up work in one shared system:

- **Sales CRM** - track leads, accounts, deals, activities, next steps, and
  pipeline movement without building a dashboard first.
- **Personal contact management** - keep notes, reminders, relationship context,
  and follow-ups for your own network.
- **Customer support** - record customers, companies, conversations, issues,
  escalations, and renewal or upsell opportunities.
- **Market monitoring** - track funded companies, competitors, target accounts,
  hiring signals, product launches, and other entities an agent watches over
  time.
- **Fundraising** - manage investors, intros, conversations, diligence status,
  commitments, and follow-up tasks across a raise.

**Example:** ChatBotKit's [AI Market Bot](https://chatbotkit.com/examples/ai-market-bot)
- a market-research and competitive-intelligence agent - is the market-monitoring
use case above, made real.

### What you get

- **No UI to build or maintain** - the agent is the front end.
- **Plain text first** - token-cheap, grepable, JSON on `Accept`.
- **Instructive errors** - every failure tells the agent what to do next.
- **OTP auth, bearer tokens** - email a code, paste it, get a token.
- **Single static Go binary** - `CGO_ENABLED=0`, SQLite embedded, deploy as one file.
- **Extensible** - every record carries a free-form `custom` object.

### Skills

Drop-in [agent skills](https://github.com/crmkit/skills) - digest, import,
backup, inbox-sync - for coding agents (on ChatGPT/Claude.ai, add the MCP
connector at `https://api.crmkit.ai/mcp` instead). Install the set with one
command (Claude Code, Cursor,
and 40+ agents via [`npx skills`](https://github.com/vercel-labs/skills)):

```bash
npx skills add crmkit/skills
```

Or download the release zip and unzip into your skills directory:

```bash
curl -fsSL https://github.com/crmkit/skills/releases/latest/download/crmkit-skills.zip -o /tmp/crmkit-skills.zip \
  && unzip -o /tmp/crmkit-skills.zip -d ~/.claude/skills/
```

Each recipe needs `CRMKIT_BASE_URL` and a crmkit token (`POST /auth/request` → `/auth/verify`).

### Repositories

| Repo                                       | Description                              |
| ------------------------------------------ | ---------------------------------------- |
| [crmkit](https://github.com/crmkit/crmkit) | Server (crmkitd), CLI, and documentation |
| [skills](https://github.com/crmkit/skills) | Agent skill definitions                  |
