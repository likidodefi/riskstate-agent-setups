# Coinbase AgentKit + Alchemy + RiskState + Agentic Wallet

Builder-friendly onchain agent. Fastest path to a risk-governed autonomous agent with managed wallet infrastructure.

## Stack

| Layer | Component | Role | Link | Cost |
|-------|-----------|------|------|------|
| Framework | AgentKit | Wallet creation, tx signing, onchain actions | [docs.cdp.coinbase.com/agent-kit](https://docs.cdp.coinbase.com/agent-kit) | Free |
| Intelligence | Alchemy | Onchain data, token balances, transaction history | [alchemy.com](https://www.alchemy.com) | Free tier available |
| Risk | RiskState | Position limits, leverage gating, policy enforcement | [riskstate.ai](https://riskstate.ai) | Free (beta) |
| Execution | AgentKit Actions | Built-in swap, transfer, and deploy actions | [docs.cdp.coinbase.com/agent-kit](https://docs.cdp.coinbase.com/agent-kit) | Free |

**Custody:** Agentic Wallet

## Data flow

1. **Alchemy** provides onchain data: token balances, transaction history, contract state
2. **AgentKit agent** processes data and decides on an action (swap, transfer, deploy)
3. **RiskState** returns `max_size_fraction`, `allowed_actions`, and `structural_blockers`
4. Agent gates the action: caps size, checks blockers, verifies action type is permitted
5. **AgentKit Actions** executes the transaction via the Agentic Wallet

## RiskState integration

```javascript
const policy = await fetch("https://riskstate.netlify.app/v1/risk-state", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${RISKSTATE_API_KEY}`,
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ asset: "ETH" })
}).then(r => r.json());

if (policy.risk_flags.structural_blockers.length > 0) {
  return; // Do not execute
}

const maxSize = policy.exposure_policy.max_size_fraction;
const size = Math.min(intendedSize, maxSize);

if (policy.exposure_policy.allowed_actions.includes("DCA")) {
  await agentKit.swap({ from: "USDC", to: "WETH", amount: size });
}
```

See [shared integration guide](../../shared/riskstate-integration.md) for the full pattern.

## Environment variables

See [env.example](env.example)

## Links

- [AgentKit documentation](https://docs.cdp.coinbase.com/agent-kit)
- [AgentKit GitHub](https://github.com/coinbase/agentkit)
- [Alchemy API](https://www.alchemy.com)
- [RiskState API docs](https://github.com/likidodefi/riskstate-docs)
- [Visual overview](https://riskstate.ai/agent-setups)
