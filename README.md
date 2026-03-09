# brevo-email-marketing-skill

An agent skill for managing CRM contacts, email campaigns, and marketing automations via the [Brevo](https://brevo.com) API. Compatible with Claude Code, OpenClaw, and any agent framework that supports skill files.

## What it does

- Create and update CRM contacts with custom attributes
- Manage contact lists and segments
- Create and send email campaigns
- Send transactional emails
- Trigger marketing automations via custom events

## When to use this skill

- An agent needs to sync leads into a CRM automatically
- You want to trigger email campaigns or sequences from agent workflows
- You're building a pipeline: collect leads → enrich → add to Brevo → send campaign

## Requirements

| Requirement | Details |
|-------------|---------|
| Brevo account | [brevo.com](https://brevo.com) |
| API key | `BREVO_API_KEY` env var |
| Python | `pip install requests` |

## Structure

```
brevo-email-marketing-skill/
├── README.md     ← you are here (overview)
└── SKILL.md      ← full implementation guide for agents
```

## Quick example

```python
import requests

requests.post(
    "https://api.brevo.com/v3/contacts",
    headers={"api-key": "YOUR_BREVO_API_KEY", "Content-Type": "application/json"},
    json={"email": "lead@example.com", "firstName": "Jane", "updateEnabled": True}
)
```

See [`SKILL.md`](./SKILL.md) for the full implementation guide.

## Tags

`ai-agent` `brevo` `email-marketing` `crm` `automation` `campaigns` `contacts` `transactional-email` `claude` `openclaw` `agent-skill` `sales-automation`
