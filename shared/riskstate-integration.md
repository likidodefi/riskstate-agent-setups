# How to wire RiskState into any agent

Add risk governance to your agent in 5 minutes. This guide applies to any framework, any data source, and any execution layer.

---

## 1. Get an API key

Sign up at [riskstate.ai/#get-started](https://riskstate.ai/#get-started). Free during beta (100 calls/month).

## 2. Call the API

```bash
curl -X POST https://riskstate.netlify.app/v1/risk-state \
  -H "Authorization: Bearer $RISKSTATE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"asset": "BTC"}'
```

Supports `BTC` and `ETH`. Response is cached for 60 seconds. Recommended polling: every 5 minutes.

## 3. Read the policy

The response contains everything your agent needs:

```json
{
  "exposure_policy": {
    "max_size_fraction": 0.42,
    "leverage_allowed": true,
    "allowed_actions": ["DCA", "LONG_SHORT_CONFIRMED"],
    "blocked_actions": ["ALL_IN", "LEVERAGE_GT_2X"]
  },
  "risk_flags": {
    "structural_blockers": [],
    "context_risks": ["HIGH_COUPLING", "TREND_NOT_CONFIRMED"]
  },
  "policy_level": 4
}
```

## 4. Gate your execution

### Python

```python
import requests

def get_risk_policy(asset="BTC"):
    resp = requests.post(
        "https://riskstate.netlify.app/v1/risk-state",
        headers={"Authorization": f"Bearer {RISKSTATE_API_KEY}"},
        json={"asset": asset}
    )
    return resp.json()

policy = get_risk_policy("BTC")

# Hard stop: structural blockers
if policy["risk_flags"]["structural_blockers"]:
    print("BLOCKED:", policy["risk_flags"]["structural_blockers"])
    return  # Do not trade

# Size gate
max_size = policy["exposure_policy"]["max_size_fraction"]
intended_size = 0.30  # 30% of portfolio

if intended_size > max_size:
    intended_size = max_size  # Cap to policy limit

# Action gate
if "DCA" not in policy["exposure_policy"]["allowed_actions"]:
    return  # Action not permitted right now

# Proceed with execution at intended_size
execute_trade(asset="BTC", size=intended_size)
```

### JavaScript

```javascript
const response = await fetch("https://riskstate.netlify.app/v1/risk-state", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${RISKSTATE_API_KEY}`,
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ asset: "BTC" })
});

const policy = await response.json();

// Hard stop
if (policy.risk_flags.structural_blockers.length > 0) {
  console.log("BLOCKED:", policy.risk_flags.structural_blockers);
  return;
}

// Size gate
const maxSize = policy.exposure_policy.max_size_fraction;
let size = Math.min(intendedSize, maxSize);

// Action gate
if (!policy.exposure_policy.allowed_actions.includes("DCA")) {
  return;
}

await executeTrade({ asset: "BTC", size });
```

## 5. Handle errors

When the API is unavailable, fall back to the most conservative policy:

```python
FALLBACK_POLICY = {
    "max_size_fraction": 0.10,
    "leverage_allowed": False,
    "allowed_actions": ["WAIT"],
    "blocked_actions": ["ALL_IN", "LEVERAGE", "AGGRESSIVE_LONG"]
}

try:
    policy = get_risk_policy("BTC")
except Exception:
    policy = {"exposure_policy": FALLBACK_POLICY}
```

Each API response includes `ttl_seconds: 60`. Cache the last known policy and re-request after TTL expires.

---

## Full API reference

See [docs/api-v1.md](https://github.com/likidodefi/riskstate-docs/blob/main/docs/api-v1.md) for the complete field reference, enum values, error codes, and interpretation guide.
