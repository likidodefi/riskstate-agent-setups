# ElizaOS + Nansen + RiskState + CoW Swap

Smart-money aware trading agent. Tracks wallet flows, gates position size with policy, and executes with MEV protection.

## Stack

| Layer | Component | Role | Link | Cost |
|-------|-----------|------|------|------|
| Framework | ElizaOS | Crypto-native agent framework (17K+ stars, 25+ DeFi plugins) | [elizaos.ai](https://elizaos.ai) | Free |
| Intelligence | Nansen | Smart-money flows, wallet tracking, sector rotations | [nansen.ai](https://www.nansen.ai) | ~$49-69/mo |
| Risk | RiskState | Position limits, leverage gating, policy enforcement | [riskstate.ai](https://riskstate.ai) | Free (beta) |
| Execution | CoW Swap | MEV-protected token swaps via intent-based trading | [swap.cow.fi](https://swap.cow.fi) | Free |

**Custody:** Safe / wallet

## Data flow

1. **Nansen** identifies smart-money wallet movements, token accumulation, and sector flows
2. **ElizaOS agent** processes wallet signals and generates a trade intent based on smart-money patterns
3. **RiskState** returns `max_size_fraction`, `allowed_actions`, and risk flags
4. Agent gates the trade: caps size, checks blockers, verifies action is permitted
5. **CoW Swap** executes the trade via EIP-712 signed batch auction order

## RiskState integration

```python
policy = get_risk_policy("ETH")

if policy["risk_flags"]["structural_blockers"]:
    return  # Do not trade

max_size = policy["exposure_policy"]["max_size_fraction"]
trade_size = min(intended_size, max_size)

if "LONG_SHORT_CONFIRMED" in policy["exposure_policy"]["allowed_actions"]:
    submit_cow_order(asset="WETH", size=trade_size)
```

See [shared integration guide](../../shared/riskstate-integration.md) for the full pattern.

## Environment variables

See [env.example](env.example)

## Links

- [ElizaOS documentation](https://elizaos.ai)
- [ElizaOS GitHub](https://github.com/elizaOS/eliza)
- [Nansen API](https://www.nansen.ai)
- [CoW Protocol docs](https://docs.cow.fi)
- [RiskState API docs](https://github.com/likidodefi/riskstate-docs)
- [Visual overview](https://riskstate.ai/agent-setups)
