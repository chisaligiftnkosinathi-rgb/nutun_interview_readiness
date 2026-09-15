# Lesson 2: Checkpoint Questions

### A — Scope
What is **lexical scope**, and why can a function access variables from an outer scope? Use a payment or AI example.

### B — Closure
Explain why this keeps working:
```javascript
function createCounter() {
    let count = 0;
    return () => ++count;
}
const counter = createCounter();
counter(); // 1
counter(); // 2
```
Where is `count` stored after `createCounter()` has returned?

### C — Event Loop
Predict the output and explain why:
```javascript
console.log("1");
setTimeout(() => { console.log("2"); }, 0);
Promise.resolve().then(() => { console.log("3"); });
console.log("4");
```

### D — async/await
Does this freeze the entire JavaScript thread?
```javascript
async function loadCustomer() {
    const response = await fetch("/api/customer");
    return response.json();
}
```
Explain precisely what `await` does.

### E — Nutun
An agent starts an AI customer-summary request, then immediately navigates to another customer. What could go wrong if the frontend doesn't properly handle cancellation or stale asynchronous results?

---

# Lesson 2: Challenge Round Questions

### Challenge 1 — Async Completion Order
An agent performs three actions in quick succession:
- Action A at 0ms
- Action B at 100ms
- Action C at 200ms

Why might the UI end up displaying the result of Action A last, even though it was initiated first? Is this a defect in React, a bug in HTTP, or a fundamental property of asynchronous operations?

### Challenge 2 — AbortController Guarantees
What does `AbortController.abort()` actually accomplish? What can you safely claim it does, and what transport-level guarantee should you avoid claiming?

### Challenge 3 — Cancellation vs Stale-Result Protection
Why is request cancellation alone not sufficient to guarantee that stale results will never corrupt UI state? What is the difference between "canceling an operation" and "verifying validity"?

### Challenge 4 — Financial State & Identity (Nutun Production Scenario)
Customer A has an outstanding debt balance of R18,450.
Customer B has an outstanding debt balance of R7,200.

An agent switches rapidly from Customer A to Customer B and clicks "Generate Payment Arrangement".
How do you architect the frontend and its communication with the backend to ensure the agent never issues a legal payment arrangement for Customer B using Customer A's financial figures?

---

# Lesson 2: Final Challenge — Stale Closures in React

### Question 1 — Lexical Environment Retention
Why can a callback created during one React render continue to "remember" the values from that render?

### Question 2 — Code Output Prediction
In this code:
```tsx
function Customer({ customerId }) {
    useEffect(() => {
        setTimeout(() => {
            console.log(customerId);
        }, 2000);
    }, []);

    return <div>{customerId}</div>;
}
```
If the first render has `customerId = "A"` and then the component renders again with `customerId = "B"`, what will the timeout print after 2 seconds? Explain why.

### Question 3 — Production Impact
Why is this concept important when building:
- AI chat interfaces
- Customer search
- Autocomplete
- Payment workflows
- Streaming responses

### Question 4 — Interview Question (60–90s Pitch)
An interviewer says:
> **"Explain stale closures in React to me as if I'm a backend engineer."**
Provide a crisp 60–90 second explanation.

