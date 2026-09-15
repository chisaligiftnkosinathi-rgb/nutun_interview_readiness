# React Lesson 1: Checkpoint Questions

### A — Why React?
Why did declarative UI become useful as web applications became more interactive?

### B — Component
What conceptually happens when React renders:
```tsx
function Customer({ customerId }) {
    return <div>{customerId}</div>;
}
```

### C — Render vs Commit
Explain the difference between the render phase and the commit phase.

### D — Fiber
What problem does Fiber's internal representation help React solve?

### E — Reconciliation
Given:
```text
Old UI:
Customer → Balance R1,000

New UI:
Customer → Balance R1,500
```
What is React trying to determine during reconciliation?

### F — JavaScript Connection
Why does stale-closure knowledge matter when learning React?

### G — Interview Challenge
Answer this as though asked by the Nutun interviewer:
> **“Gift, don't just tell me that React is a library for building user interfaces. What problem was React actually trying to solve, and what does React do mechanically when state changes?”**
