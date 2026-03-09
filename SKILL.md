---
name: brevo-email-marketing-skill
description: Manage CRM contacts, email lists, campaigns, and automations via Brevo API. Use when an agent needs to sync leads, send campaigns, or trigger email automations.
tags: [brevo, email-marketing, crm, automation, campaigns, contacts]
---

# Brevo Email Marketing Skill

## Purpose

Give agents the ability to manage contacts and run email marketing operations via the Brevo REST API.

---

## Setup

```bash
pip install requests
export BREVO_API_KEY=your_key_here
```

Get your API key: Brevo Dashboard → Account → SMTP & API

**Base URL:** `https://api.brevo.com/v3`
**Auth header:** `api-key: YOUR_BREVO_API_KEY`

---

## Contacts

### Create or Update

```python
import requests

headers = {
    "api-key": "YOUR_BREVO_API_KEY",
    "Content-Type": "application/json"
}

data = {
    "email": "contact@example.com",
    "firstName": "Jane",
    "lastName": "Doe",
    "attributes": {
        "PHONE": "+1234567890",
        "COMPANY": "Acme Inc"
    },
    "listIds": [1, 2],
    "updateEnabled": True  # Update if contact already exists
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
response = requests.get(
    "https://api.brevo.com/v3/contacts/contact@example.com",
    headers=headers
)
```

---

## Lists

### Create a List

```python
response = requests.post(
    "https://api.brevo.com/v3/contacts/lists",
    json={"name": "My Segment"},
    headers=headers
)
# Returns: {"id": 123, "name": "My Segment"}
```

### Add Contact to List

```python
requests.post(
    "https://api.brevo.com/v3/contacts/lists/LIST_ID/addContacts",
    json={"emails": ["contact@example.com"]},
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
    "htmlContent": "<html><body>Your content here</body></html>",
    "recipients": {"listIds": [123]}
}
response = requests.post("https://api.brevo.com/v3/emailCampaigns", json=data, headers=headers)
campaign_id = response.json()["id"]
```

### Send Immediately

```python
requests.post(f"https://api.brevo.com/v3/emailCampaigns/{campaign_id}/sendNow", headers=headers)
```

### Schedule

```python
requests.post(
    f"https://api.brevo.com/v3/emailCampaigns/{campaign_id}/schedule",
    json={"scheduledAt": "2026-04-01T10:00:00Z"},
    headers=headers
)
```

---

## Transactional Emails

```python
data = {
    "sender": {"name": "Agent", "email": "agent@yourdomain.com"},
    "to": [{"email": "recipient@example.com"}],
    "subject": "Your Subject",
    "htmlContent": "<html><body>Your message</body></html>"
}
requests.post("https://api.brevo.com/v3/smtp/email", json=data, headers=headers)
```

---

## Automation Triggers

```python
data = {
    "email": "user@example.com",
    "eventName": "lead_added",
    "properties": {
        "source": "agent-workflow",
        "lead_status": "new"
    }
}
requests.post("https://api.brevo.com/v3/events", json=data, headers=headers)
```

---

## Sync Lead Helper

```python
def sync_lead(name, email, company, phone=None):
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

    return requests.post(
        "https://api.brevo.com/v3/contacts",
        json=data,
        headers={"api-key": "YOUR_BREVO_API_KEY", "Content-Type": "application/json"}
    ).json()
```

---

## Use Cases

1. Sync agent-collected leads into Brevo CRM
2. Trigger campaigns when lead status changes
3. Automated follow-up sequences
4. Newsletter delivery
5. Event-triggered transactional emails

---

## Security

- Store `BREVO_API_KEY` in environment variables — never hardcode
