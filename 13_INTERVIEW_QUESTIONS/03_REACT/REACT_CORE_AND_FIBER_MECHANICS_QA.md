# React Core Architecture & Fiber Mechanics: Interview Questions & Verified Answers

> **ROLE CONTEXT**: Nutun Front-End Engineer (OfferZen Facilitated)  
> **OPERATIONAL SCALE**: 18.5M monthly customer interactions, up to 10,000 concurrent agents, sub-second CTI screen pops, high-frequency WebRTC audio threads, and low-latency financial state transitions.

---

## Q3.1: Virtual DOM, Reconciliation & Fiber Mechanics

### Scenario — Nutun Real-Time Agent Control Surface
In the Nutun contact centre dashboard, an agent is negotiating a payment arrangement while:
* The WebRTC telephony component pulses an audio waveform timer every 100ms.
* An AI co-pilot streams legal advice token-by-token over Server-Sent Events (SSE).
* The agent types into the "Settlement Discount %" numeric input.

A junior developer on the team observes the high frequency of updates and remarks:
> *"Every time an AI token arrives or the timer ticks, React creates a whole new Virtual DOM copy, compares the entire page in memory, and writes it to the DOM. That's why React is faster than normal JavaScript."*

The interviewer asks:
> **“Is the Virtual DOM just an in-memory copy of the real DOM? Does React compare the entire Virtual DOM on every update? Is Fiber the Virtual DOM? Why is React's declarative model preferred over direct DOM mutations, and what mechanically happens from the moment `setState` is called to pixels changing on screen?”**

---

### Verified Candidate Answer

