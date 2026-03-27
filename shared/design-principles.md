# Design Principles

Five principles that govern every setup in this repository.

---

### 1. Open components

Every layer is a public API or open-source SDK. No partnerships required to assemble any setup. If a component disappears tomorrow, you can swap it for an equivalent.

### 2. Risk layer independent from intelligence

RiskState doesn't need to know your data source. It ingests 30+ signals internally and returns a policy decision. Your intelligence layer feeds your agent's logic, not RiskState's.

### 3. Execution rails are replaceable

Swap CoW Swap for Uniswap. Safe for an EOA. AgentKit Actions for a custom signing service. The execution layer is downstream of the policy decision and can be changed without affecting governance.

### 4. Minimal vendor lock-in

No component in any setup requires exclusive access, a partnership agreement, or a token gate. Everything listed is publicly available today.

### 5. Conservative fallback on risk errors

When RiskState is unavailable, the agent defaults to the most conservative policy (smallest position, no leverage, no new trades). This is the only safe default for autonomous agents managing real capital.
