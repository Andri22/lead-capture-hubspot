# 🚀 Lead Capture Automation — n8n + Make.com + HubSpot + OpenAI

Automated lead capture pipeline that validates incoming leads, creates contacts in HubSpot, enriches them with AI scoring, sends email notifications, and logs every execution — all without manual effort.

---

## 📌 Business Use Case

Sales teams waste hours manually entering leads into CRM, qualifying them, and notifying the right people. This workflow automates the entire process from form submission to qualified, logged CRM entry in seconds.

---

## ⚙️ Implementations

This workflow is available in two automation platforms — same logic, different tools:

| | n8n | Make.com |
|---|---|---|
| **Best for** | Technical clients | Non-technical clients |
| **Hosting** | Self-hosted, more control | Cloud-based, visual |
| **Cost** | Lower long-term | Per-operation pricing |
| **Flexibility** | Higher | Moderate |

---

## 🔧 Tech Stack

### n8n Version
n8n · HubSpot CRM · OpenAI GPT-4o-mini · Gmail · Google Sheets

### Make.com Version
Make.com · HubSpot CRM · OpenAI GPT-4o-mini · Gmail · Google Sheets

---

## ⚙️ Workflow Architecture

```
Webhook (POST)
   ↓
[Validation Layer]
   ├── Missing fields? → Log to error sheet → Stop
   └── Invalid email format? → Log to error sheet → Stop
        ↓
[HubSpot] Create or Update Contact
        ↓
[OpenAI] Summarize lead intent + score (hot/warm/cold)
        ↓
[Extract] Parse Summary + Score from AI response
        ↓
[HubSpot] Update contact with AI fields
        ↓
[Gmail] Send formatted HTML notification
        ↓
[Google Sheets] Log successful execution
```

---

## ✅ Features

- **Validation** — rejects missing fields and invalid email formats before touching CRM
- **Deduplication** — HubSpot upsert prevents duplicate contacts automatically
- **AI Enrichment** — GPT-4o-mini scores each lead as hot/warm/cold with a 2-sentence summary
- **HTML Email Notification** — formatted alert sent to sales team instantly
- **Dual Logging** — separate sheets for success and error logs with timestamps
- **Error Handling** — failed runs are caught and logged without breaking the workflow

---

## 📋 Webhook Payload

```json
{
  "first_name": "Budi",
  "last_name": "Santoso",
  "email": "budi@example.com",
  "phone": "+6281234567890",
  "message": "I need help automating my sales pipeline"
}
```

---

## 📊 Google Sheets Log Structure

**Success Sheet**

| timestamp | email | name | score | status |
|---|---|---|---|---|
| 15 May 2026, 13:05:00 | budi@example.com | Budi | hot | SUCCESS |

**Error Sheet**

| timestamp | email | status | error_message |
|---|---|---|---|
| 15 May 2026, 13:10:00 | invalid@@ | FAILED | Invalid email format |

---

## 🚀 Setup Guide

### 1. Prerequisites
- n8n instance (self-hosted or cloud) **or** Make.com account
- HubSpot free account
- OpenAI API key
- Gmail account
- Google Sheets

### 2. HubSpot Setup
1. Go to Settings → Integrations → Legacy Apps
2. Create a Private app with these scopes:
   - `crm.objects.contacts.write`
   - `crm.objects.contacts.read`
   - `crm.schemas.contacts.read`
3. Copy the access token

### 3. HubSpot Custom Properties
Create these in HubSpot → Settings → Properties → Contact Properties:

| Label | Internal Name | Type |
|---|---|---|
| AI Summary | `ai_summary` | Multi-line text |
| Lead Score | `lead_score` | Single-line text |

### 4. n8n Setup
1. Import `n8n/workflow.json` into n8n
2. Add credentials:
   - HubSpot (paste access token)
   - OpenAI (API key)
   - Gmail (OAuth)
   - Google Sheets (OAuth)
3. Update Google Sheets node with your Sheet ID
4. Activate workflow

### 5. Make.com Setup
1. Import `makecom/scenario.json` into Make.com
2. Add connections:
   - HubSpot (paste access token)
   - OpenAI (API key)
   - Gmail (OAuth)
   - Google Sheets (OAuth)
3. Update Google Sheets module with your Sheet ID
4. Activate scenario

---

## 📁 Repository Structure

```
lead-capture-hubspot/
├── n8n/
│   └── workflow.json          # n8n workflow export
├── makecom/
│   └── scenario.json          # Make.com scenario export
├── sample-payload.json        # Test webhook payload
└── README.md                  # This file
```

---

## 📝 License

MIT — free to use and modify for client projects.