#### 1. Decoupling the Architectural Layers
Candidates frequently collapse several distinct browser and React concepts into a single vague bucket. A senior engineer must explicitly demarcate these six layers:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│                    THE SIX LAYERS OF REACT RENDERING                       │
├──────────────────────────────┬─────────────────────────────────────────────┤
│ 1. JSX COMPILATION           │ Transpiled by Babel/Vite into React element │
│                              │ creation calls (`jsx("div", { ... })`).     │
├──────────────────────────────┼─────────────────────────────────────────────┤
│ 2. REACT ELEMENTS            │ Lightweight, immutable plain JavaScript     │
│                              │ objects describing desired UI tree.         │
├──────────────────────────────┼─────────────────────────────────────────────┤
│ 3. FIBER TREE (Current/Work) │ Mutable in-memory graph of component        │
│                              │ instances, state, hooks, and work units.    │
├──────────────────────────────┼─────────────────────────────────────────────┤
│ 4. RECONCILIATION (Render)   │ Heuristic O(N) diffing of new elements      │
│                              │ against current Fiber nodes (Interruptible).│
├──────────────────────────────┼─────────────────────────────────────────────┤
│ 5. COMMIT PHASE              │ Applying calculated DOM mutations to the    │
│                              │ real browser C++ DOM (Synchronous).         │
├──────────────────────────────┼─────────────────────────────────────────────┤
│ 6. BROWSER FRAME PIPELINE    │ Style Recalculation ──► Layout ──► Paint    │
│                              │ ──► Compositing (Executed by browser C++).  │
└──────────────────────────────┴─────────────────────────────────────────────┘
```

The governing architectural rule:
> **"React's reconciliation work is NOT the same thing as browser DOM work. Reconciliation calculates the delta in JavaScript memory; the browser rendering pipeline rasterizes pixels."**

---

#### 2. Is the "Virtual DOM" Just a Copy of the Real DOM?
**No. This is a common misconception.**

* The browser DOM is a heavy, stateful, internal C++ tree structure exposed via browser APIs (`HTMLDivElement`). A single HTML element instance contains over 250 properties, event listeners, layout geometry, and style maps.
* The so-called "Virtual DOM" is **not a copy of the DOM**. It is a tree of **ephemeral, lightweight, plain JavaScript objects** (`ReactElement`):
  ```typescript
  // A React Element is literally just this plain object:
  {
    $$typeof: Symbol(react.element),
    type: 'button',
    key: 'btn-submit',
    props: { className: 'btn-primary', children: 'Confirm Settlement' },
    ref: null
  }
  ```
* Creating 1,000 React element objects in memory takes microseconds; allocating 1,000 real DOM nodes in C++ takes orders of magnitude more time and memory.

---

#### 3. Does React Compare the "Entire" Virtual DOM on Every Update?
**No. React does not diff the entire application tree from the root on every update.**

1. **Subtree Scoping**:
   - Reconciliation begins **at the component where `setState` was invoked**.
   - If `setAmount()` is called inside `<PaymentSlider />`, React reconciles `<PaymentSlider />` and its children. Sibling branches (such as `<CustomerProfileHeader />` or `<TelephonySoftphone />`) are completely bypassed unless their props or contexts change.
2. **Reconciliation Heuristics ($O(N)$ vs. $O(N^3)$)**:
   - Mathematically finding the minimum number of operations to transform one arbitrary tree into another is an $O(N^3)$ problem (e.g., 1,000 elements would require 1 billion comparisons).
   - React achieves $O(N)$ efficiency using two practical heuristics:
     * **Different Types Produce Different Subtrees**: If an element changes from `<div>` to `<section>`, or from `<CustomerCard>` to `<CompanyCard>`, React tears down the old tree and builds the new one from scratch (destroying all state).
     * **Key Heuristic**: In lists, React uses the `key` prop to match child elements across renders, allowing it to preserve identity and reorder nodes efficiently.

---

#### 4. Is "Fiber" the Virtual DOM?
**No. Fiber is React's internal execution engine and component state container.**

* Prior to React 16 (the "Stack Reconciler"), React walked the component tree recursively. Once rendering started, it could not be stopped until the entire tree was traversed, freezing the main thread on large dashboards.
* **Fiber** (introduced in React 16) redesigned the reconciler as a **cooperative multitasking work loop**:
  * A **Fiber node** is a mutable JavaScript object representing a unit of work.
  * Unlike React elements (which are recreated on every render), Fibers are persistent and retain component state, hook lists, and DOM node references.
  * Fibers are linked using a singly-linked list structure with three pointers:
    ```text
    parent Fiber ◄── [return] ── Fiber Node ── [child] ──► first child Fiber
                                    │
                                [sibling]
                                    ▼
                            next sibling Fiber
    ```
  * This pointer structure allows React to pause rendering, yield control back to the browser event loop to handle user input or WebRTC audio, and resume where it left off.

##### The Double-Buffering Pattern:
React maintains **two Fiber trees** in memory simultaneously:
1. **The `current` tree**: Represents the nodes currently rendered on screen.
2. **The `workInProgress` tree**: The alternate tree being constructed in memory during the render phase.
Once all render calculations are complete, React simply flips a single pointer: `root.current = workInProgress`. This guarantees atomic, flicker-free UI updates.

---

#### 5. Is React Faster Than Direct DOM Manipulation?
**No. Direct, hand-optimized imperative DOM manipulation will always be faster than React.**

* If an engineer writes custom C++ or optimized JavaScript that directly mutates `node.textContent = 'R500'`, that direct mutation has zero abstraction overhead.
* React must:
  1. Allocate React elements.
  2. Traverse Fiber nodes.
  3. Run reconciliation diffing.
  4. Build an effect list.
  5. Finally execute the exact same `node.textContent = 'R500'` mutation!

##### Why We Use React:
React was not created to be faster than hand-written DOM code; it was created to provide **maintainable, declarative state coordination at enterprise scale**. In high-density applications with dozens of concurrent asynchronous events, manual DOM manipulation devolves into race conditions, memory leaks, and layout thrashing. React makes applications **fast enough by default** while guaranteeing predictable state synchronization.

---

#### 6. The End-to-End Lifecycle: From `setState` to Pixels on Screen

```text
[ 1. TRIGGER ]       Agent types: `setDiscount(15)` called.
                     React marks the component Fiber dirty and schedules work.
        │
        ▼
