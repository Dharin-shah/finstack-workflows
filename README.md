# finstack-workflows

Pre-built n8n workflows powered by [finstack](https://github.com/Dharin-shah/finstack) skills. Import, add credentials, go.

## Workflows

### [`/ib`](./ib) — IB Analyst Workflow
Email-triggered investment banking research. Send a company name, get back a full IB-grade report with comps, valuation, M&A screening, and Excel attachment.

### [`/er`](./er) — Earnings Call Sentiment Workflow
Email-triggered earnings call sentiment analysis for Indian equities. Send a sector name, get back a ranked classification with top pick recommendation, credibility tracking, and sentiment charts.

**One import. Two credentials. Done.**

## How It Works

```
finstack (skills)              finstack-workflows (automation)
┌──────────────┐              ┌─────────────────────────────────────┐
│ ib/SKILL.md  │─── loaded ──▶│ Gmail → Fetch Skill → AI Agent     │
│ er/SKILL.md  │   at runtime │ → Build Report → Send Email + Excel │
│ risk/SKILL.md│              └─────────────────────────────────────┘
└──────────────┘
```

Skills live in [finstack](https://github.com/Dharin-shah/finstack). Workflows live here. Update a skill, every workflow picks it up automatically.

## Setup

1. Self-host n8n: `docker run -d -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n`
2. Open n8n at `http://localhost:5678`
3. Add credentials: OpenAI + Gmail (see workflow README for details)
4. Import the workflow JSON
5. Activate

## License

MIT
