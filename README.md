# Voidly Censorship Research

**The authoritative source for global internet censorship data.**

[![Data](https://img.shields.io/badge/measurements-11.7M-blue)](https://voidly.ai/data)
[![Incidents](https://img.shields.io/badge/incidents-5,356+-orange)](https://voidly.ai/censorship-index)
[![Countries](https://img.shields.io/badge/countries-119-green)](https://voidly.ai/censorship-index)
[![License](https://img.shields.io/badge/license-CC%20BY%204.0-lightgrey)](LICENSE)

---

## What is This?

This repository contains documentation and architecture overview for Voidly's censorship intelligence platform. The actual data is served via our public API and available on HuggingFace.

**Voidly aggregates data from:**
- [OONI](https://ooni.org) — Open Observatory of Network Interference (8 test types, 80 countries)
- [IODA](https://ioda.inetintel.cc.gatech.edu/) — Internet Outage Detection and Analysis (ASN-level)
- [Censored Planet](https://censoredplanet.org/) — Satellite DNS + Hyperquack HTTP/S (50 countries)
- **Voidly Probes** — 16 global nodes, 62 domains, 5-minute intervals

---

## Data Scale

| Metric | Value |
|--------|-------|
| Live Measurements | 11.7M |
| Historical Records | 1.6M (10-year archive, 120 countries) |
| Documented Incidents | 5,356+ (citable with evidence) |
| Evidence Items | 16,822 (OONI + IODA + CensoredPlanet) |
| Countries Tracked | 119 |
| Data Sources | 4 |
| Update Frequency | Every 30 minutes |

---

## Quick Start

### REST API (No Auth Required)

```bash
# Global censorship index
curl https://api.voidly.ai/data/censorship-index.json

# Country detail
curl https://api.voidly.ai/data/country/IR

# List incidents
curl "https://api.voidly.ai/data/incidents?limit=10&country=IR"

# Single incident (human-readable ID)
curl https://api.voidly.ai/data/incidents/IR-2026-0142

# Verify a censorship claim
curl -X POST https://api.voidly.ai/verify-claim \
  -H "Content-Type: application/json" \
  -d '{"claim": "Twitter is blocked in Iran"}'

# Domain accessibility check
curl "https://api.voidly.ai/v1/accessibility/check?domain=twitter.com&country=IR"

# 7-day risk forecast
curl https://api.voidly.ai/v1/forecast/IR/7day
```

### Python

```python
import requests

# Get most censored countries
data = requests.get("https://api.voidly.ai/data/censorship-index.json").json()
for c in data["countries"][:10]:
    print(f"{c['rank']}. {c['country']}: {c['score']}/100")

# Check if a domain is blocked
check = requests.get(
    "https://api.voidly.ai/v1/accessibility/check",
    params={"domain": "twitter.com", "country": "IR"}
).json()
print(f"Twitter in Iran: {check['status']}")
```

### JavaScript

```javascript
const data = await fetch("https://api.voidly.ai/data/censorship-index.json").then(r => r.json());
console.log(`Tracking ${data.metadata.totalCountries} countries`);
console.log(`Based on ${data.metadata.liveMeasurements.toLocaleString()} measurements`);
```

---

## Intelligence Products

| Product | Endpoint | Description |
|---------|----------|-------------|
| Censorship Index | `/data/censorship-index.json` | Live country rankings |
| Incident Database | `/data/incidents` | 5,356+ citable incidents |
| Claim Verification | `/verify-claim` | Fact-check censorship claims |
| Domain Risk | `/v1/accessibility/check` | Is domain X blocked in Y? |
| 7-Day Forecast | `/v1/forecast/{country}/7day` | Shutdown risk predictions |
| Platform Risk | `/v1/platform/{name}/risk` | Per-platform censorship scores |
| ISP Risk Index | `/v1/isp/index?country={code}` | ISP censorship scoring |
| Election Briefings | `/v1/elections/upcoming` | Election-censorship correlation |
| Real-Time Alerts | `/api/alerts/subscribe` | Webhook notifications |

Full API docs: [voidly.ai/api-docs](https://voidly.ai/api-docs)

---

## AI Integrations

| Platform | Integration | Link |
|----------|-------------|------|
| Claude / Cursor / Windsurf | MCP Server (27 tools) | `npx @voidly/mcp-server` |
| ChatGPT | OpenAI Action | [voidly-ai/chatgpt-action](https://github.com/voidly-ai/chatgpt-action) |
| HuggingFace (live) | Dataset | [global-censorship-index](https://huggingface.co/datasets/emperor-mew/global-censorship-index) |
| HuggingFace (archive) | Dataset | [ooni-censorship-historical](https://huggingface.co/datasets/emperor-mew/ooni-censorship-historical) |
| Any LLM | llms.txt | [voidly.ai/llms.txt](https://voidly.ai/llms.txt) |

---

## Repository Structure

```
voidly-public/
├── README.md             # This file
├── docs/
│   ├── architecture.md   # High-level system architecture
│   ├── privacy.md        # Data privacy and aggregation policy
│   └── roadmap.md        # Current status and what's next
├── CONTRIBUTING.md       # How to contribute
└── LICENSE               # CC BY 4.0
```

---

## Citing Voidly

```bibtex
@misc{voidly2026,
  title  = {Voidly Global Censorship Index},
  author = {{Voidly Research}},
  year   = {2026},
  url    = {https://voidly.ai/censorship-index}
}
```

Individual incidents are citable:
```
Voidly Research. (2026). Censorship Incident IR-2026-0142.
Retrieved from https://voidly.ai/incident/IR-2026-0142
```

Export citations in BibTeX, RIS, Markdown, or JSON from any incident page.

---

## Links

- [voidly.ai](https://voidly.ai) — Website
- [voidly.ai/censorship-index](https://voidly.ai/censorship-index) — Live Rankings
- [voidly.ai/live](https://voidly.ai/live) — Real-Time Incident Feed
- [voidly.ai/data](https://voidly.ai/data) — Open Data Hub
- [voidly.ai/api-docs](https://voidly.ai/api-docs) — API Documentation
- [voidly.ai/methodology](https://voidly.ai/methodology) — How We Measure

---

## License

- **Data:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — Free to use with attribution
- **Code Examples:** MIT License