[ 2. SCHEDULER ]     React Scheduler assigns priority (UserBlocking / Normal / Idle).
                     In Concurrent React, low-priority work can yield to user input.
        │
        ▼
[ 3. RENDER PHASE ]  React invokes component function: `PaymentForm(props)`.
(Interruptible)      Generates new React Element tree.
                     Reconciler diffs new elements against `current` Fibers.
                     Builds `workInProgress` Fiber tree.
                     Tags nodes with Effect Flags: `Placement`, `Update`, `Deletion`.
        │
        ▼
[ 4. COMMIT PHASE ]  SYNCHRONOUS & UNINTERRUPTIBLE:
(Uninterruptible)    • Mutation Pass: Host mutations applied to real browser DOM.
                     • Layout Pass: `useLayoutEffect` runs; DOM measurements taken.
                     • Pointer Flip: `root.current = workInProgress`.
        │
        ▼
[ 5. BROWSER PAINT ] React yields control to the browser event loop.
                     Browser runs: Style Recalc ──► Layout ──► Paint ──► Composite.
                     Real physical pixels change on the agent's monitor.
        │
        ▼
[ 6. PASSIVE EFFECT] `useEffect` callbacks run asynchronously after paint.
```

---

### The Hostile Curveball Defense

#### Curveball 1:
> **Interviewer**: *"If Fiber rendering is interruptible, what stops an agent from seeing half-rendered UI when a high-priority keystroke interrupts an AI streaming update?"*

#### Candidate Defense:
*"Because **only the Render Phase is interruptible; the Commit Phase is completely synchronous and atomic.**

When a low-priority render (like an AI streaming paragraph) is interrupted by an agent typing into a payment input:
1. React pauses or discards the in-progress `workInProgress` Fiber tree.
2. React runs the high-priority typing update and commits it to the DOM.
3. React then restarts or resumes the background render on a fresh `workInProgress` tree.

Because the browser DOM is **never touched during the Render Phase**, the agent never sees half-rendered UI or state tears. Host DOM mutations only occur during the Commit Phase, which executes synchronously from start to finish."*

---

#### Curveball 2:
> **Interviewer**: *"Why does React 18 render twice in Strict Mode in development? Does that mean our production app does double the work?"*

#### Candidate Defense:
*"No, double rendering is **strictly a development-only verification tool**; it is completely compiled out in production builds.

React 18 intentionally double-invokes component functions, reducers, and initializer functions in development to **catch impure side effects in the Render Phase**. 

Because Fiber's Concurrent Mode can pause, discard, and re-run component rendering multiple times before committing, a component render **must be a pure function with zero observable side effects** ($\text{UI} = f(\text{state})$). If a developer mutates global variables, attaches event listeners, or makes HTTP calls directly inside the component body instead of inside `useEffect`, the double render immediately exposes the bug by creating duplicate subscriptions or memory leaks."*

---

## Q3.2: Render Phase vs. Commit Phase & Effect Timing

### Scenario — Nutun Payment Arrangement Settlement Screen
In the Cheetah Collections CRM, an agent is negotiating a debt settlement. The agent clicks:
> **"Apply 20% Settlement Waiver"**

React must calculate the revised instalment schedule, update state, render the confirmation preview, commit DOM changes, and synchronize external telemetry.

The interviewer asks:
> **“Why can the Render phase execute multiple times, be paused, or be completely abandoned without committing to the DOM? What belongs in the Render phase versus the Commit phase? What is the exact execution order between DOM mutations, `useLayoutEffect`, browser paint, and `useEffect`? Why is placing an API call in `useLayoutEffect` an anti-pattern?”**

---

### Verified Candidate Answer

#### 1. The Core Conceptual Separation: Planning vs. Execution

To master React architecture, one must view React as an engine operating in two distinct phases:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│                    RENDER PHASE (Planning / Calculation)                   │
│                                                                            │
│  • PURE, DETERMINISTIC, NO SIDE EFFECTS                                    │
│  • Asks: "Given current props & state, what SHOULD the UI look like?"      │
│  • Evaluates component functions & produces React elements.                │
│  • Reconciles Fiber nodes.                                                 │
│  • CAN BE PAUSED, RE-RUN, OR ABANDONED AT ANY TIME BY CONCURRENT REACT!   │
└─────────────────────────────────────┬──────────────────────────────────────┘
                                      │ Work completed & coherent
                                      ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                    COMMIT PHASE (Execution / Host Mutation)                │
│                                                                            │
│  • SYNCHRONOUS, UNINTERRUPTIBLE, MUTATES THE HOST ENVIRONMENT              │
│  • Asks: "Apply the completed calculation to the real DOM."                │
│  • Executes DOM mutations (insert, update, delete).                        │
│  • Synchronously runs layout effects (`useLayoutEffect`).                  │
│  • Flips Fiber root pointer (`root.current = workInProgress`).             │
│  • Yields to browser for Paint, then schedules passive `useEffect`.        │
└────────────────────────────────────────────────────────────────────────────┘
```

