# AI-Powered Routing Architecture

> How Voidly's machine learning system selects optimal VPN routes

---

## Overview

Traditional VPNs use simple routing:
- Round-robin (connect to next server in list)
- Lowest latency (ping-based selection)
- Geographic proximity
- Random selection

**The problem:** These approaches ignore censorship, user patterns, and real-world connection success rates.

Voidly uses machine learning to make smarter routing decisions based on real data.

---

## Model Architecture

### Algorithm: XGBoost (Gradient Boosted Trees)

We chose XGBoost for:
- Fast inference (< 50ms predictions)
- Interpretable results (we can explain why routes are chosen)
- Handles non-linear relationships well
- Works with limited training data

### Model: `xgboost_v2.pkl`

**Current version:** 2.0 (deployed January 2025)  
**Training frequency:** Every 6 hours  
**Training data:** 8,429+ real connection attempts  
**Features:** 23 dimensions  

---

## Input Features

The model considers 23 different factors to choose the optimal node:

### User Context (Anonymized)
- Source region (continent-level only)
- Time of day (UTC hour)
- Day of week
- Estimated censorship risk score

### Node Characteristics
- Geographic location
- Current load (active connections)
- Historical uptime (30-day average)
- Average latency from region
- Bandwidth capacity

### Historical Performance
- Success rate from user's region (7-day window)
- Recent connection failures
- Time-to-connect average
- Packet loss rate
- Throughput metrics

### Censorship Intelligence
- Known blocks from region
- DPI detection events
- Recent censorship reports
- Obfuscation effectiveness score

---

## Output

The model predicts:

1. **Recommended Node** - Best server for this connection
2. **Success Probability** - Likelihood of successful connection (0-100%)
3. **Confidence Score** - How certain the model is (0-100%)
4. **Alternative Routes** - 2nd and 3rd best options with scores

Example output:
```json
{
  "recommendation": {
    "node": "eu-west-1",
    "reason": "Optimal latency and 94% success rate from your region",
    "successRate": 94.2,
    "confidence": 87.5
  },
  "alternatives": [
    {"node": "us-east-1", "score": 89.1},
    {"node": "ap-southeast-1", "score": 78.3}
  ],
  "reasoning": [
    "12ms faster than nearest alternative",
    "94% success rate from your region (last 7 days)",
    "Low censorship risk (0.02 probability)",
    "Node health: 99.8% uptime"
  ]
}
