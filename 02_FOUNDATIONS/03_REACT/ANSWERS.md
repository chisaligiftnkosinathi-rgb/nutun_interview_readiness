# React Lesson 1: Answers & Technical Evaluation

## A — Why React?

### Candidate Answer
Declarative UI became useful because complex interactive applications create substantial coordination work when developers imperatively manipulate the DOM from many independent event handlers.

The core distinction is:

```text
Imperative:
state changes
    ↓
developer manually determines DOM mutations

Declarative:
state
    ↓
UI = f(state)
    ↓
React determines required host updates
```

In an imperative codebase, multiple asynchronous events (e.g. telephony events, AI streaming, REST fetches) trigger handlers that directly modify DOM elements. Keeping the DOM synchronized with internal state becomes an exponential coordination problem. Declarative UI shifts the burden of computing and applying host DOM mutations to the library, allowing developers to write components as pure descriptions of state.

### Assessment
🟢 **KNOW**

### Precision Note
Do not assert that React was historically created solely or explicitly to solve this exact generalized coordination problem. That is our architectural explanatory framing; historical claims are separately categorized as documented / attributed / inferred.

---

## B — Component

### Candidate Answer
When React renders:
```tsx
function Customer({ customerId }) {
    return <div>{customerId}</div>;
}
```

Conceptually:
1. **Plain Function Invocation**: React invokes the component function with props: `Customer({ customerId: "..." })`.
2. **Distinct Lexical Environment**: The invocation creates its own JavaScript execution context and lexical environment in heap memory.
3. **Element Description Generation**: JSX evaluates into a React element description—a lightweight, plain JavaScript object (`{ type: "div", props: { children: customerId } }`).
4. **Not a DOM Node**: A React element is merely an immutable description; it is not a browser DOM node.
5. **Internal Machinery**: React's internal Fiber architecture receives this element description to determine work units and scheduling.
6. **Host Updates**: The resulting reconciliation work may eventually produce mutations that are applied to the host DOM.

### Assessment
🟢 **KNOW**

---

## C — Render vs Commit

### Candidate Answer

### Render Phase
During render work, React invokes components and calculates the next UI representation.
* This work is primarily calculation and reconciliation work in JavaScript memory.
* Modern React can schedule, prioritize, pause, or interrupt render work if higher-priority user input arrives.
* **No DOM mutations occur, and zero pixels are painted.**

### Commit Phase
The commit phase applies the completed result to the host environment.
* For a browser renderer, this means applying the necessary DOM mutations in a single, synchronous pass.
* React updates DOM refs, runs layout effects (`useLayoutEffect`), and hands control to the browser.
* The browser then executes its own rendering pipeline (Style Recalculation → Layout → Paint → Composite).
* React subsequently schedules passive effects (`useEffect`) to run asynchronously after the paint.

### Assessment
🟢 **KNOW**

### Required Precision Rules
* Do NOT state that render creates pixels.
* Do NOT state that React render equals browser paint.
* Do NOT collapse render and commit into one operation.

---

## D — Fiber

### Candidate Answer
Fiber addresses React's ability to organize, schedule, and prioritize rendering work.

Rather than running a synchronous recursive stack walk (which blocked the main JavaScript thread in React 15 and earlier), Fiber models rendering as discrete work units. Each Fiber node represents component or host-element work and identity, connected through explicit structural relationships:
* `child`: First child Fiber node.
* `sibling`: Next sibling Fiber node.
* `return`: Parent Fiber node (the return destination for completed work units).

This internal architecture allows React to:
1. Break rendering work into small, time-sliced units.
2. Yield execution back to the browser Event Loop for urgent user interactions (keystrokes, clicks).
3. Prioritize high-urgency updates over background data fetching.
4. Discard obsolete render trees if newer state arrives before completion.

### Assessment
🟢 **KNOW**

### Precision Correction
Do NOT describe Fiber merely as "a singly-linked list." That is incomplete.
*Use*: Fiber is an internal React architecture representing units of rendering work and identity, connected through relationships such as child, sibling, and return.

---

## E — Reconciliation

### Candidate Answer
Given:
```text
Old UI: Customer → Balance R1,000
New UI: Customer → Balance R1,500
```

React compares the newly produced element structure with the existing React/Fiber representation and determines how identity and changes should be handled.

Mechanically:
1. React checks the node at that tree position: the element type remains unchanged (`<p>`).
2. Because the element type is identical, React preserves the existing host DOM node and component identity.
3. React diffs the properties and discovers that only the children text has changed.
4. React updates only the text content of the relevant host node during commit, rather than reconstructing the entire subtree.

### Assessment
🟢 **KNOW**

### Precision Correction
Do NOT promise: "React always calculates the mathematically minimal set of DOM mutations."
*Use*: React uses reconciliation heuristics ($O(n)$ diffing rules based on element type and keys) to determine how the previous and next trees correspond and what host updates are required.

---

## F — JavaScript Connection (Stale Closures)

### Candidate Answer
Every React render is a distinct function invocation with its own closed-over lexical environment.

When a component renders, its props, state, and local bindings form an immutable snapshot for that specific render pass. Any asynchronous callback, timer, or effect created during that render closes over that render's snapshot in heap memory.

Later renders create entirely new execution contexts with new bindings. Therefore, an asynchronous callback from an earlier render that resolves later can observe the old snapshot values.

This mechanical reality explains:
* Why hook dependency arrays are necessary (to trigger re-subscription when bindings change).
* Why functional state updates (`setBalance(prev => prev + 100)`) are required to operate on atomic, latest state.
* Why `useRef` is used when a mutable reference is needed across render boundaries without triggering a re-render.

### Assessment
🟢 **KNOW**

---

## G — Interview Challenge Delivery (60–90s Pitch)

> *"In the early web, interfaces were built imperatively using tools like jQuery. If customer data changed, developers manually queried DOM nodes, altered text, toggled classes, and displayed spinners. As applications grew into complex dashboards with multiple asynchronous streams—such as live telephony, agent chat, and balance updates—keeping the DOM synchronized with underlying state became an unmaintainable coordination problem, often causing event handlers to overwrite each other or leave stale UI elements on screen.
> 
> React was introduced to address this coordination problem through a declarative model: UI equals a function of state ($\text{UI} = f(\text{state})$). Instead of writing step-by-step instructions on how to mutate the DOM, developers write pure component functions that describe what the UI should look like for any given state snapshot.
> 
> Mechanically, when state changes in React:
> 1. React invokes the component function, which produces a declarative React element description (a plain JavaScript object tree, not real DOM nodes).
> 2. React's internal Fiber architecture performs reconciliation and render work in memory, diffing the new element tree against existing Fiber nodes to calculate what changed. This work can be scheduled, prioritized, or interrupted.
> 3. Once the render work is complete, React enters the Commit phase, applying the necessary mutations to the real browser DOM in a single synchronous pass.
> 4. The browser then executes its own rendering pipeline to paint the updated frame.
> 
> It separates the calculation of what should change from the host environment mutations that apply those changes."*