---

#### 2. Why Can the Render Phase Run Multiple Times (And Why Side Effects Break)?

In Concurrent React, rendering is **cooperative and non-blocking**. If a low-priority render (e.g. background data recalculation or AI streaming text) is in progress, and high-priority input arrives (e.g. an agent typing a settlement amount or clicking a telephony button):
1. React pauses the background render.
2. If the state changes while paused, the previous in-progress render is **completely thrown away**.
3. React restarts rendering from scratch with the newer state.

##### The Catastrophic Anti-Pattern: Side Effects During Render
```tsx
// ❌ CRITICAL BUG: Side effect executed during Render!
function SettlementForm({ debtorId, settlementAmount }) {
  // If React abandons or restarts this render, this HTTP call or analytics event STILL FIRED!
  trackEvent('SETTLEMENT_VIEWED', { debtorId, amount: settlementAmount });
  
  // Worse: Mutating an external global variable or initiating network mutations
  window.lastEvaluatedAmount = settlementAmount;

  return <div>Settlement: R{settlementAmount}</div>;
}
```
If React evaluates this component twice (as in development Strict Mode, or when interrupted by user input), the analytics endpoint records duplicate phantom events, and external systems receive unsynchronized commands for UI that was never committed to the user's screen!

##### The Rule:
> **The Render phase must be a pure projection: $\text{UI} = f(\text{props}, \text{state})$. It must calculate values, return JSX descriptions, and do NOTHING that leaves an irreversible footprint on the outside world.**

---

#### 3. What Belongs in Render vs. Commit vs. Event Handlers?

| Responsibility | Phase / Location | Valid Code Examples | Prohibited Anti-Patterns |
| :--- | :--- | :--- | :--- |
| **Pure Calculations & UI Description** | **Render Phase** (Component body) | Computing discounts, formatting currency (`Intl.NumberFormat`), filtering memoized lists, returning JSX. | `fetch()`, `setTimeout`, writing to `localStorage`, mutating external variables. |
| **User-Initiated Mutations** | **Event Handlers** (`onClick`, `onSubmit`) | Dispatching server actions, triggering payment mutations, generating idempotency keys. | Putting transaction mutations inside `useEffect` or render. |
| **Synchronous DOM Reading & Layout Adaptation** | **Commit Phase** (`useLayoutEffect`) | Measuring DOM dimensions (`getBoundingClientRect`), adjusting tooltip positions to avoid viewport clipping. | Long-running CPU work, network requests, state updates that trigger layout loops. |
| **Passive External Synchronization** | **Commit Phase (Post-Paint)** (`useEffect`) | Setting up WebSocket listeners, updating document title, logging analytics, subscribing to external stores. | Synchronously measuring DOM geometry where layout jumps cause visible flicker. |

---

#### 4. The Complete Execution Sequence: Order of Operations

When an agent clicks "Apply Discount" and triggers a state update:

