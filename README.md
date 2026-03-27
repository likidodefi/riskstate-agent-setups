# RiskState Agent Setups

Open reference architectures for autonomous crypto agents with external risk governance.

Examples using Hermes, OpenClaw, ElizaOS, AgentKit, CoinGlass, DeFiLlama, Nansen, Alchemy, CoW Swap, Uniswap, Safe, and RiskState.

---

## How it works

```
Framework  →  Intelligence  →  RiskState  →  Execution
(decides)     (observes)       (permits)     (acts)
```

Frameworks decide. RiskState sets the limits. Execution follows policy.

---

## Setups

| Setup | Use Case | Best For | Framework | Intelligence | Risk | Execution | Status |
|-------|----------|----------|-----------|--------------|------|-----------|--------|
| [Hermes + CoinGlass + RiskState + CoW](setups/hermes-coinglass-cow/) | BTC/ETH tactical trading | Active traders | Hermes | CoinGlass | RiskState | CoW Swap | `Guide` |
| [OpenClaw + DeFiLlama + RiskState + Uniswap](setups/openclaw-defillama-uniswap/) | DeFi opportunity execution | Solo operators | OpenClaw | DeFiLlama | RiskState | Uniswap | `Guide` |
| [ElizaOS + Nansen + RiskState + CoW](setups/elizaos-nansen-cow/) | Smart-money aware execution | Advanced onchain | ElizaOS | Nansen | RiskState | CoW Swap | `Guide` |
| [Hermes + CoinGlass + RiskState + Safe](setups/hermes-coinglass-safe/) | Treasury risk-controlled execution | Treasuries / DAOs | Hermes | CoinGlass | RiskState | Safe | `Guide` |
| [AgentKit + Alchemy + RiskState](setups/agentkit-alchemy-riskstate/) | Builder-friendly onchain agent | Developers | AgentKit | Alchemy | RiskState | AgentKit Actions | `Guide` |

> **LLM:** Any compatible model supported by the selected framework. All frameworks listed here are model-agnostic.

---

## Quick start

Add RiskState to any agent in 3 steps:

```bash
curl -X POST https://riskstate.netlify.app/v1/risk-state \
  -H "Authorization: Bearer $RISKSTATE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"asset": "BTC"}'
```

Read `max_size_fraction`, check `structural_blockers`, gate your execution.

See [How to wire RiskState into any agent](shared/riskstate-integration.md) for the full guide.

---

## Links

| Resource | URL |
|----------|-----|
| Landing page | [riskstate.ai/agent-setups](https://riskstate.ai/agent-setups) |
| API Documentation | [riskstate-docs](https://github.com/likidodefi/riskstate-docs) |
| SKILL.md | [Agent discovery file](https://github.com/likidodefi/riskstate-docs/blob/main/SKILL.md) |
| MCP Server | [@riskstate/mcp-server](https://www.npmjs.com/package/@riskstate/mcp-server) |
| Get API key | [riskstate.ai/#get-started](https://riskstate.ai/#get-started) |

---

## Build your own

These setups are starting points. Swap any layer. RiskState works with any framework, any data source, and any execution rail that accepts a position limit.

- [How to wire RiskState into any agent](shared/riskstate-integration.md)
- [Design principles](shared/design-principles.md)

---

## License

MIT
