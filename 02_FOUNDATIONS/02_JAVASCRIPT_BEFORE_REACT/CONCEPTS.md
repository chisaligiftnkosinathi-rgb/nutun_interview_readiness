# Lesson 2: Concepts & Definitions

### 1. Lexical Scope
Scope determined at author-time by where functions and blocks are declared in source code, not where they are called. Inner scopes have access to identifiers declared in outer enclosing scopes via the scope chain.

### 2. Closures
A function bundled together with references to its surrounding lexical environment. A closure allows an inner function to access and mutate variables in an outer function even after that outer function has returned and exited the Call Stack.

### 3. The Call Stack
LIFO (Last In, First Out) execution stack where JavaScript keeps track of active execution contexts and synchronous function calls.

### 4. The Event Loop, Microtasks & Macrotasks
* **Call Stack**: Executes synchronous code.
* **Microtask Queue** (`Promise.then`, `queueMicrotask`): Completely drained immediately when the current Call Stack empties, before any rendering or macro-tasks run.
* **Rendering Opportunities**: Style, layout, and paint can occur after microtasks drain.
* **Task / Macrotask Queue** (`setTimeout`, `setInterval`, I/O, UI events): Executes one task per turn of the event loop.

### 5. `async` / `await`
Syntactic sugar over Promises and generators. `await` pauses only the execution of the enclosing `async` function and yields control back to the event loop, allowing other synchronous and asynchronous tasks to execute on the main thread while the awaited Promise is pending.

### 6. `AbortController`
Web API providing a cancellation signal (`AbortSignal`) that fetch and other APIs observe. When aborted, pending operations reject with `AbortError`, allowing application cleanup without making transport-layer socket termination guarantees.

### 7. Asynchronous Completion Order vs Request Order
`Request Order ≠ Completion Order`. Promises settle independently based on variable network and server latencies. Sequential requests (`A → B → C`) frequently finish out of order (`B → C → A`), causing race conditions if unhandled.

### 8. Cancellation vs Validity
* **Cancellation**: Asks *"Can we stop the in-flight operation from completing or consuming more resources?"*
* **Validity**: Asks *"Even if the operation completes, is this result still relevant and authoritative for the current UI state?"*
Cancellation alone does not guarantee stale-result protection; client-side validity checks and identity-bound server state are mandatory.

### 9. Stale Closures in React
Occurs when an asynchronous callback, effect, or event handler captures an immutable snapshot of props and state from an earlier render's lexical environment. If the component re-renders with new state while the callback remains in-flight, the callback "remembers" and acts upon outdated values unless properly synchronized (via hook dependency arrays, mutable `useRef` containers, or functional state setters).

### 10. Component Identity vs Security Boundaries
React `key` attributes reset component state across identity transitions via unmount/remount. However, React keys are UI lifecycle tools, not security boundaries. Enterprise transactional truth requires backend authorization, optimistic concurrency versioning (ETags), and request idempotency keys.

