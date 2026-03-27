# OpenClaw + DeFiLlama + RiskState + Uniswap

DeFi research and allocation agent. Free data, policy-gated sizing, and intent-based execution via UniswapX.

## Stack

| Layer | Component | Role | Link | Cost |
|-------|-----------|------|------|------|
| Framework | OpenClaw | Agent orchestration via ClawHub skills | [openclaw.ai](https://openclaw.ai) | Free |
| Intelligence | DeFiLlama | TVL, yields, protocol data, fees, stablecoin flows | [defillama.com](https://defillama.com) | Free |
| Risk | RiskState | Position limits, leverage gating, policy enforcement | [riskstate.ai](https://riskstate.ai) | Free (beta) |
| Execution | Uniswap | Token swaps via UniswapX intents (gasless, MEV-protected) | [app.uniswap.org](https://app.uniswap.org) | Free |

**Custody:** Self-custody wallet

## Data flow

1. **DeFiLlama** provides TVL trends, yield opportunities, protocol fee data, and stablecoin flows
2. **OpenClaw agent** identifies allocation opportunities (yield rotation, protocol rebalancing)
3. **RiskState** returns `max_size_fraction` and `allowed_actions` for the target asset
4. Agent caps deployment size and checks for `structural_blockers`
5. **Uniswap** executes the swap via UniswapX intent-based order (gasless for the agent)

## RiskState integration

```python
policy = get_risk_policy("ETH")

if policy["risk_flags"]["structural_blockers"]:
    return  # Do not deploy

max_size = policy["exposure_policy"]["max_size_fraction"]
deploy_size = min(intended_size, max_size)

if "DCA" in policy["exposure_policy"]["allowed_actions"]:
    submit_uniswap_order(asset="WETH", size=deploy_size)
```

See [shared integration guide](../../shared/riskstate-integration.md) for the full pattern.

## Environment variables

See [env.example](env.example)

## Links

- [OpenClaw documentation](https://openclaw.ai)
- [DeFiLlama API](https://defillama.com/docs/api)
- [Uniswap developer docs](https://docs.uniswap.org)
- [RiskState API docs](https://github.com/likidodefi/riskstate-docs)
- [Visual overview](https://riskstate.ai/agent-setups)
