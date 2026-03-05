# Voidly Censorship Intelligence Platform

**Making internet censorship measurable, verifiable, and actionable.**

[![Samples](https://img.shields.io/badge/samples-16.9M-blue)](https://voidly.ai/data)
[![Incidents](https://img.shields.io/badge/incidents-351+-orange)](https://voidly.ai/censorship-index)
[![Countries](https://img.shields.io/badge/countries-126-green)](https://voidly.ai/censorship-index)
[![Probes](https://img.shields.io/badge/probes-39+-purple)](https://voidly.ai/probes)
[![License](https://img.shields.io/badge/data-CC%20BY%204.0-lightgrey)](LICENSE)

---

## What is Voidly?

Voidly is a censorship intelligence platform that aggregates data from multiple measurement networks, applies ML classification, and makes the results accessible to researchers, journalists, policymakers, and AI systems.

**Data sources:**
- [OONI](https://ooni.org) — Open Observatory of Network Interference (8 test types, 80+ countries)
- [IODA](https://ioda.inetintel.cc.gatech.edu/) — Internet Outage Detection and Analysis (ASN-level)
- [Censored Planet](https://censoredplanet.org/) — Satellite DNS + Hyperquack HTTP/S (50 countries)
- **Voidly Probe Network** — 39+ global nodes, 62 domains, 5-minute intervals

---

## Data Scale

| Metric | Value |
|--------|-------|
| Live Samples | 16.9M |
| Historical Records | 1.6M (10-year archive) |
| Verified Incidents | 351+ (ML-classified, citable) |
| Evidence Items | 27,000+ |
| Countries | 126 |
| Probe Nodes | 39+ (6 continents) |
| Total Measurements | 2.2B+ aggregated |
| Platform Users | 56,100+ |

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

# RSS feed
curl https://api.voidly.ai/data/incidents/feed.rss
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
| Incident Database | `/data/incidents` | 351+ citable incidents with evidence |
| Claim Verification | `/verify-claim` | ML-powered fact-checking |
| Domain Accessibility | `/v1/accessibility/check` | Is domain X blocked in Y? |
| 7-Day Forecast | `/v1/forecast/{country}/7day` | Shutdown risk predictions |
| Platform Risk | `/v1/platform/{name}/risk` | Per-platform censorship scores |
| ISP Risk Index | `/v1/isp/index?country={code}` | ISP censorship scoring |
| Election Briefings | `/v1/elections/upcoming` | Election-censorship correlation |
| Real-Time Alerts | `/api/alerts/subscribe` | Webhook notifications |
| Bulk Export | `/data/incidents/export?format=csv` | CSV, JSONL, JSON |
| RSS/Atom Feeds | `/data/incidents/feed.rss` | Real-time incident feeds |

Full API docs: [voidly.ai/api-docs](https://voidly.ai/api-docs)

---

## AI & Developer Tools

| Platform | Package / Integration | Description |
|----------|----------------------|-------------|
| Claude / Cursor / Windsurf | [`@voidly/mcp-server`](https://www.npmjs.com/package/@voidly/mcp-server) | **83 MCP tools** — censorship data + agent relay |
| Agent Messaging | [`@voidly/agent-sdk`](https://www.npmjs.com/package/@voidly/agent-sdk) | E2E encrypted agent-to-agent communication |
| ChatGPT | [OpenAI Action](https://github.com/voidly-ai/voidly-public/tree/main/openai-action) | GPT Builder action spec |
| HuggingFace | [global-censorship-index](https://huggingface.co/datasets/emperor-mew/global-censorship-index) | Live dataset (JSON) |
| HuggingFace | [ooni-censorship-historical](https://huggingface.co/datasets/emperor-mew/ooni-censorship-historical) | 10-year archive (Parquet) |
| Any LLM | [llms.txt](https://voidly.ai/llms.txt) | LLM-friendly context file |

### Install MCP Server

```bash
npx @voidly/mcp-server
```

### Install Agent SDK

```bash
npm install @voidly/agent-sdk
```

---

## Public Repositories

| Repository | Description |
|------------|-------------|
| [voidly-ai/voidly-public](https://github.com/voidly-ai/voidly-public) | This repo — docs, architecture, API overview |
| [voidly-ai/mcp-server](https://github.com/voidly-ai/mcp-server) | MCP server source (83 tools) |
| [voidly-ai/agent-sdk](https://github.com/voidly-ai/agent-sdk) | Agent SDK docs + examples |
| [voidly-ai/community-probe](https://github.com/voidly-ai/community-probe) | Community probe node |
| [voidly-ai/voidly-probe-app](https://github.com/voidly-ai/voidly-probe-app) | Desktop probe app (Tauri) |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                     voidly.ai                           │
│              (Next.js on Cloudflare Pages)               │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                 api.voidly.ai                           │
│           (Cloudflare Worker + D1)                       │
│                                                         │
│  Endpoints:                                             │
│  ├── /data/*          Censorship data + incidents       │
│  ├── /v1/forecast/*   Shutdown risk predictions         │
│  ├── /v1/platform/*   Platform risk scores              │
│  ├── /v1/isp/*        ISP censorship index              │
│  ├── /v1/elections/*  Election risk briefings           │
│  ├── /v1/probe/*      Probe network status              │
│  ├── /v1/agent/*      Agent Relay (E2E encrypted)       │
│  └── /api/alerts/*    Real-time alert subscriptions     │
└────┬────────────┬───────────────────────────────────────┘
     │            │
     ▼            ▼
┌─────────┐  ┌──────────────┐  ┌───────────────────────┐
│ ML Model│  │ Probe Network│  │   Data Ingestion      │
│ (v2)    │  │ (39+ nodes)  │  │                       │
│         │  │ 62 domains   │  │ OONI + CensoredPlanet │
│ 99.8%   │  │ every 5 min  │  │ + IODA (every 6h)     │
│ F1 score│  │ 6 continents │  │                       │
└─────────┘  └──────────────┘  └───────────────────────┘
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

Individual incidents are citable by human-readable ID:
```
Voidly Research. (2026). Censorship Incident IR-2026-0142.
Retrieved from https://voidly.ai/censorship-index/incidents/IR-2026-0142
```

Export citations in BibTeX, RIS, Markdown, or JSON from any incident page.

---

## Support Voidly

Voidly is independently funded. If you find this useful, consider supporting continued development:

- **ETH / Base**: `0x6E04f0c02A7838440FE9c0EB06C7556D66e00598` (ENS: `voidly.base.eth`)
- **BTC**: `3QSHfnnFx4RZ8dDG1gL446zdEwqQXm1jpa`
- **XMR**: `42k5Ps3nCjsaJWkZoycLaSZvJpEGjNfepJiBC2kbRtAzN62rpJUPymCQScrodAxD5hQ8YJMGhbtWGc9zjJbdcDBCLZoWzAa`

---

## Links

- [voidly.ai](https://voidly.ai) — Website
- [voidly.ai/censorship-index](https://voidly.ai/censorship-index) — Live Rankings
- [voidly.ai/report](https://voidly.ai/report) — Global Report
- [voidly.ai/live](https://voidly.ai/live) — Real-Time Feed
- [voidly.ai/data](https://voidly.ai/data) — Open Data Hub
- [voidly.ai/mcp](https://voidly.ai/mcp) — MCP Tools Reference
- [voidly.ai/agents](https://voidly.ai/agents) — Agent Relay
- [voidly.ai/api-docs](https://voidly.ai/api-docs) — API Documentation
- [voidly.ai/methodology](https://voidly.ai/methodology) — How We Measure
- [hello@voidly.ai](mailto:hello@voidly.ai) — Contact

---

## License

- **Data:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — Free to use with attribution
- **Code Examples:** MIT License

Operated by Ai Analytics LLC · Founded by Dillon Parkes