```text
1. USER EVENT HANDLER FIRES:
   • Agent clicks button ──► `onClick` handler executes.
   • `setDiscount(20)` dispatches.

2. RENDER PHASE (Interruptible):
   • React evaluates `PaymentForm({ discount: 20 })`.
   • Computes `discountedAmount = amount * 0.8`.
   • Generates new React element tree.
   • Reconciler diffs against current Fiber tree and marks mutations on `workInProgress`.

3. COMMIT PHASE (Synchronous & Uninterruptible):
   • Step 3A: DOM Mutation Pass
     React applies host changes to the real DOM (e.g. updating input value, updating text).
   • Step 3B: Layout Effect Pass (`useLayoutEffect`)
     Synchronously invokes `useLayoutEffect` callbacks.
     DOM nodes are in their final positions, but the browser has NOT painted yet!
     If state is updated here, React synchronously re-renders before paint.
   • Step 3C: Fiber Pointer Flip
     `root.current = workInProgress`.

4. BROWSER PAINT OPPORTUNITY:
   • React yields control back to the browser event loop.
   • The browser C++ engine runs: `Style Recalculate` ──► `Layout` ──► `Paint` ──► `Composite`.
   • Physical pixels update on the agent's screen.

5. PASSIVE EFFECT PASS (`useEffect`):
   • React executes queued `useEffect` callbacks asynchronously after paint.
   • Analytics events, telemetry, and background socket synchronization fire.
```

---

#### 5. `useLayoutEffect` vs. `useEffect`: When to Use Each

##### The Golden Rule:
* Default to **`useEffect`** for 99% of synchronization.
* Use **`useLayoutEffect`** **only** when reading layout properties and mutating the DOM before the browser paints to prevent **visual flickering**.

##### Concrete Contact Centre Example (Tooltip / Popover Positioning):
An agent hovers over an overdue account badge. A tooltip opens:
* If rendered via `useEffect`:
  1. Tooltip mounts at default `(0, 0)`.
  2. Browser paints the tooltip at the top-left of the screen.
  3. `useEffect` runs asynchronously, measures the trigger button, and computes `left: 420px, top: 180px`.
  4. Tooltip jumps across the screen to its corrected position.
  5. **Result: Perceptible visual jitter and Cumulative Layout Shift (CLS).**
* If rendered via `useLayoutEffect`:
  1. Tooltip mounts in the DOM.
  2. `useLayoutEffect` runs synchronously *before* paint. It reads `getBoundingClientRect()` and applies `left: 420px, top: 180px`.
  3. Browser paints **only once**, rendering the tooltip directly at its final, correct coordinates.
  4. **Result: Zero visual jitter.**

---

### The Hostile Curveball Defense

> **Interviewer**: *"A developer says: 'I'll put my payment API call in `useLayoutEffect` because then it happens immediately after the DOM updates, rather than waiting for paint.' Why is that a terrible idea? How would you architect: 'Apply Discount' $\to$ calculate $\to$ save arrangement $\to$ show confirmation?"*

---

### Candidate Defense:

#### 1. Why Putting an API Call in `useLayoutEffect` is an Anti-Pattern:
*"Placing an API call or transaction dispatch inside `useLayoutEffect` is fundamentally flawed for two reasons:

1. **Blocks the Main Thread & Delays Paint**:
   `useLayoutEffect` runs **synchronously before the browser can paint**. While calling `fetch()` itself initiates an asynchronous network request, setting up request payloads, headers, or state transitions inside `useLayoutEffect` directly delays the browser from rasterizing the current frame. The agent's screen freezes, unable to reflect that the button was even clicked!
2. **Confuses Synchronization with Intent**:
   Effects are meant to **synchronize React state with external systems**. A financial payment submission or discount application is an **intentional user action**, not an ambient synchronization side effect of rendering. If you tie transactions to component mount/update effects, you risk duplicate submissions on re-renders, route transitions, or Strict Mode double-invocations."*

---

#### 2. The Production Architecture for Financial Arrangement Updates:

We strictly decouple **Intent**, **Transactional Truth**, **Optimistic UI**, and **Synchronization**:

