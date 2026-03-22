# Earnings Call Sentiment Workflow

Fully wrapped n8n workflow for Indian equity earnings call sentiment analysis. One import, two credentials, done.

## What You Get

Send an email with subject containing **"earnings call sentiment"** and a sector in the body. You get back:

- **Top Pick** with confidence score and detailed rationale
- Ranked classification of all companies in the sector
- Per-company sentiment breakdown, credibility tracker, forward catalysts
- Credibility vs Sentiment matrix (Trust & Act / Verify / Hidden Gem / Avoid)
- Sector heatmap and themes
- CSV attachment with all data (rankings, quarterly data, credibility tracker)
- Delivered to the sender's inbox

## Supported Sectors

| Email Body | Index |
|---|---|
| `NIFTY IT` or `IT` | TCS, Infosys, HCLTech, Wipro, Tech Mahindra, LTIMindtree, Persistent, Coforge, Mphasis, LTTS |
| `NIFTY BANK` or `banking` | HDFC Bank, ICICI Bank, Kotak, Axis, SBI, IndusInd, Bandhan, Federal, IDFC First, PNB, BoB, AU |
| `NIFTY PHARMA` or `pharma` | Sun Pharma, Dr Reddy's, Cipla, Divi's, Apollo Hospitals, Lupin, Torrent, Aurobindo, Biocon, Alkem |
| `NIFTY AUTO` or `auto` | M&M, Tata Motors, Maruti, Bajaj Auto, Hero, Eicher, Ashok Leyland, TVS, Bosch, MRF |
| `NIFTY FMCG` or `fmcg` | HUL, ITC, Nestle, Britannia, Godrej CP, Dabur, Marico, Colgate, Tata Consumer, VBL |
| `NIFTY METAL` or `metals` | Tata Steel, JSW Steel, Hindalco, Vedanta, Coal India, NMDC, SAIL, NALCO, Jindal Steel |
| `NIFTY ENERGY` or `energy` | Reliance, NTPC, Power Grid, ONGC, BPCL, IOC, GAIL, Adani Green, Tata Power |
| `NIFTY REALTY` or `realty` | DLF, Godrej Properties, Oberoi, Prestige, Phoenix Mills, Brigade, Sobha |
| `NIFTY FIN SERVICE` or `financial services` | HDFC Bank, ICICI Bank, Bajaj Finance, Bajaj Finserv, SBI Life, HDFC Life, HDFC AMC, SBI Cards, Chola, Muthoot |

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
  → Subject filter ("earnings call sentiment")
  → Sanitize input + extract sector from body
  → Fetch ER skill from finstack repo (always latest)
  → GPT-4o runs 7-phase earnings analysis for all companies
  → Build styled HTML report + CSV
  → Email report back with attachment
```

The skill is fetched live from [finstack/er/SKILL.md](https://github.com/Dharin-shah/finstack/blob/main/er/SKILL.md). When the skill gets updated, your workflow automatically uses the latest version.

## The Analysis

For each company in the sector, the agent runs:

1. **Earnings Metrics** — Revenue, EBITDA, PAT, EPS per quarter (QoQ + YoY)
2. **Management Commentary** — Key statements extracted with sentiment scores
3. **Sentiment Scoring** — Composite score (-100 to +100) across 5 dimensions
4. **Credibility Assessment** — "What they said" vs "what they delivered" per quarter
5. **Confidence Classification** — Final score (0-100) mapped to tiers
6. **Cross-Company Ranking** — All companies ranked, top pick selected with rationale
7. **Forward Catalysts** — Upcoming triggers with impact and probability

### Confidence Tiers

| Tier | Score | Meaning |
|---|---|---|
| HIGH CONVICTION BUY SIGNAL | 75-100 | Bullish management + strong track record + improving fundamentals |
| POSITIVE | 55-74 | Good signals, some uncertainty |
| NEUTRAL | 40-54 | Mixed signals |
| CAUTIOUS | 20-39 | Concerning trends or credibility issues |
| HIGH CONVICTION AVOID | 0-19 | Bearish + poor track record + deteriorating fundamentals |

## Test It

Send this email to your connected Gmail:

```
Subject: earnings call sentiment
Body: NIFTY IT
```

Or try other sectors:

```
Subject: earnings call sentiment
Body: banking last 6 quarters
```

## Customization

| Want to... | Change... |
|---|---|
| Different trigger subject | Edit the "Check Subject" filter node |
| Different LLM | Swap the OpenAI node for Anthropic/other |
| Add more sectors | Edit the sectorMap in "Sanitize & Extract Sector" node |
| Change confidence thresholds | Update the skill in finstack repo |
| Add Slack notification | Insert a Slack node after "Build Report" |
| Add more tools (financial APIs) | Connect HTTP Request tools to the agent |
