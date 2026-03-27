# Hermes + CoinGlass + RiskState + CoW Swap

BTC/ETH tactical trading agent with derivatives context, policy-based risk gating, and MEV-protected execution.

## Stack

| Layer | Component | Role | Link | Cost |
|-------|-----------|------|------|------|
| Framework | Hermes | Agent orchestration and skill execution | [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com) | Free |
| Intelligence | CoinGlass | Funding rates, open interest, liquidations, ETF flows | [coinglass.com](https://www.coinglass.com) | ~$29-99/mo |
| Risk | RiskState | Position limits, leverage gating, policy enforcement | [riskstate.ai](https://riskstate.ai) | Free (beta) |
| Execution | CoW Swap | MEV-protected token swaps via intent-based trading | [swap.cow.fi](https://swap.cow.fi) | Free |

**Custody:** Safe / EOA

## Data flow

1. **CoinGlass** provides funding rate, OI, liquidation data, and ETF flow context
2. **Hermes agent** processes signals and generates a trade intent (direction, size, asset)
3. **RiskState** receives `POST /v1/risk-state` and returns `max_size_fraction`, `allowed_actions`, and `structural_blockers`
4. Agent gates the trade: caps size to `max_size_fraction`, checks `structural_blockers` is empty
5. **CoW Swap** executes the trade via EIP-712 signed order (MEV-protected batch auction)

## RiskState integration

```python
policy = get_risk_policy("BTC")

if policy["risk_flags"]["structural_blockers"]:
    return  # Do not trade

max_size = policy["exposure_policy"]["max_size_fraction"]
trade_size = min(intended_size, max_size)

if "LONG_SHORT_CONFIRMED" in policy["exposure_policy"]["allowed_actions"]:
    submit_cow_order(asset="cbBTC", size=trade_size)
```

See [shared integration guide](../../shared/riskstate-integration.md) for the full pattern.

## Environment variables

See [env.example](env.example)

## Links

- [Hermes documentation](https://hermes-agent.nousresearch.com)
- [CoinGlass API](https://www.coinglass.com)
- [CoW Protocol docs](https://docs.cow.fi)
- [RiskState API docs](https://github.com/likidodefi/riskstate-docs)
- [Visual overview](https://riskstate.ai/agent-setups)