```text
[ USER INTENT ]               Agent clicks "Apply Discount"
       │
       ▼
[ EVENT HANDLER ]            `handleApplyDiscount()` fires:
                             1. Generates cryptographic `idempotencyKey`.
                             2. Sets UI to optimistic 'SUBMITTING' state.
                             3. Initiates server mutation.
       │
       ▼
[ SERVER MUTATION ]          `POST /api/arrangements/{id}/discount`
                             Authoritative financial engine validates legal limits.
       │
       ▼
[ TRANSACTION COMMIT ]       Server returns `200 OK` with audited settlement contract.
                             Component commits confirmed state.
       │
       ▼
[ PASSIVE EFFECT ]           `useEffect` fires post-paint:
                             Emits audit telemetry to compliance logs.
```

##### Production Implementation:
```tsx
export function ArrangementWaiverPanel({ arrangementId, currentBalanceCents }) {
  const [status, setStatus] = useState<'IDLE' | 'SAVING' | 'CONFIRMED' | 'ERROR'>('IDLE');
  const [confirmedArrangement, setConfirmedArrangement] = useState(null);

  // 1. EVENT HANDLER: Transactional Intent
  const handleApplyDiscount = async (discountPercent: number) => {
    setStatus('SAVING');

    try {
      // Transactional boundary: Authoritative financial state on server
      const response = await fetch(`/api/arrangements/${arrangementId}/discount`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-Idempotency-Key': `DISC-${arrangementId}-${Date.now()}`
        },
        body: JSON.stringify({ discountPercent })
      });

      if (!response.ok) throw new Error('Waiver rejected by compliance rules');

      const data = await response.json();

      // State update triggers Render -> Commit -> Paint
      setConfirmedArrangement(data);
      setStatus('CONFIRMED');
    } catch (err) {
      setStatus('ERROR');
    }
  };

  // 2. PASSIVE EFFECT: Telemetry (Runs post-paint, never blocks UI)
  useEffect(() => {
    if (status === 'CONFIRMED') {
      telemetryClient.log('DISCOUNT_APPLIED', { arrangementId });
    }
  }, [status, arrangementId]);

  // 3. RENDER: Pure UI description
  return (
    <div>
      <button 
        onClick={() => handleApplyDiscount(20)} 
        disabled={status === 'SAVING'}
      >
        {status === 'SAVING' ? 'Applying Waiver...' : 'Apply 20% Discount'}
      </button>

      {status === 'CONFIRMED' && (
        <div role="status">New Balance: R{confirmedArrangement.newBalanceCents / 100}</div>
      )}
    </div>
  );
}
```

> **The Architectural Rule**:
> **“UX responsiveness must never compromise transactional truth. User actions belong in event handlers; pure UI belongs in Render; layout measurements belong in `useLayoutEffect`; external synchronization belongs in `useEffect`.”**

---

## Q3.3: Component Identity, Keys & Preventing Cross-Customer Data Leaks

### Scenario — Nutun Debtor Workspace Context Switch
An agent is handling Customer A (Absa Loan portfolio) in the collections workspace.
The agent types draft negotiation notes into the payment form:
> *“Customer agreed to R1,850/month pending salary deposit on the 25th...”*

Suddenly, an incoming CTI event switches the workspace context to:
> **Customer B (Woolworths Store Card portfolio)**

The parent workspace re-renders. The arrangement form remains mounted at the same location in the component hierarchy:
```tsx
// Before context switch (Customer A)
<WorkspaceLayout>
  <ArrangementForm customerId="CUST-A" defaultBook="Absa" />
</WorkspaceLayout>

// After context switch (Customer B)
<WorkspaceLayout>
  <ArrangementForm customerId="CUST-B" defaultBook="Woolworths" />
</WorkspaceLayout>
```
To the agent's shock, the form still displays Customer A's draft settlement notes and proposed repayment amount under Customer B's header!

