# Data Privacy Policy

How Voidly handles censorship measurement data.

## What We Collect

Voidly aggregates **network measurement data** from public sources. We do not collect personal information.

### From Public Sources

| Source | Data Type | PII Risk |
|--------|-----------|----------|
| OONI | Network test results (DNS, HTTP, TLS) | None — OONI publishes anonymized data |
| CensoredPlanet | DNS/HTTP blocking signals | None — automated remote measurements |
| IODA | BGP/Active Probing/Darknet signals | None — infrastructure-level data |

### From Voidly Probes

Community probe nodes submit:
- Domain accessibility results (reachable/blocked/timeout)
- Latency measurements
- Blocking method indicators (DNS, TCP, TLS)
- Country code (from node registration)

Probes do **not** submit: IP addresses, user identities, browsing history, or device information.

## What We Publish

All published data is **aggregated and anonymized**:

- Country-level censorship scores (not individual measurements)
- Incident reports with evidence links (to public OONI/IODA reports)
- Domain blocking status by country (not by individual network)
- ISP-level analysis (by ASN, not individual users)

## Data Licensing

All Voidly censorship data is published under **[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)**:

- Free to use, share, and adapt
- Attribution required: "Data from Voidly Global Censorship Index (voidly.ai)"
- No additional restrictions

## Data Retention

- Live measurements: retained indefinitely (aggregated)
- Incident reports: permanent (archival value)
- Probe results: retained for 90 days, then aggregated
- API request logs: 30 days (standard operational logging)

## Contact

For privacy questions: [research@voidly.ai](mailto:research@voidly.ai)
