# Architecture Overview

High-level overview of the Voidly censorship intelligence platform.

## Data Pipeline

```
┌─────────────┐    ┌──────────────┐    ┌───────────────┐    ┌──────────┐
│ Data Sources │───▶│  Ingestion   │───▶│  ML Pipeline  │───▶│  Public  │
│             │    │  (scheduled) │    │  (classify)   │    │   API    │
└─────────────┘    └──────────────┘    └───────────────┘    └──────────┘
```

### 1. Data Sources

| Source | What We Collect | Frequency |
|--------|-----------------|-----------|
| **OONI** | 8 test types: web_connectivity, signal, whatsapp, telegram, facebook_messenger, tor, http_invalid_request_line, http_header_field_manipulation | 4x daily (tiered) |
| **CensoredPlanet** | Satellite DNS blocking + Hyperquack HTTP/S blocking | 4x daily |
| **IODA** | ASN-level outage alerts | 4x daily |
| **Voidly Probes** | Direct connectivity testing (16 nodes, 62 domains) | Every 5 min |
| **Social Signals** | Cross-correlation of OONI anomalies + IODA alerts | Every 2 hours |

### 2. Ingestion & Enrichment

Raw measurements are collected, deduplicated, and enriched:

- **Geographic enrichment** — continent, UN region, risk tier (1–5)
- **Domain categorization** — Citizen Lab test lists (14K domains: NEWS, ANON, GRP, etc.)
- **Blocking method detection** — dns-poison, tcp-reset, blockpage, tls-reset
- **ASN attribution** — ISP-level blocking identification

### 3. ML Classification

A supervised classifier separates real censorship from network noise:

- Trained on 37K labeled incidents (4.5K confirmed censorship)
- Multiple signal types as input features
- Country risk tier as primary feature
- Produces confidence scores per incident

### 4. Incident Pipeline

Classified events become citable incidents:

- Human-readable IDs: `{COUNTRY}-{YEAR}-{SEQ}` (e.g., `IR-2026-0142`)
- Evidence linkage to source measurements (OONI, IODA, CensoredPlanet)
- Severity classification (info, warning, critical)
- Real-time alert delivery via webhooks

### 5. Public API

All data served via `api.voidly.ai`:

- Censorship index (country rankings)
- Incident database (5,356+ incidents)
- Domain accessibility checks
- Risk forecasting (7-day)
- Platform and ISP risk scores
- Bulk export (CSV, JSONL)
- RSS/Atom feeds

## Predictive Layer

A separate forecasting model provides 7-day shutdown risk predictions:

- Uses historical censorship patterns + event calendar (elections, protests)
- Retrained weekly on real ground truth
- Integrated with election risk briefings

## Data Flow

```
OONI ──────┐
            │
CensoredP ─┤
            ├──▶ Ingest ──▶ Enrich ──▶ Classify ──▶ Incidents ──▶ API
IODA ──────┤                                                      │
            │                                            ┌────────┤
Probes ────┘                                             │        │
                                                    Alerts    Feeds
                                                  (webhooks)  (RSS)
```

## Interfaces

| Interface | Who It's For |
|-----------|-------------|
| [Web UI](https://voidly.ai) | Researchers, journalists, general public |
| [REST API](https://voidly.ai/api-docs) | Developers, researchers |
| [MCP Server](https://github.com/voidly-ai/mcp-server) | AI assistants (Claude, Cursor) |
| [ChatGPT Action](https://github.com/voidly-ai/chatgpt-action) | ChatGPT users |
| [HuggingFace](https://huggingface.co/datasets/emperor-mew/global-censorship-index) | ML researchers |
| [RSS/Atom](https://voidly.ai/incidents/feed.xml) | News aggregators |