### Interview Question
> **“Gift, walk me through exactly how that can happen in React under the hood. What does React's `key` prop actually do mechanically during reconciliation? Why is `key` NOT a security boundary? Why can't we just put `key={Math.random()}` on every component? And if Customer A had an AI stream running when the agent switched to Customer B, show me where your architecture prevents Customer A's late tokens from entering Customer B's UI.”**

---

### Verified Candidate Answer

#### 1. Why the Draft Leaked: Component Identity vs. Data Identity
What occurred here is a fundamental clash between **component identity** and **data identity** in React’s reconciliation engine:

1. **State Lives with the Fiber Representation, Not the JSX**:
   - In React, local state (`useState`, `useReducer`) is associated with the component's position and identity in React's internal Fiber tree.
   - When the parent re-renders with `customerId="CUST-B"`, React evaluates the element at that exact position in the tree:
     * *Is the element type identical?* **Yes** (`ArrangementForm`).
     * *Did its key change?* **No key was provided**, so React falls back to position-based identity.
2. **Fiber Node Reuse**:
   - React concludes: *“This is the exact same component instance. I do not need to unmount DOM nodes or recreate state. I will simply pass the new props (`customerId="CUST-B"`) to the existing Fiber instance.”*
3. **The Trap of Local Draft State**:
   - The state—`const [notes, setNotes] = useState('Customer agreed...')`—was initialized once during Customer A's mount.
   - Props changed, but local uncontrolled or draft state was **never reset**. The Fiber preserved its hook state memory.
   - As a result, Customer B's workspace inherits Customer A's confidential financial draft.

---

#### 2. What `key` Actually Does Mechanically
When you assign an explicit domain key:
```tsx
<ArrangementForm key={selectedCustomer.id} customer={selectedCustomer} />
```
You explicitly instruct React's reconciler: **"The identity of this component is strictly bound to this specific customer ID."**

When `selectedCustomer.id` transitions from `CUST-A` to `CUST-B`:
1. React detects a **key mismatch** at that tree position.
2. React marks the old Fiber node for deletion (`Deletion` effect tag).
3. It unmounts Customer A's component, executing all cleanup effects.
4. It completely destroys the previous Fiber's local state memory.
5. It mounts a brand-new Fiber instance for Customer B, initializing fresh state and clean form fields.

---

#### 3. Why `key` is NOT a Security Boundary (The 4-Layer Defense)
In an enterprise financial application like Nutun, **a React `key` alone is not a security boundary and cannot guarantee data isolation**:
* **In-Flight Asynchronous Operations**: If Customer A had an AI copilot stream running or a financial calculation in flight, unmounting the component does not terminate the network stream unless wired to an `AbortController`. Unhandled promises can still resolve into global stores.
* **Cached State & Global Stores**: If drafts are saved in global state (Redux/Zustand) or a query cache (TanStack Query) under generic keys like `'arrangement-draft'`, remounting the component simply rehydrates Customer A's cached draft.
* **Regulatory Consequence**: Disclosing one debtor's financial balances or settlement offers to another consumer is a severe confidentiality breach and regulatory violation.

##### The Production 4-Layer Identity Agreement:
```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE 4-LAYER IDENTITY AGREEMENT                       │
│                                                                        │
│ 1. UI IDENTITY (React Key)                                             │
│    • Keyed by `customerId`: forces clean unmount & state wipe on switch │
│                                                                        │
│ 2. ASYNC LIFECYCLE (AbortController + Epoch)                           │
│    • Switching customer immediately cancels in-flight streams & fetch  │
│    • Request ID epoch drops any resolving microtasks                   │
│                                                                        │
│ 3. CACHE IDENTITY (Scoped Keys)                                        │
│    • TanStack Query / Form Drafts strictly keyed by `['draft', custId]`│
│    • Drafts are partitioned strictly by customer ID                    │
│                                                                        │
│ 4. TRANSACTIONAL AUTHORIZATION (Backend Contract)                      │
│    • Submitting an arrangement requires `{ customerId, expectedHash }` │
│    • Backend rejects the payload if customerId doesn't match active call│
└────────────────────────────────────────────────────────────────────────┘
```

