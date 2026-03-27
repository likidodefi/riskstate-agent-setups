# Hermes + CoinGlass + RiskState + Safe

Treasury risk-controlled execution agent. DAO or team multisig with derivatives context and policy guardrails.

## Stack

| Layer | Component | Role | Link | Cost |
|-------|-----------|------|------|------|
| Framework | Hermes | Agent orchestration and skill execution | [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com) | Free |
| Intelligence | CoinGlass | Funding rates, open interest, liquidations, ETF flows | [coinglass.com](https://www.coinglass.com) | ~$29-99/mo |
| Risk | RiskState | Position limits, leverage gating, policy enforcement | [riskstate.ai](https://riskstate.ai) | Free (beta) |
| Execution | Safe | Multisig Smart Account with programmable modules | [safe.global](https://safe.global) | Free |

**Custody:** Safe Smart Account

## Data flow

1. **CoinGlass** provides market context: funding rates, OI, liquidation maps, ETF flows
2. **Hermes agent** processes derivatives context and generates a treasury rebalancing proposal
3. **RiskState** returns `max_size_fraction`, `allowed_actions`, and `structural_blockers`
4. Agent gates the proposal: caps size, checks blockers, verifies action is permitted
5. **Safe** queues the transaction for multisig approval and execution

## RiskState integration

```python
policy = get_risk_policy("BTC")

if policy["risk_flags"]["structural_blockers"]:
    return  # Do not propose trade

max_size = policy["exposure_policy"]["max_size_fraction"]
proposal_size = min(intended_size, max_size)

if "DCA" in policy["exposure_policy"]["allowed_actions"]:
    queue_safe_transaction(asset="cbBTC", size=proposal_size)
```

See [shared integration guide](../../shared/riskstate-integration.md) for the full pattern.

## Environment variables

See [env.example](env.example)

## Links

- [Hermes documentation](https://hermes-agent.nousresearch.com)
- [CoinGlass API](https://www.coinglass.com)
- [Safe developer docs](https://docs.safe.global)
- [RiskState API docs](https://github.com/likidodefi/riskstate-docs)
- [Visual overview](https://riskstate.ai/agent-setups)
