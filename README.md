# Voidly Censorship Research

**The authoritative source for global internet censorship data.**

[![Data](https://img.shields.io/badge/measurements-11.7M-blue)](https://voidly.ai/data)
[![Incidents](https://img.shields.io/badge/incidents-4,481-orange)](https://voidly.ai/censorship-index/incidents)
[![Countries](https://img.shields.io/badge/countries-51-green)](https://voidly.ai/censorship-index)
[![License](https://img.shields.io/badge/license-CC%20BY%204.0-lightgrey)](LICENSE)

---

## What is This?

This repository contains documentation, schemas, and examples for working with Voidly's censorship intelligence data. The actual data is served via our API and available on HuggingFace.

**Voidly aggregates data from:**
- [OONI](https://ooni.org) - Open Observatory of Network Interference
- [IODA](https://ioda.inetintel.cc.gatech.edu/) - Internet Outage Detection and Analysis
- [Censored Planet](https://censoredplanet.org/) - University of Michigan censorship observatory
- **Voidly Probes** - Our own network monitoring infrastructure

---

## Data Scale

| Metric | Value |
|--------|-------|
| Live Measurements | 11.7M |
| Historical Records | 1.6M (10-year archive) |
| Labeled Incidents | 4,481 |
| Countries Tracked | 51 |
| Data Sources | 4 |
| Update Frequency | Every 5 minutes |

---

## Quick Start

### REST API (No Auth Required)

```bash
# Global censorship index
curl https://api.voidly.ai/data/censorship-index.json

# Single country
curl https://api.voidly.ai/data/country/IR

# Methodology
curl https://api.voidly.ai/data/methodology
```

### Python

```python
import requests

# Get most censored countries
response = requests.get("https://api.voidly.ai/data/censorship-index.json")
data = response.json()

for country in data["countries"][:10]:
    print(f"{country['rank']}. {country['country']}: {country['score']}/100")
```

### JavaScript

```javascript
const response = await fetch("https://api.voidly.ai/data/censorship-index.json");
const data = await response.json();

console.log(`Tracking ${data.metadata.totalCountries} countries`);
console.log(`Based on ${data.metadata.liveMeasurements.toLocaleString()} measurements`);
```

---

## AI Integrations

| Platform | Integration | Link |
|----------|-------------|------|
| Claude / Cursor | MCP Server | [voidly-ai/mcp-server](https://github.com/voidly-ai/mcp-server) |
| ChatGPT | OpenAI Action | [voidly-ai/chatgpt-action](https://github.com/voidly-ai/chatgpt-action) |
| HuggingFace | Datasets | [emperor-mew/global-censorship-index](https://huggingface.co/datasets/emperor-mew/global-censorship-index) |

---

## Repository Structure

```
voidly-public/
├── README.md          # This file
├── docs/
│   ├── api.md         # Full API documentation
│   ├── methodology.md # How we measure censorship
│   └── sources.md     # Data source details
├── schemas/
│   ├── incident.json  # Censorship incident schema
│   ├── country.json   # Country data schema
│   └── api.json       # API response schemas
├── examples/
│   ├── python/        # Python examples
│   └── javascript/    # JS/Node examples
└── data-samples/      # Sample data files
```

---

## Citing Voidly

If you use Voidly data in research or publications:

```bibtex
@misc{voidly2026,
  title={Voidly Global Censorship Index},
  author={Voidly Research},
  year={2026},
  url={https://voidly.ai/censorship-index}
}
```

For individual incidents:
```
Voidly Censorship Incident #{id}. Retrieved from https://voidly.ai/incident/{id}
```

---

## Links

- **Website:** [voidly.ai](https://voidly.ai)
- **Censorship Index:** [voidly.ai/censorship-index](https://voidly.ai/censorship-index)
- **Incidents Database:** [voidly.ai/censorship-index/incidents](https://voidly.ai/censorship-index/incidents)
- **API Docs:** [voidly.ai/api-docs](https://voidly.ai/api-docs)
- **Open Data Hub:** [voidly.ai/data](https://voidly.ai/data)
- **Methodology:** [voidly.ai/methodology](https://voidly.ai/methodology)

---

## License

- **Data:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) - Free to use with attribution
- **Code Examples:** MIT License

---

*Maintained by [Voidly Research](https://voidly.ai) - Building the world's most comprehensive censorship intelligence.*
