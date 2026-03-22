# IB Analyst Workflow

Fully wrapped n8n workflow for Investment Banking research. One import, two credentials, done.

## What You Get

Send an email with subject containing **"IB Research"** and company names in the body. You get back:

- Professional HTML report (comps, valuation, SWOT, M&A, thesis)
- Excel/CSV attachment with all financial data
- Delivered to the sender's inbox

## Setup (5 minutes)

### 1. Import
Open n8n → Workflows → Import from file → select `workflow.json`

### 2. Credentials
You need exactly two:

| Credential | How |
|---|---|
| **OpenAI** | Credentials → Add → OpenAI → paste API key |
| **Gmail** | Credentials → Add → Gmail OAuth2 → [setup guide](https://docs.n8n.io/integrations/builtin/credentials/google/oauth-single-service/) |

### 3. Activate
Toggle the workflow to active. That's it.

## How It Works

```
Email arrives
  → Subject filter ("IB Research")
  → Sanitize input (prompt injection protection)
  → Fetch IB skill from finstack repo
  → GPT-4o runs the 8-phase IB analysis
  → Build styled HTML report + CSV
  → Email report back with attachment
```

The skill is fetched live from [finstack/ib/SKILL.md](https://github.com/Dharin-shah/finstack/blob/main/ib/SKILL.md). When the skill gets updated, your workflow automatically uses the latest version.

## Test It

Send this email to your connected Gmail:

```
Subject: IB Research
Body: Analyze Stripe, Plaid, and Adyen for a fintech sector overview
```

## Customization

| Want to... | Change... |
|---|---|
| Different trigger subject | Edit the "Check Subject" filter node |
| Different LLM | Swap the OpenAI node for Anthropic/other |
| Add Slack approval | Insert a Wait node before email send |
| Add more tools | Connect HTTP Request tools to financial APIs |
| Change output format | Edit the "Build Report" code node |