> **The Invariant**:
> **“The UI identity, the network lifecycle, the client-side cache, and the backend authorization token must all agree on which customer is active.”**

---

### The Hostile Curveball Defenses

#### Curveball 1:
> **Interviewer**: *"Why don't we just put `key={Math.random()}` on every component? Every render gets a new identity. That guarantees no stale state. Why isn't that the safest architecture for a financial application?"*

#### Candidate Defense:
*"That sounds attractive on the surface because it feels like an aggressive fail-safe, but in reality, `key={Math.random()}` destroys the very invariants that make an interactive financial application usable, accessible, and correct:

1. **The Form Input Focus Death Spiral**:
   - Every single keystroke updates state, which triggers a re-render.
   - Because `key` is random, React unmounts the component and remounts a new DOM node.
   - The `<input>` that had focus is **physically removed from the DOM**, resetting browser focus to `document.body`! The agent types the first character, loses focus, and subsequent typing goes nowhere or triggers global hotkeys.
2. **Destruction of Legitimate In-Flight User State**:
   - A financial workspace relies on local draft state while the agent is negotiating. If an ambient dialler event, WebSocket heartbeat, or clock tick triggers a parent re-render, `Math.random()` instantly wipes out whatever the agent was typing. You haven't prevented stale state; you've created **uncontrolled data loss**.
3. **Effect & Network Flooding**:
   - Every mount re-executes `useEffect` and `useLayoutEffect`. If the component loads customer verification rules or account summaries on mount, `key={Math.random()}` fires those network requests on **every single keystroke**, DDOSing our internal API gateways.
4. **Total Accessibility Destruction**:
   - Screen readers maintain reading cursors in the Accessibility Tree. Tearing down and remounting the DOM on every state change resets screen reader anchors, repeatedly re-announcing the top of the page and disorienting the operator.

**The Engineering Principle**: A `key` represents **domain identity**, not a blunt hammer to wipe memory. It must match the real-world business entity: stable while working on Customer A, transitioning cleanly when Customer B takes the desk."*

---

#### Curveball 2:
> **Interviewer**: *"The customer changes while an AI response is streaming. The old stream was not aborted. The old response arrives after the new customer loads. Show me exactly where your architecture prevents Customer A's response from entering Customer B's UI."*

#### Candidate Defense:
*"Here is the exact mechanism that guarantees Customer A's late streaming tokens cannot contaminate Customer B's workspace:

```text
CUSTOMER A ACTIVE (epoch = 1, streamRequestId = "REQ-A-101")
   │
   ▼
[ SSE Stream A In-Flight ] ──► Chunks arriving...
   │
   ▼
AGENT SWITCHES TO CUSTOMER B:
   • activeCustomerRef.current = "CUST-B"
   • activeEpochRef.current = 2 (Monotonically incremented)
   │
   ▼
STREAM A CHUNK ARRIVES LATE:
   • Chunk payload carries: `{ customerId: "CUST-A", requestId: "REQ-A-101" }`
   │
   ▼
[ THE INVARIANT CHECK IN THE STREAM REDUCER / CONTROLLER ]:
   if (
     chunk.customerId !== activeCustomerRef.current || 
     chunk.requestId !== activeStreamRequestRef.current
   ) {
     // DISCARD SILENTLY! Zero state mutation, zero DOM update.
     return;
   }
```

Even if `abort()` failed or network packets were already buffered in the browser's TCP stack:
1. The stream controller checks the incoming chunk's `customerId` and `requestId` against the active workspace's **authoritative customer epoch**.
2. Because `chunk.customerId ("CUST-A") !== activeCustomerRef.current ("CUST-B")`, the chunk is discarded at the boundary.
3. React's state reducer is never invoked for that chunk, no state mutations occur, and Customer B's workspace remains completely uncontaminated.

This proves our governing rule:
> **“Cancellation improves efficiency; Request Identity establishes correctness.”**"*
