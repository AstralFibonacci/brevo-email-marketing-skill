# Brevo Ops — Claude Agent Skill

Manage CRM contacts, email lists, marketing campaigns, and automations using the Brevo API. Designed for Claude agents that need autonomous email marketing operations.

---

## What This Skill Does

- Create and update CRM contacts
- Manage contact lists and segments
- Create and send email campaigns
- Send transactional emails
- Trigger marketing automations via custom events

---

## Requirements

- Brevo API key (`BREVO_API_KEY`) — get from Brevo Dashboard → Account → SMTP & API

```bash
pip install requests
```

**Base URL:** `https://api.brevo.com/v3`

**Auth Header:** `api-key: YOUR_BREVO_API_KEY`

---

## Contacts

### Create or Update a Contact

```python
import requests

headers = {
    "api-key": "YOUR_BREVO_API_KEY",
    "Content-Type": "application/json"
}

data = {
    "email": "contact@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "attributes": {
        "PHONE": "+1234567890",
        "COMPANY": "Acme Inc"
    },
    "listIds": [1, 2],
    "updateEnabled": True
}

response = requests.post("https://api.brevo.com/v3/contacts", json=data, headers=headers)
```

### Get All Contacts

```python
params = {"limit": 50, "offset": 0}
response = requests.get("https://api.brevo.com/v3/contacts", params=params, headers=headers)
```

### Get Single Contact

```python
response = requests.get("https://api.brevo.com/v3/contacts/contact@example.com", headers=headers)
```

---

## Lists

### Create a List

```python
data = {"name": "My List Name"}
response = requests.post("https://api.brevo.com/v3/contacts/lists", json=data, headers=headers)
# Returns: {"id": 123, "name": "My List Name"}
```

### Get All Lists

```python
response = requests.get("https://api.brevo.com/v3/contacts/lists", headers=headers)
```

### Add Contact to List

```python
data = {"emails": ["contact@example.com"]}
response = requests.post(
    "https://api.brevo.com/v3/contacts/lists/LIST_ID/addContacts",
    json=data,
    headers=headers
)
```

---

## Email Campaigns

### Create a Campaign

```python
data = {
    "name": "Campaign Name",
    "subject": "Email Subject",
    "sender": {"name": "Your Name", "email": "you@example.com"},
    "type": "classic",
    "htmlContent": "<html><body>Your content</body></html>",
    "recipients": {
        "listIds": [123]
    }
}
response = requests.post("https://api.brevo.com/v3/emailCampaigns", json=data, headers=headers)
```

### Send a Campaign Immediately

```python
campaign_id = 123
response = requests.post(
    f"https://api.brevo.com/v3/emailCampaigns/{campaign_id}/sendNow",
    headers=headers
)
```

### Schedule a Campaign

```python
data = {"scheduledAt": "2026-04-01T10:00:00Z"}
response = requests.post(
    f"https://api.brevo.com/v3/emailCampaigns/{campaign_id}/schedule",
    json=data,
    headers=headers
)
```

---

## Transactional Emails

```python
data = {
    "sender": {"name": "Your Agent", "email": "agent@example.com"},
    "to": [{"email": "recipient@example.com"}],
    "subject": "Your Subject",
    "htmlContent": "<html><body>Message</body></html>"
}
response = requests.post("https://api.brevo.com/v3/smtp/email", json=data, headers=headers)
```

---

## Automation Triggers

```python
data = {
    "email": "user@example.com",
    "eventName": "added_to_crm",
    "properties": {
        "source": "agent",
        "lead_status": "new"
    }
}
response = requests.post("https://api.brevo.com/v3/events", json=data, headers=headers)
```

---

## Sync Lead Helper

```python
def sync_lead_to_brevo(name, email, company, phone=None):
    headers = {
        "api-key": "YOUR_BREVO_API_KEY",
        "Content-Type": "application/json"
    }
    parts = name.split()
    data = {
        "email": email,
        "firstName": parts[0] if parts else "",
        "lastName": " ".join(parts[1:]) if len(parts) > 1 else "",
        "attributes": {"COMPANY": company or ""},
        "updateEnabled": True
    }
    if phone:
        data["attributes"]["PHONE"] = phone

    response = requests.post("https://api.brevo.com/v3/contacts", json=data, headers=headers)
    return response.json()
```

---

## Use Cases

1. Sync leads from CRM to Brevo contacts
2. Create and send email campaigns to list segments
3. Automated follow-up sequences
4. Newsletter delivery
5. Event-triggered transactional emails

---

## Security

- Store `BREVO_API_KEY` in environment variables
- Never hardcode credentials in agent prompts or skill files
