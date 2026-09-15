# React Lesson 1: Concepts & Definitions

### 1. Declarative UI
A programming model where the developer describes *what* the UI should look like for a given state snapshot, leaving the runtime to determine the host operations required to reach that state. Expressed conceptually as:
$$\text{UI} = f(\text{state})$$

### 2. Imperative DOM Manipulation
Directly instructing the browser host environment step-by-step on how to locate, mutate, insert, and delete individual DOM nodes (e.g., `document.getElementById`, `element.appendChild`).

### 3. React Component
A JavaScript function (or historically an ES6 class) that accepts props as input and returns a React element description of the intended UI.

### 4. React Element
A lightweight, immutable, plain JavaScript object describing a virtual UI node (e.g. `{ type: 'button', props: { children: 'Pay' } }`). Created via JSX or `React.createElement`. It is **not** a browser DOM node.

### 5. JSX
An XML-like syntactic extension for JavaScript that compiles at build time into standard JavaScript function calls that produce React elements.

### 6. Host DOM
The actual tree of C++ objects managed by the browser runtime (e.g., `HTMLDivElement`, `Text`) that participates in layout, paint, and OS event dispatching.

### 7. Fiber Architecture
React's internal architecture representing units of rendering work and component identity, connected through relationships such as `child`, `sibling`, and `return`. It enables React to schedule, prioritize, pause, resume, and abort rendering work.

### 8. Reconciliation
The algorithm and heuristic process by which React compares a newly returned React element tree with the existing Fiber representation to determine what host updates are needed.

### 9. Render Phase
The phase in which React invokes component functions, produces the new React element tree, and diffs it against existing Fiber nodes to calculate required changes. It is purely in-memory, produces no DOM mutations, and can be interrupted or rescheduled in modern React.

### 10. Commit Phase
The phase in which React takes the completed calculation from the render phase and synchronously applies the necessary mutations to the host environment (the browser DOM), updates refs, runs layout effects, and allows the browser to paint.

### 11. Component Identity & React Keys
The mechanism React uses to track which elements correspond across renders.
> **Security & Architectural Principle**: A React `key` is a **component identity and lifecycle mechanism**. It tells React whether to preserve or reset local component state across renders. It is **NOT an authentication, authorization, privacy, or security boundary**.

### 12. Render Snapshot
The immutable set of props, state, and local variable bindings created during a single component function invocation.

### 13. Stale Closure
A scenario where an asynchronous callback, effect, or event handler retains an active reference to a Lexical Environment from an earlier render snapshot, thereby reading or acting upon outdated values.
