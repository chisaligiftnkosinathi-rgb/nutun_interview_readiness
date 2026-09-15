# Lesson 2: JavaScript Before React

## Governing Principle
React is not a separate programming language—it is a JavaScript library operating strictly under the rules of the ECMAScript runtime. To engineer resilient, high-volume contact centre dashboards that never freeze or display stale financial data, we must master the underlying runtime machinery: lexical scope, closures, the Event Loop, microtasks, macrotasks, and asynchronous cancellation with `AbortController`.

---

## 1. Real-World Mental Models: The Operational Scaffold

Before analyzing execution contexts and the heap, we anchor every JavaScript runtime primitive in an operational contact centre or real-life analogy:

| Technical Concept | Real-World Operational Model | What It Prevents |
| :--- | :--- | :--- |
| **Closures** | **The Agent's Commuter Backpack**: An agent packs their notes, ID badge, and customer account reference into a backpack before leaving the office. Wherever they go later in the day, they still have access to what was in their backpack when they packed it. | Functions losing access to parent scope variables after execution contexts pop. |
| **Event Loop & Queues** | **The Restaurant Chef & Order Slips**: A restaurant has one chef (the single Call Stack). The chef works through orders sequentially. Urgent kitchen cleanups/plating garnishes (Microtasks) must be finished immediately before the chef pulls the next customer order ticket (Macrotask). | Main-thread freezing; UI locking up during long asynchronous sequences. |
| **`AbortController`** | **The Cancelled Taxi Order**: You request a taxi to take you to a debtor's address. Before the driver arrives, the customer phones in and settles their debt. You send a cancellation signal to the driver. The driver stops driving; you don't pay the full fare or wait for them. | Wasteful network bandwidth, race conditions, and unmounted state updates. |
| **Request vs Completion Order** | **Three Postal Couriers**: You dispatch three couriers with letters to three different cities at 09:00, 09:05, and 09:10. Courier 3 arrives first because they had a shorter highway route. Sequential dispatch never guarantees sequential delivery. | Race conditions where slow search queries overwrite newer keystroke results. |

---

## 2. Closures & Lexical Scope

### 2.1 The Intuitive Reality
In JavaScript, functions carry their birthplace with them. 

```javascript
function createAccountAuditor(accountId) {
    // Outer scope: "The Office"
    const timestamp = Date.now();

    return function auditTransaction(amount) {
        // Inner scope: "The Backpack"
        // This function remembers accountId and timestamp forever,
        // even though createAccountAuditor has finished executing!
        console.log(`[${timestamp}] Audited R${amount} on account ${accountId}`);
    };
}

const audit = createAccountAuditor("ACC-99214");
audit(450); // Remembers ACC-99214 and timestamp
```

### 2.2 The React Trap: Stale Closures
In React, component functions run on every render, creating a brand-new lexical scope. If an asynchronous callback or `useEffect` captures variables from an earlier render without proper dependency declarations:

```tsx
function DebtTimer() {
    const [seconds, setSeconds] = useState(0);

    useEffect(() => {
        const interval = setInterval(() => {
            // STALE CLOSURE TRAP:
            // This closure captured 'seconds' as 0 at mount time!
            // Every second, 0 + 1 evaluates to 1. The clock never advances past 1!
            setSeconds(seconds + 1);
        }, 1000);

        return () => clearInterval(interval);
    }, []); // Empty dependency array captures initial render snapshot forever

    return <div>Call Elapsed: {seconds}s</div>;
}
```

**The Fix (Functional Updater)**:
Pass a function updater `setSeconds(prev => prev + 1)`. React supplies the latest committed state directly from the Fiber node, bypassing the stale closure completely.

---

## 3. The Event Loop, Microtasks & Macrotasks

### 3.1 The Single-Threaded Runtime
JavaScript has a single Call Stack. It can only execute one line of synchronous code at any instant.

```text
       ┌────────────────────────┐
       │   CALL STACK (Chef)    │
       └───────────┬────────────┘
                   │ Empties
                   ▼
       ┌────────────────────────┐
       │ MICROTASKS (Promises)  │ ◄── Completely drained until empty!
       └───────────┬────────────┘
                   │
                   ▼
       ┌────────────────────────┐
       │ RENDER / PAINT PASS    │ ◄── Browser recalculates styles & paints pixels
       └───────────┬────────────┘
                   │
                   ▼
       ┌────────────────────────┐
       │ MACROTASKS (setTimeout)│ ◄── Executes ONE macrotask, then loops back
       └────────────────────────┘
```

### 3.2 Microtask Starvation
Because the Microtask Queue is drained completely before the browser is allowed to paint or process user input:
* If microtasks continuously schedule more microtasks (e.g. recursive `Promise.resolve().then(...)` or `queueMicrotask`), the Call Stack never stays empty long enough to yield.
* The browser locks up, clicks don't register, and the tab crashes.

---

## 4. Asynchronous Race Conditions & `AbortController`

### 4.1 The Contact Centre Race Condition
An agent types "Smith" into a customer search box:
1. Keystroke 'S' fires Request 1 (takes 800ms due to database cold cache).
2. Keystroke 'm' fires Request 2 (takes 150ms due to fast index hit).
3. Request 2 completes first: UI displays "Smith, Mary".
4. 650ms later, Request 1 finally completes: UI **overwrites** with "S, John".
5. The agent is now looking at the wrong customer's debt file!

### 4.2 The Solution: `AbortController` & Signal Cleanup
```tsx
function CustomerSearch({ query }: { query: string }) {
    const [results, setResults] = useState<Customer[]>([]);

    useEffect(() => {
        // 1. Create cancellation signal
        const controller = new AbortController();

        async function fetchCustomers() {
            try {
                const res = await fetch(`/api/customers?q=${encodeURIComponent(query)}`, {
                    signal: controller.signal
                });
                const data = await res.json();
                setResults(data);
            } catch (err: any) {
                if (err.name === "AbortError") {
                    // Benign cancellation: intentionally aborted by user action or new keystroke
                    return;
                }
                console.error("Search failed:", err);
            }
        }

        fetchCustomers();

        // 2. Cleanup function runs whenever query changes or component unmounts
        return () => {
            controller.abort();
        };
    }, [query]);

    return <ResultsList items={results} />;
}
```

### 4.3 Cancellation vs. Validity
* **Cancellation (`AbortController`)**: Informs the browser network layer to abandon reading the response stream and release socket buffers.
* **Validity (`Boolean Flag` / `Request ID`)**: Guarantees that even if an HTTP response has already arrived in flight, the application will not commit stale data into state.

---

## 5. Interview Verification Questions
1. *"Explain what a closure is without using technical jargon, and then explain how a stale closure occurs inside a React `useEffect`."*
2. *"Why does `Promise.resolve().then(...)` execute before `setTimeout(..., 0)`? What are the implications for main-thread responsiveness?"*
3. *"If an agent types fast in a search input, how do you prevent an earlier slow HTTP request from overwriting a newer fast response? What role does `AbortController` play?"*

