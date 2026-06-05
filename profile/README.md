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

crmkit is an **agent-first CRM**, built for AI agents — ChatGPT, Claude, Cursor —
to drive directly. The agent _is_ the interface: it loads a one-page manual and
operates the CRM over plain HTTP. Headless by design (no UI). Responses are
grepable plain text by default; JSON on request.

```bash
# 1. begin login — a 6-digit code is emailed
curl -s -X POST https://api.crmkit.ai/auth/request -d '{"email":"you@example.com"}'

# 2. verify the code to get a token
curl -s -X POST https://api.crmkit.ai/auth/verify -d '{"email":"you@example.com","code":"123456"}'

# 3. operate the CRM
curl -s -X POST https://api.crmkit.ai/contacts \
  -H 'Authorization: Bearer ck_…' \
  -d '{"name":"Jane Doe","email":"jane@acme.com","stage":"lead"}'
```

Plain-text, grepable output — one labeled line per record:

```
contact/c_4u8a…  name="Jane Doe"  email=jane@acme.com  stage=lead  updated=2026-06-04
# 1 contact(s)
```

### Why crmkit

- **No UI to build or maintain** — the agent is the front end.
- **Plain text first** — token-cheap, grepable, JSON on `Accept`.
- **Instructive errors** — every failure tells the agent what to do next.
- **OTP auth, bearer tokens** — email a code, paste it, get a token.
- **Single static Go binary** — `CGO_ENABLED=0`, SQLite embedded, deploy as one file.
- **Extensible** — every record carries a free-form `custom` object.

### Repositories

| Repo                                       | Description                              |
| ------------------------------------------ | ---------------------------------------- |
| [crmkit](https://github.com/crmkit/crmkit) | Server (crmkitd), CLI, and documentation |
| [skills](https://github.com/crmkit/skills) | Agent skill definitions                  |
