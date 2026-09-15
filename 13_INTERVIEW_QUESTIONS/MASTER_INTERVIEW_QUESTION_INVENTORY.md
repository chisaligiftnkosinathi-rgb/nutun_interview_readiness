# Master Interview Question Inventory & Coverage Audit

> **GOVERNING SOURCE**: Nutun Front-End Engineer Job Specification (OfferZen Facilitated)  
> **OPERATIONAL CONTEXT**: 18.5M monthly customer interactions, up to 10,000 concurrent agents, AI-powered conversational systems, real-time telephony, and financial debt restructuring workflows.  
> **EVALUATION CODES**:  
> * 🛡️ **INTERVIEW READY**: Exit test passed with timed verbal defense & curveball survival without notes.  
> * 🟢 **KNOW**: Mechanically understood with verified code evidence in `C:\Projects`.  
> * 🟡 **UNDERSTAND**: Conceptually understood, but needs structured interview drill or edge-case defense.  
> * 🔴 **NOT YET**: Identified gap; question drafted, answer model pending.

---

## Executive Summary: Surface Area & Readiness Map

| Domain / Category | Total Questions | 🛡️ Ready | 🟢 Know | 🟡 Understand | 🔴 Not Yet | Primary Interview Risk / Trap |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **1. Semantic HTML5 & DOM** | 8 | 0 | 4 | 3 | 1 | Treating accessibility as an afterthought; `<div onClick>` habits |
| **2. Modern CSS3 & Responsive UI** | 8 | 0 | 4 | 3 | 1 | Flexbox vs Grid misuse; layout shifts (CLS); compositor ignorance |
| **3. React Mechanics & Architecture** | 8 | 0 | 6 | 2 | 0 | Believing props trigger re-renders; Fiber internals vs Stack |
| **4. JavaScript Runtime & Async** | 8 | 0 | 5 | 3 | 0 | Event loop microtask starvation; stale closures in hooks |
| **5. REST / Web APIs & Networking** | 6 | 2 | 3 | 1 | 0 | Collapsing TTFB to backend code; lack of idempotency keys |
| **6. AI / LLM Frontend Engineering** | 8 | 0 | 4 | 3 | 1 | Frontend API key leaks; handling malformed streaming JSON |
| **7. Streaming & Token-Level UI** | 6 | 0 | 3 | 2 | 1 | Global re-renders on every token; stream memory leaks |
| **8. RAG & Vector Search UI** | 6 | 0 | 1 | 3 | 2 | Displaying hallucinated answers without citation grounding |
| **9. Accessibility (WCAG 2.1 AA)** | 6 | 0 | 1 | 4 | 1 | Missing ARIA live regions for AI streams; broken focus traps |
| **10. High Performance & Scalability** | 6 | 0 | 3 | 3 | 0 | Virtualization memory vs DOM confusion; unmemoized props |
| **11. Automated Testing & Quality** | 6 | 0 | 1 | 3 | 2 | Testing implementation details instead of user behavior |
| **12. Wireframe → Production Ownership** | 4 | 0 | 2 | 2 | 0 | Building UI without edge states (loading, empty, error, slow) |
| **13. Cross-Cutting Systems Scenarios** | 4 | 0 | 1 | 3 | 0 | AI token streaming freezing contact centre agent dashboard |
| **TOTAL** | **84** | **2** | **37** | **35** | **10** | **44% Ready/Known; 56% Target Practice Universe** |

---

## Category 1: Semantic HTML5 & DOM Architecture

### Q1.1: Why Semantic HTML Over `<div>` Soup?
* **Job Spec Requirement**: Semantic HTML5 markup, accessibility, SEO, document structure.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Semantic landmarks (`<header>`, `<main>`, `<section>`, `<nav>`) in [`axis_clean/apps/web/src/App.tsx`](file:///c:/Projects/axis_clean/apps/web/src/App.tsx).
* **Question**: *"Why use semantic HTML5 elements like `<main>`, `<article>`, `<nav>`, and `<aside>` instead of styled `<div>` elements everywhere?"*
* **Core Technical Mechanism**: Accessibility Tree construction, built-in ARIA landmark roles, browser native keyboard navigation, screen reader rotor shortcuts.
* **Senior Follow-Up**: *"If an engineer styles a `<div>` to look like a button with `cursor: pointer` and adds `onClick`, what 4 critical capabilities did they fail to provide?"*
* **Hostile Curveball Trap**: *"Can't you just add `role="button"` and `tabIndex={0}` to that `<div>` and call it fully accessible?"*

### Q1.2: Accessible Form Architecture in Financial Debt Negotiation
* **Job Spec Requirement**: Intelligent UX, accessible forms, transactional data collection.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Code Evidence**: Form fields in `axis_clean`, debt arrangement forms.
* **Question**: *"In a high-volume debt repayment portal, what makes a multi-step financial arrangement form truly accessible according to WCAG 2.1 AA?"*
* **Core Technical Mechanism**: Explicit `<label htmlFor>`, `aria-describedby` for field errors and format hints, `aria-invalid`, grouping via `<fieldset>` and `<legend>`, and programmatic focus management to error summaries.
* **Senior Follow-Up**: *"When an agent hits 'Submit Arrangement' and server validation fails with 3 errors, where should keyboard and screen reader focus immediately move?"*
* **Hostile Curveball Trap**: *"Why should you never use `placeholder` text as a substitute for a `<label>`?"*

### Q1.3: `<button>` vs. `<a>` (Hyperlink vs. Action)
* **Job Spec Requirement**: Semantic HTML5, UI navigation integrity.
* **Difficulty**: Fundamentals | **Current Status**: 🟢 **KNOW**
* **Question**: *"When should you use an anchor tag `<a>` versus a `<button>`, and how does that affect browser behavior and accessibility?"*
* **Core Technical Mechanism**: `<a>` represents navigation (URL change, history push, middle-click open in new tab). `<button>` triggers a state action, mutation, or modal without URL departure.
* **Senior Follow-Up**: *"What happens if a developer puts an `<a>` inside a `<button>` or vice versa?"*
* **Hostile Curveball Trap**: *"How do you handle a button that opens an external document download while still retaining keyboard activation via Spacebar?"*

### Q1.4: Heading Hierarchy & Document Outline
* **Job Spec Requirement**: Semantic HTML5, accessibility.
* **Difficulty**: Fundamentals | **Current Status**: 🟢 **KNOW**
* **Question**: *"Explain the rules of heading hierarchy (`<h1>` through `<h6>`). Why is skipping from `<h2>` to `<h4>` an accessibility violation?"*
* **Core Technical Mechanism**: Screen readers generate outline trees allowing users to navigate by heading levels. Skipping levels disorients users relying on hierarchical table-of-contents mental models.
* **Senior Follow-Up**: *"If a designer demands that a sub-section title look visually like a tiny 12px label, what tag and styling approach do you use?"*

### Q1.5: Dynamic Content & Live Region Announcements
* **Job Spec Requirement**: Real-time telephony events, live chat, WCAG 2.1 AA.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"When an incoming debt collection call connects or an AI sentiment warning triggers in real-time, how do you inform a screen-reader user without pulling focus away from their current typing task?"*
* **Core Technical Mechanism**: `aria-live="polite"` vs `aria-live="assertive"`, `aria-atomic="true"`.
* **Senior Follow-Up**: *"Why would setting `aria-live="assertive"` on streaming LLM tokens be a catastrophic UX disaster for an agent using a screen reader?"*

### Q1.6: The First Rule of ARIA
* **Job Spec Requirement**: Semantic HTML5, accessibility standards.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"What is the 'First Rule of ARIA', and can you give an example where adding ARIA actually made a component worse?"*
* **Core Technical Mechanism**: If you can use a native HTML5 element or attribute with the semantics and behavior already built in, do so instead of re-purposing an element and adding ARIA.
* **Senior Follow-Up**: *"What happens when someone adds `role="presentation"` or `role="none"` to an accessible `<button>`?"*

### Q1.7: Accessibility Tree Inspection & DevTools Auditing
* **Job Spec Requirement**: Accessibility testing and compliance.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"How do you inspect the Accessibility Tree in Chrome or Firefox DevTools to verify what assistive technology actually sees?"*
* **Core Technical Mechanism**: Elements panel → Accessibility sub-panel → Computed Properties, Name, Role, Focusable, and full accessibility tree toggle.
* **Senior Follow-Up**: *"How does Lighthouse detect accessibility violations, and why does automated testing catch only ~30-40% of real WCAG issues?"*

### Q1.8: Custom Dialogs & Modal Focus Trapping
* **Job Spec Requirement**: Reusable components, accessible interactive workflows.
* **Difficulty**: Senior | **Current Status**: 🔴 **NOT YET**
* **Question**: *"How do you build a completely accessible modal dialog using either native `<dialog>` or React portals, including keyboard trapping and backdrop behavior?"*
* **Core Technical Mechanism**: `role="dialog"`, `aria-modal="true"`, focus capture (trapping `Tab` / `Shift+Tab` within modal bounds), `Escape` key close, and restoring focus to the triggering element upon close.

---

## Category 2: Modern CSS3 & Responsive UI Architecture

### Q2.1: Flexbox vs. CSS Grid: The Authoritative Decision Framework
* **Job Spec Requirement**: CSS3, Flexbox, Grid, responsive layouts.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Flexible dashboards in `axis_clean/apps/web`.
* **Question**: *"When do you choose CSS Grid versus Flexbox? What is the core structural difference between 1-dimensional and 2-dimensional layouts?"*
* **Core Technical Mechanism**: Flexbox is content-first (1-dimensional: row or column; items wrap based on intrinsic size). Grid is container-first (2-dimensional: rows AND columns aligned simultaneously).
* **Senior Follow-Up**: *"Can Flexbox replicate an aligned 2D data table across multiple rows when content widths vary? Why or why not?"*
* **Hostile Curveball Trap**: *"Why would you use Flexbox inside a CSS Grid cell?"*

### Q2.2: CSS Box Model & `box-sizing: border-box`
* **Job Spec Requirement**: CSS layout fundamentals.
* **Difficulty**: Fundamentals | **Current Status**: 🟢 **KNOW**
* **Question**: *"Explain the CSS box model. What is the exact difference between `content-box` and `border-box`, and why does modern CSS universally default to `border-box`?"*
* **Core Technical Mechanism**: `content-box` calculates `width = content`; padding and borders add onto the outer footprint. `border-box` includes padding and border within the declared width, preventing layout overflow calculations.
* **Senior Follow-Up**: *"How does margin collapsing work, and under what conditions do vertical margins NOT collapse?"*

### Q2.3: Stacking Contexts & The `z-index` Nightmare
* **Job Spec Requirement**: Component isolation, modal overlays.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"An agent opens a customer details dropdown, but it appears underneath a sidebar even though the dropdown has `z-index: 999999` and the sidebar has `z-index: 10`. What is happening mechanically, and how do you resolve it?"*
* **Core Technical Mechanism**: Stacking context creation (`opacity < 1`, `transform`, `filter`, `isolation: isolate`, `contain: paint`). A high `z-index` cannot escape a parent's lower stacking context.
* **Senior Follow-Up**: *"How does CSS `isolation: isolate` prevent child stacking contexts from leaking into sibling components?"*

### Q2.4: Containing Blocks & `position: absolute`
* **Job Spec Requirement**: CSS positioning, dropdowns, floating menus.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"How does an element with `position: absolute` determine its containing block? Does it always look for the nearest ancestor with `position: relative`?"*
* **Core Technical Mechanism**: Ancestor with `position` other than `static`, or an ancestor with `transform`, `perspective`, or `filter` set.
* **Senior Follow-Up**: *"What happens if you place a `position: fixed` element inside a parent that has `transform: translate(0, 0)` applied?"*

### Q2.5: Mobile-First Responsive Design Strategy
* **Job Spec Requirement**: Mobile-first approach, cross-device interfaces.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"What does 'mobile-first' actually mean in CSS architecture, and why is `min-width` media queries preferred over `max-width` queries?"*
* **Core Technical Mechanism**: Mobile-first builds baseline styles for constrained viewports without media queries, then progressively enhances layout via `min-width` breakpoints as screen real estate expands.
* **Senior Follow-Up**: *"How do you test a desktop-first design that looks great at 1440px but breaks completely on a 375px mobile device?"*

### Q2.6: Preventing Cumulative Layout Shift (CLS)
* **Job Spec Requirement**: High performance, web vitals, smooth UX.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What causes Cumulative Layout Shift (CLS) in a web app, and how do you prevent layout jumps when images, fonts, or asynchronous AI widgets load?"*
* **Core Technical Mechanism**: Aspect ratio reservation (`aspect-ratio: 16/9`), explicit `width`/`height` attributes on images, `font-display: swap` with size adjust, and skeleton placeholders for async components.
* **Senior Follow-Up**: *"Why do CSS animations using `top`, `left`, `width`, or `height` trigger layout recalculations while `transform` and `opacity` do not?"*

### Q2.7: CSS Performance: Reflow, Repaint, and GPU Compositing
* **Job Spec Requirement**: High-performance web apps, smooth 60fps animations.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Walk me through the browser render pipeline: JavaScript → Style → Layout → Paint → Composite. Which CSS properties trigger layout (reflow), and which bypass layout straight to the GPU compositor?"*
* **Core Technical Mechanism**: Layout triggers: geometry changes (`width`, `height`, `margin`, `padding`, `display`). Paint triggers: visual appearance (`color`, `background-color`, `box-shadow`). Composite-only: `transform`, `opacity`, `filter`.
* **Senior Follow-Up**: *"What is the risk of abusing `will-change: transform` across hundreds of table rows in an agent dashboard?"*

### Q2.8: CSS Architecture for Enterprise Design Systems
* **Job Spec Requirement**: Reusable components, scalable CSS.
* **Difficulty**: Intermediate | **Current Status**: 🔴 **NOT YET**
* **Question**: *"How would you architect CSS for a 50-component design system shared across multiple teams: Tailwind CSS, CSS Modules, or CSS-in-JS? What are the performance and bundle size trade-offs?"*
* **Core Technical Mechanism**: Runtime CSS-in-JS (Emotion, styled-components) introduces runtime style recalculation and bundle overhead. Zero-runtime solutions (Tailwind, Vanilla Extract, CSS Modules) compile ahead of time with zero JS main-thread styling penalty.

---

## Category 3: React Mechanics & Architecture

### Q3.1: Why React Exists & The State-to-Screen Pipeline
* **Job Spec Requirement**: High-performance React web applications.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: React foundation in `02_FOUNDATIONS/03_REACT/LESSON.md`.
* **Question**: *"What problem was React actually created to solve, and what does React do mechanically when state changes?"*
* **Core Technical Mechanism**: State coordination crisis in imperative code. Declarative UI: $\text{UI} = f(\text{state})$. Invocation produces element tree; diffed against Fiber; committed to DOM in batch.
* **Senior Follow-Up**: *"Why is a React Element not a DOM node?"*

### Q3.2: Fiber Node Structure (`child`, `sibling`, `return`) vs. Stack Reconciler
* **Job Spec Requirement**: High-performance React, scheduler mechanics.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Contact centre department tree scaffold in `LESSON.md` & `ANSWERS.md`.
* **Question**: *"Explain what Fiber represents, the purpose of `child`, `sibling`, and `return` pointers, and what problem this solved over the legacy Stack Reconciler."*
* **Core Technical Mechanism**: Cooperative work loop (`while (workInProgress !== null && !shouldYield())`). Call stack replaced by heap-allocated linked list. Interruptible render phase, atomic synchronous commit phase.
* **Senior Follow-Up**: *"Is the Commit Phase interruptible?"* (Answer: Never. Commit must be synchronous to avoid half-painted UI).

### Q3.3: Re-Render Triggers & The "Props Changing" Myth
* **Job Spec Requirement**: React performance, component lifecycle.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"A junior developer says: 'This child component re-rendered because its props changed.' Why is that explanation mechanically inaccurate?"*
* **Core Technical Mechanism**: Props changing is not an independent trigger. The parent re-rendered, and by default React recursively re-renders children regardless of prop values unless wrapped in `React.memo`.
* **Senior Follow-Up**: *"What actually triggers a re-render in React?"* (State update via setter, parent re-render, context value change, hook-triggered state update).

### Q3.4: Memoization Mechanics: `React.memo`, `useCallback`, `useMemo`
* **Job Spec Requirement**: High-performance React, memoization.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Production code patterns in `axis_clean/apps/web`.
* **Question**: *"Explain the exact mechanics of `React.memo`, `useCallback`, and `useMemo`. What is the most common reason `React.memo` fails to prevent a re-render?"*
* **Core Technical Mechanism**: Shallow equality check (`Object.is`). Inline function or object references created anew in parent break shallow equality.
* **Senior Follow-Up**: *"Why is wrapping every primitive calculation in `useMemo` an anti-pattern?"*

### Q3.5: Structural Optimization (Composition Over Memoization)
* **Job Spec Requirement**: Clean architecture, high-performance React.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Component composition in `axis_clean`.
* **Question**: *"How can you prevent unnecessary re-renders of an expensive 50-component dashboard using component composition (moving state down or lifting content up via `children`) without using `React.memo`?"*
* **Core Technical Mechanism**: Passing JSX as `children` evaluates the element in the parent's scope. When the wrapper re-renders, the `children` prop reference is identical, skipping reconciliation of the nested subtree.

### Q3.6: React Keys: Identity, State Preservation, and Security
* **Job Spec Requirement**: Dynamic list rendering, transactional integrity.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"Why is using array index as `key` dangerous in dynamic lists that can be sorted or filtered? And why is a React key NOT a security boundary?"*
* **Core Technical Mechanism**: Keys establish identity across renders. Index keys cause state leakage across shifted elements. Keys are internal UI lifecycle tools, not security boundaries.
* **Senior Follow-Up**: *"How can you use the `key` prop deliberately to reset a complex form's internal state on customer switch?"*

### Q3.7: `useLayoutEffect` vs. `useEffect`
* **Job Spec Requirement**: React hooks, DOM measurement, avoiding layout flicker.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"When should you reach for `useLayoutEffect` instead of `useEffect`? What are the consequences of abusing `useLayoutEffect`?"*
* **Core Technical Mechanism**: `useLayoutEffect` runs synchronously after DOM mutations but BEFORE the browser paints. Used for layout measurement and synchronous style adjustments to prevent flicker. Blocks browser painting.
* **Senior Follow-Up**: *"Why does SSR warn when using `useLayoutEffect`?"*

### Q3.8: React 18 Concurrency: Transitions & Deferred Values
* **Job Spec Requirement**: High-performance React, modern React 18+ features.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What is the difference between `useTransition` and `useDeferredValue`? How do they help keep a high-volume search input responsive while updating a heavy list?"*
* **Core Technical Mechanism**: `useTransition` wraps state setters marking updates as low-priority (interruptible). `useDeferredValue` wraps a value, deferring its update until urgent input renders complete.

---

## Category 4: JavaScript Runtime & Asynchronous Fundamentals

### Q4.1: Lexical Scope & Closures
* **Job Spec Requirement**: JavaScript fundamentals, runtime mechanics.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Foundation in `02_FOUNDATIONS/02_JAVASCRIPT_BEFORE_REACT/LESSON.md`.
* **Question**: *"Explain what a closure is without using technical jargon, and provide a real-world software example where a closure is essential."*
* **Core Technical Mechanism**: The Commuter Backpack analogy. Function bundled with references to its surrounding lexical environment; retains access after outer function exits the call stack.

### Q4.2: Stale Closures in React Hooks
* **Job Spec Requirement**: React state synchronization, bug diagnosis.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"What is a stale closure in React? Walk through an example where `setInterval` inside `useEffect` gets stuck on an initial counter value."*
* **Core Technical Mechanism**: Closure captures immutable snapshot of state from mount render. Solve via functional state updater `setCount(c => c + 1)` or `useRef`.

### Q4.3: The Event Loop: Microtasks vs. Macrotasks
* **Job Spec Requirement**: JavaScript runtime, non-blocking execution.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"Why does `Promise.resolve().then(...)` execute before `setTimeout(..., 0)`? Trace the exact sequence of the Call Stack, Microtask Queue, Macrotask Queue, and Browser Render Pass."*
* **Core Technical Mechanism**: Microtasks drain completely immediately after the call stack empties, before any macrotask runs and before the browser can paint.
* **Senior Follow-Up**: *"What is microtask starvation, and how does it freeze a web application?"*

### Q4.4: Asynchronous Race Conditions in Search Inputs
* **Job Spec Requirement**: Intelligent UX, async state, data integrity.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"An agent types fast in a customer search box: 'A' then 'B'. Request A takes 800ms; Request B takes 150ms. Request B returns first, then Request A overwrites it. How do you solve this race condition in React?"*
* **Core Technical Mechanism**: `Request Order ≠ Completion Order`. Solve using `AbortController` cancellation in `useEffect` cleanup or query-key isolation in TanStack Query.

### Q4.5: `AbortController`: Network Cancellation vs. UI Validity
* **Job Spec Requirement**: Resilient API consumption, clean async lifecycle.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"Explain the role of `AbortController`. Does aborting a `fetch` request guarantee that the backend server stopped processing the database query?"*
* **Core Technical Mechanism**: `AbortController` tells the browser to terminate socket reading and reject the client promise with `AbortError`. It does NOT guarantee server-side execution stoppage unless the backend listens for socket closure.

### Q4.6: `async` / `await` Under the Hood
* **Job Spec Requirement**: JavaScript async patterns.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"What does `await` actually do to the JavaScript thread? Does it block the browser?"*
* **Core Technical Mechanism**: `await` yields execution back to the event loop. The remainder of the `async` function is wrapped in a microtask callback that resumes after the Promise settles. The main thread is never blocked.

### Q4.7: Memory Leaks in Single-Page Applications
* **Job Spec Requirement**: Long-running enterprise dashboards (8-hour shifts without refresh).
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Nutun agents keep their dashboard open for 8 hours without refreshing. By 3 PM, the tab consumes 3GB of RAM and crashes. What common JavaScript patterns cause memory leaks in React SPAs?"*
* **Core Technical Mechanism**: Uncleared intervals/timeouts, uncleaned event listeners on `window`/`document`, retained closures holding massive objects in global state, detached DOM nodes.
* **Senior Follow-Up**: *"How do you diagnose a memory leak using the Chrome DevTools Memory Heap Snapshot comparison tool?"*

### Q4.8: Deep vs. Shallow Equality & Object Mutability
* **Job Spec Requirement**: State immutability, bug prevention.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Why must React state never be mutated directly (`state.customers.push(newCustomer)`)? How does `Object.is` determine whether a component should re-render?"*
* **Core Technical Mechanism**: React relies on referential equality (`Object.is`) to detect state changes. In-place mutation preserves the object reference; React assumes nothing changed and skips re-rendering.

---

## Category 5: REST / Web APIs & Networking Architecture

### Q5.1: Web Request Lifecycle Sequence
* **Job Spec Requirement**: Web APIs, microservices, network fundamentals.
* **Difficulty**: Senior | **Current Status**: 🛡️ **INTERVIEW READY (PASSED)**
* **Question**: *"What exact sequence of technical steps must occur before an HTTP request can reach the application server?"*
* **Defense**: URL parse → DNS resolve → TCP 3-way handshake → TLS 1.3 key negotiation → HTTP request serialization → Gateway/Reverse Proxy routing.

### Q5.2: TTFB Decomposition & Diagnostics
* **Job Spec Requirement**: Performance diagnostics, high TTFB resolution.
* **Difficulty**: Senior | **Current Status**: 🛡️ **INTERVIEW READY (PASSED)**
* **Question**: *"Explain TTFB: what constitutes it, and how do you diagnose whether high TTFB is caused by DNS, TLS, network transit, or backend execution?"*
* **Defense**: Correlate Chrome DevTools Waterfall timing with server APM distributed tracing. Decompose TTFB into network transit vs backend processing.

### Q5.3: The 1,982ms Infrastructure Mystery (Curveball Defense)
* **Job Spec Requirement**: High-availability enterprise infrastructure.
* **Difficulty**: Senior | **Current Status**: 🛡️ **INTERVIEW READY (PASSED)**
* **Question**: *"DNS/TCP/TLS are 0ms (HTTP/2 reuse), TTFB is 2,000ms, but backend APM shows DB and controller took only 18ms. Where did the remaining 1,982ms go?"*
* **Defense**: Reverse proxy worker queueing, geographic latency/packet loss, API gateway authentication/rate-limiting middleware, or intermediary response buffering (`proxy_buffering`).

### Q5.4: Idempotency Keys in Financial Transactions
* **Job Spec Requirement**: Financial integrity, payment arrangements, network resilience.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"An agent clicks 'Submit Payment Arrangement' (R1,500). The network connection drops before the HTTP 200 response arrives. The agent clicks Submit again. How do you prevent the customer from being charged twice?"*
* **Core Technical Mechanism**: Client generates a unique UUID `Idempotency-Key` header on initial submit. Retries pass the exact same key. The backend deduplicates against a transactional cache and returns the original result without re-executing payment.

### Q5.5: Optimistic Updates & Concurrency Control (ETags)
* **Job Spec Requirement**: Concurrent agent interactions, transactional truth.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"Two agents open the same customer debt file simultaneously. Agent 1 promises a 20% discount; Agent 2 updates the contact details. How do you prevent Agent 2 from silently overwriting Agent 1's financial arrangement?"*
* **Core Technical Mechanism**: Optimistic concurrency control via `ETag` / `If-Match` headers or entity version numbers (`version: 4`). If the version has changed on the server, reject with HTTP `412 Precondition Failed`.

### Q5.6: TanStack Query: Caching & Stale-While-Revalidate
* **Job Spec Requirement**: Async state management, intelligent UX.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Production implementation in `axis_clean/apps/web/package.json`.
* **Question**: *"What is the Stale-While-Revalidate caching pattern, and how does TanStack Query manage cache invalidation using structured query keys?"*
* **Core Technical Mechanism**: Displays cached (stale) data immediately for instant UX while asynchronously fetching fresh data in the background, updating the UI seamlessly when the network resolves.

---

## Category 6: AI / LLM Frontend Engineering

### Q6.1: Architecture of an LLM Integration in React
* **Job Spec Requirement**: High-performance React integrated with AI/LLM-powered features.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: AI service integration in [`science-of-our-world/src/services/aiService.ts`](file:///c:/Projects/science-of-our-world/src/services/aiService.ts).
* **Question**: *"How do you architect an LLM-powered feature in a React enterprise application? Walk through the complete lifecycle from user prompt to rendered response."*
* **Core Technical Mechanism**: UI prompt input → Client validates format → Dispatches to authenticated Backend Proxy (never direct to OpenAI/Anthropic from browser) → Backend enriches prompt with customer context/RAG → Streams tokens back via SSE/chunked HTTP → Frontend Reader incrementally updates state.

### Q6.2: API Key Security & Frontend Boundaries
* **Job Spec Requirement**: Security compliance, enterprise governance.
* **Difficulty**: Fundamentals / Critical | **Current Status**: 🟢 **KNOW**
* **Question**: *"Can you ever store an OpenAI, Anthropic, or LLM API key inside React environment variables (`VITE_OPENAI_KEY` or `REACT_APP_KEY`)? What is the attack vector?"*
* **Core Technical Mechanism**: Client-side environment variables are bundled directly into the compiled JavaScript files sent to the browser. Anyone opening DevTools can extract the key, leading to quota theft and data compromise. All AI calls must terminate at an authenticated backend API gateway.

### Q6.3: UI State Machine for Generative AI (Thinking → Streaming → Done → Error)
* **Job Spec Requirement**: Conversational UI, intelligent UX.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Chat state handling in `science-of-our-world/src/components/common/AIAssistant.tsx`.
* **Question**: *"What discrete states must an AI chat or summary interface represent, and how do you handle user cancellation during active generation?"*
* **Core Technical Mechanism**: Finite State Machine: `IDLE → SUBMITTING → THINKING (Reasoning/TTFB) → STREAMING (Tokens accumulating) → COMPLETED | CANCELLED | ERRORED`. Cancellation uses `AbortController.abort()`.

### Q6.4: Handling Malformed Structured LLM Output (JSON Mode)
* **Job Spec Requirement**: AI-driven workflows, data validation.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Your LLM is instructed to output a JSON debt settlement proposal (`{ discountedAmount: number, instalments: number }`), but the model occasionally returns markdown wrapping (```json ... ```) or truncated JSON. How do you defend the frontend from crashing?"*
* **Core Technical Mechanism**: Schema validation using `Zod`. Robust JSON extraction regex, parsing fallbacks, and rendering structured error states with a 'Regenerate' option rather than `JSON.parse` unhandled exceptions.

### Q6.5: Dealing with AI Hallucinations in Financial Workflows
* **Job Spec Requirement**: Transactional truth, financial compliance.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"An AI assistant tells a debtor: 'Nutun has forgiven 50% of your debt.' However, company policy allows only 15%. From a frontend architecture perspective, how do you prevent the UI from presenting hallucinated financial commitments as truth?"*
* **Core Technical Mechanism**: Principle of Transactional Truth: AI outputs are treated as **untrusted user input / suggestions**, never authoritative commands. Financial actions require human-in-the-loop validation, schema-enforced tool calling, and strict backend authorization gates.

### Q6.6: Handling Rapid Consecutive Prompts (User Spamming Submit)
* **Job Spec Requirement**: Conversational UI resilience, race condition mitigation.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"An agent clicks 'Generate Summary' three times in 2 seconds. How do you prevent three concurrent LLM streams from fighting for the chat window?"*
* **Core Technical Mechanism**: Disable submit button upon submission (`isGenerating`); abort previous in-flight stream via `AbortController` before initiating a new one; assign unique `turnId` to each request.

### Q6.7: Intelligent Fallbacks When LLM Latency Spikes (Degraded Mode)
* **Job Spec Requirement**: High availability, SLA compliance.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"The LLM provider's API latency jumps from 800ms to 25 seconds during peak hours. How should the frontend handle this without stranding 10,000 agents?"*
* **Core Technical Mechanism**: Client-side timeout triggers (e.g. after 8s); graceful fallback to deterministic rule-based suggestions; explicit status indicators informing agents of degraded AI assistance with manual override controls.

### Q6.8: System Prompt vs. User Prompt Separation on the Frontend
* **Job Spec Requirement**: AI safety, prompt injection defense.
* **Difficulty**: Intermediate | **Current Status**: 🔴 **NOT YET**
* **Question**: *"Why should the frontend never be responsible for constructing the system prompt containing business logic and compliance rules?"*
* **Core Technical Mechanism**: Prompt injection vulnerability. If the frontend constructs the system prompt, malicious users can intercept network requests and alter instructions to bypass compliance boundaries. System prompts belong strictly on the server.

---

## Category 7: Streaming & Token-Level UI Architecture

### Q7.1: SSE vs. WebSockets vs. Chunked Transfer HTTP
* **Job Spec Requirement**: Streaming responses & token-level UI.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"Compare Server-Sent Events (SSE), WebSockets, and standard HTTP chunked transfer for streaming LLM tokens to a browser. Why is SSE or chunked fetch generally preferred for LLMs?"*
* **Core Technical Mechanism**: LLM token generation is **unidirectional** (server to client). SSE/Chunked HTTP operates over standard HTTP/2, supports connection multiplexing, traverses corporate firewalls effortlessly, and has built-in reconnection semantics, avoiding the stateful infrastructure overhead of full-duplex WebSockets.

### Q7.2: Reading a `ReadableStream` in the Browser
* **Job Spec Requirement**: Token streaming consumption.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Stream reader in `science-of-our-world/src/services/aiService.ts`.
* **Question**: *"Write or explain the exact JavaScript code required to consume an HTTP streaming response using `response.body.getReader()` and `TextDecoder`."*
* **Core Technical Mechanism**:
  ```javascript
  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      const chunk = decoder.decode(value, { stream: true });
      accumulateTokens(chunk);
  }
  ```

### Q7.3: Preventing Global Re-Render Thrashing During High-Frequency Token Arrival
* **Job Spec Requirement**: High performance React with streaming AI.
* **Difficulty**: Senior / Master | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Tokens arrive from the LLM at 40 tokens per second. If you call `setText(prev => prev + token)` on every incoming chunk, React schedules 40 re-renders per second, causing keystroke lag. How do you buffer or throttle streaming UI updates to maintain 60fps?"*
* **Core Technical Mechanism**: Token buffer batching using `requestAnimationFrame` or 50ms throttling. Accumulate tokens in a mutable buffer (`useRef`), flushing to React state only on frame boundaries so React renders at most once every 16ms.

### Q7.4: Rendering Markdown & Code Blocks While Tokens Stream
* **Job Spec Requirement**: Conversational UI, streaming Markdown.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"When streaming markdown, how do you render incomplete markdown syntax (e.g. an unclosed bold tag `**some bold text` or half of a code block ````typescript`) without crashing the parser or flickering?"*
* **Core Technical Mechanism**: Streaming markdown parsers that tolerate unclosed delimiter tokens; memoizing completed paragraphs and code blocks so only the active tail paragraph is parsed on each chunk.

### Q7.5: Stream Cancellation & Socket Cleanup
* **Job Spec Requirement**: Resource management, clean teardown.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"When an agent clicks 'Stop Generating' or navigates to a new tab, what exact steps ensure the stream reader is released and the HTTP connection terminates?"*
* **Core Technical Mechanism**: Call `abortController.abort()`; in the stream loop catch `AbortError`; invoke `reader.cancel()` to release the lock on the `ReadableStream`.

### Q7.6: Distinguishing Partial Generation from Network Disconnections
* **Job Spec Requirement**: Stream reliability, error recovery.
* **Difficulty**: Senior | **Current Status**: 🔴 **NOT YET**
* **Question**: *"The connection drops midway through an AI response. How does the frontend know whether generation finished naturally or was cut off by a TCP reset? How do you inform the user?"*
* **Core Technical Mechanism**: Protocol-level end-of-stream delimiter (e.g. `data: [DONE]` in SSE) or checking the reader's `done` property. If the stream terminates without the sentinel, transition to an `INCOMPLETE_STREAM` state with a 'Resume' or 'Retry' prompt.

---

## Category 8: RAG & Vector Search UI Architecture

### Q8.1: What is RAG & Why is it Used?
* **Job Spec Requirement**: Vector search / RAG front-ends.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"What is Retrieval-Augmented Generation (RAG), and why is it preferred over fine-tuning a model or dumping all enterprise policies into the system prompt?"*
* **Core Technical Mechanism**: Prompt context window limits and token costs make full-corpus prompting impossible. Fine-tuning models is expensive, slow, and cannot guarantee real-time updates. RAG queries a vector database for the top $k$ relevant policy snippets and injects only those into the prompt context at query time.

### Q8.2: Embeddings & Vector Similarity Explained Simply
* **Job Spec Requirement**: Vector search fundamentals.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Explain what a vector embedding is to a business stakeholder, and how cosine similarity finds relevant documents."*
* **Core Technical Mechanism**: Converting unstructured text into a list of numbers (coordinates in multidimensional semantic space). Phrases with similar meanings (e.g. *"debt dispute"* and *"refusal to pay"*) have vectors pointing in nearly identical directions (high cosine similarity).

### Q8.3: Displaying Citations, Grounded Snippets & Confidence Scores
* **Job Spec Requirement**: Intelligent UX, grounded answer presentation.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Code Evidence**: Knowledge retrieval in `Global_Trade_Atlas/packages/knowledge`.
* **Question**: *"How should the frontend render AI-generated policy answers alongside their retrieved source citations so agents can audit the answer before speaking to a customer?"*
* **Core Technical Mechanism**: Structured response containing the answer text plus a `citations: Array<{ id, documentTitle, section, similarityScore, excerpt }>` array. Rendering interactive inline pill tags `[1]` that open an excerpt preview drawer on hover/click.

### Q8.4: Handling Irrelevant Retrieval & Hallucination Suppression
* **Job Spec Requirement**: Transactional compliance, avoiding false information.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What happens when vector retrieval returns documents with very low similarity scores (<0.65)? How should the frontend handle this rather than letting the LLM invent a policy?"*
* **Core Technical Mechanism**: Backend thresholding returns an empty retrieval set or explicit `NO_RELEVANT_POLICIES` flag. Frontend renders: *"No official Nutun policy matches this query. Please consult your team lead,"* suppressing ungrounded generation entirely.

### Q8.5: Document Security & Tenant Isolation in RAG UI
* **Job Spec Requirement**: Enterprise financial data security, multi-tenant boundaries.
* **Difficulty**: Senior / Critical | **Current Status**: 🔴 **NOT YET**
* **Question**: *"If Bank A and Bank B both outsource debt collection to Nutun, how do you prevent an AI summary for a Bank A customer from citing Bank B's private debt recovery guidelines?"*
* **Core Technical Mechanism**: Role-Based Access Control (RBAC) and tenant metadata filtering. Retrieval queries MUST include strict metadata filters (`tenant_id == customer.tenant_id`) enforced at the backend vector query level, never relying on the LLM to self-censor.

### Q8.6: Hybrid Search: Keyword (BM25) + Vector (Dense) Search
* **Job Spec Requirement**: Information retrieval accuracy.
* **Difficulty**: Senior | **Current Status**: 🔴 **NOT YET**
* **Question**: *"Why is pure vector semantic search often insufficient for financial customer lookups, and why is hybrid search (BM25 + Dense Vectors) required?"*
* **Core Technical Mechanism**: Vector embeddings excel at semantic concepts (*"can't afford to pay"*), but perform poorly on exact alphanumeric identifiers (e.g. South African ID numbers, account codes `ACC-99214`). Hybrid search combines exact keyword matching (BM25) with semantic embeddings.

---

## Category 9: Accessibility (WCAG 2.1 AA) in Production

### Q9.1: The 4 Core Principles of WCAG (POUR)
* **Job Spec Requirement**: Accessibility adherence, WCAG 2.1 AA.
* **Difficulty**: Fundamentals | **Current Status**: 🟢 **KNOW**
* **Question**: *"What are the 4 fundamental principles of WCAG (POUR), and give one practical frontend engineering example of each."*
* **Core Technical Mechanism**: **Perceivable** (color contrast, text alternatives), **Operable** (keyboard navigation, no keyboard traps), **Understandable** (predictable UI, clear error messages), **Robust** (compatible with modern and legacy assistive technologies via semantic markup).

### Q9.2: Accessible Live Chat & Streaming AI for Screen Readers
* **Job Spec Requirement**: Conversational UI, accessibility.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"You are building an AI chatbot for contact centre agents. New tokens are streaming in, and a screen-reader user is currently typing a question. How do you design the accessibility announcements so the user isn't interrupted while typing?"*
* **Core Technical Mechanism**: Do NOT announce every token in `aria-live`. Set `aria-live="polite"` on a dedicated status element that announces only milestone events: *"AI is thinking..."* and *"Response completed"*, allowing the user to press a shortcut to read the full message when ready.

### Q9.3: Building a Bulletproof Accessible Focus Trap
* **Job Spec Requirement**: Accessible interactive modals.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What makes a keyboard focus trap accessible, and what critical mistakes lead to keyboard trapping bugs?"*
* **Core Technical Mechanism**: Query focusable elements (`button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])`). Listen for `KeyDown`: if `Tab` on last element, wrap to first; if `Shift+Tab` on first, wrap to last. Trap must release on `Escape` and restore focus to trigger button.

### Q9.4: Color Contrast Ratios (WCAG AA Standards)
* **Job Spec Requirement**: Accessible UI styling.
* **Difficulty**: Fundamentals | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What are the minimum color contrast ratios required by WCAG 2.1 AA for normal text, large text, and interactive UI components?"*
* **Core Technical Mechanism**: Normal text (<18pt / 24px regular): **4.5:1**. Large text (>=18pt or >=14pt bold): **3:1**. UI components and graphical objects (borders, focus indicators): **3:1**.

### Q9.5: Accessible Error Handling in Complex Forms
* **Job Spec Requirement**: Accessible forms, transactional compliance.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"When a user submits an invalid form, how should validation errors be associated with inputs so screen readers announce them automatically when focused?"*
* **Core Technical Mechanism**: `aria-invalid="true"`; `aria-describedby="error-message-id"`; rendering an `id="error-message-id"` containing the specific plain-language guidance. Focus moved programmatically to the first invalid field or error summary.

### Q9.6: Automated vs. Manual Accessibility Testing
* **Job Spec Requirement**: Quality assurance, accessibility testing.
* **Difficulty**: Senior | **Current Status**: 🔴 **NOT YET**
* **Question**: *"Your automated CI pipeline runs `axe-core` and reports 0 accessibility violations. Can you guarantee the application meets WCAG 2.1 AA compliance? Why or why not?"*
* **Core Technical Mechanism**: Automated tools detect only rule-based syntax issues (missing alt tags, invalid ARIA, contrast). They cannot test logical keyboard tab order, screen reader pronunciation, meaningful alt text descriptions, or cognitive flow. Manual keyboard and screen-reader (NVDA/VoiceOver) testing is mandatory.

---

## Category 10: High Performance & Scalability

### Q10.1: DOM Virtualization Mechanics (List Windowing)
* **Job Spec Requirement**: High performance, scalability (10k concurrent agents).
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"Explain how list virtualization works. If a contact centre agent opens a table with 5,000 customer accounts, how does virtualization keep rendering at 60fps?"*
* **Core Technical Mechanism**: The Receptionist Desk model. Only the visible slice (e.g. 20 rows) plus an overscan buffer are rendered in the DOM ($O(1)$ host nodes). Absolute positioning coordinates update on scroll. The underlying JavaScript array remains $O(N)$ in memory.

### Q10.2: Diagnosing Layout Thrashing (Forced Synchronous Layout)
* **Job Spec Requirement**: Performance diagnostics, smooth UI.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"What is Layout Thrashing, and what JavaScript code patterns cause the browser to repeatedly recalculate geometry in a single frame?"*
* **Core Technical Mechanism**: Interleaving DOM reads (`element.offsetHeight`, `element.getBoundingClientRect()`) with DOM writes (`element.style.width = ...`). The browser is forced to flush pending style changes and execute synchronous layout before each read.

### Q10.3: Code Splitting & Route-Based Lazy Loading
* **Job Spec Requirement**: Scalability, fast initial bundle loading.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Vite code splitting in `axis_clean`.
* **Question**: *"How do you implement code splitting in React using `React.lazy` and `Suspense`? What should you consider when deciding where to split chunks?"*
* **Core Technical Mechanism**: Dynamic `import()` statements create separate bundle chunks downloaded on-demand. Split along heavy route boundaries or behind infrequently used complex modals (e.g. PDF export, chart visualizers), avoiding over-fragmentation.

### Q10.4: Web Worker Offloading for Heavy Calculations
* **Job Spec Requirement**: Main-thread responsiveness.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"An agent imports a 20,000-row CSV of customer repayments. Sorting and filtering this dataset in the main thread takes 400ms, causing typing lag. How would you offload this calculation using a Web Worker?"*
* **Core Technical Mechanism**: Web Workers run on a background OS thread. Data is passed via `postMessage` (structured cloning or Transferable Objects like `ArrayBuffer`). Main thread UI remains 60fps and completely responsive while the worker computes.

### Q10.5: React Profiler Flamegraphs & Interaction Tracing
* **Job Spec Requirement**: Performance measurement and diagnosis.
* **Difficulty**: Senior | **Current Status**: 🟢 **KNOW**
* **Question**: *"How do you diagnose why a specific button click feels sluggish using the React Developer Tools Profiler?"*
* **Core Technical Mechanism**: Record interaction → Flamegraph view → Identify tall/wide bars → Inspect 'Render Reason' (*"Props changed"*, *"Hook changed"*, *"Parent rendered"*) → Check component self-render time vs subtree time.

### Q10.6: Bundle Size Optimization & Tree Shaking
* **Job Spec Requirement**: Fast load times, lightweight bundles.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"How do you analyze an enterprise React bundle to identify bloated dependencies, and what makes a package tree-shakeable?"*
* **Core Technical Mechanism**: Bundle analyzers (`rollup-plugin-visualizer` or `@next/bundle-analyzer`). Tree shaking requires ES Module syntax (`import`/`export`) and `sideEffects: false` in `package.json` to allow compilers to drop unused exports.

---

## Category 11: Automated Testing Discipline & Quality Assurance

### Q11.1: The Testing Trophy & What to Test
* **Job Spec Requirement**: Automated testing, reusable component testing.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Vitest and React Testing Library setup in `axis_clean/apps/web`.
* **Question**: *"Explain Kent C. Dodds' 'Testing Trophy' (Static → Unit → Integration → E2E). Where should the bulk of frontend test effort be spent, and why?"*
* **Core Technical Mechanism**: Bulk in **Integration tests**. Testing isolated units in mocks gives false confidence; testing full E2E is slow and brittle. Integration tests verify components working together with realistic user events.

### Q11.2: React Testing Library: Testing Behavior vs. Implementation Details
* **Job Spec Requirement**: Robust testing standards.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Why does React Testing Library discourage testing component internal state or checking whether `useMemo` was called? How does testing behavior prevent brittle tests?"*
* **Core Technical Mechanism**: *"The more your tests resemble the way your software is used, the more confidence they can give you."* Query by accessible role (`getByRole('button', { name: /submit/i })`) and text rather than class names or internal state variables. Refactoring implementation won't break the test if behavior remains identical.

### Q11.3: Testing Asynchronous UI with Mock Service Worker (MSW)
* **Job Spec Requirement**: API mocking, resilient tests.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Why is Mock Service Worker (MSW) superior to mocking `global.fetch` directly in Vitest/Jest?"*
* **Core Technical Mechanism**: MSW intercepts requests at the network level (Service Worker in browser, interceptor in Node). The application executes real `fetch` calls, real headers, and real serialization. No internal mock leaks into production code.

### Q11.4: Testing Nondeterministic AI & LLM Streaming UIs
* **Job Spec Requirement**: Testing AI/LLM web applications.
* **Difficulty**: Senior / Master | **Current Status**: 🔴 **NOT YET**
* **Question**: *"How do you write reliable automated tests for an AI streaming chat component when the LLM's response text and token timing are nondeterministic?"*
* **Core Technical Mechanism**: Mock the backend streaming endpoint using MSW with a simulated chunked `ReadableStream`. Test state transitions: verifying `thinking` skeleton appears, chunks append to DOM incrementally, abort button cancels the stream, and completion removes streaming indicators.

### Q11.5: Testing Asynchronous Race Conditions in Search
* **Job Spec Requirement**: Async testing, bug prevention.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"Write a conceptual test verifying that if Request 1 resolves after Request 2, the UI preserves Request 2's data."*
* **Core Technical Mechanism**: Use MSW with delayed network responses. Trigger keystroke 'A' (delay 500ms), then keystroke 'B' (delay 50ms). Fast-forward timers or wait for promises; assert UI displays results for 'B', and advancing 500ms does NOT overwrite with 'A'.

### Q11.6: End-to-End (E2E) Testing with Playwright
* **Job Spec Requirement**: End-to-end quality validation.
* **Difficulty**: Intermediate | **Current Status**: 🔴 **NOT YET**
* **Question**: *"What critical user journeys belong in Playwright E2E tests versus integration tests in a financial debt management application?"*
* **Core Technical Mechanism**: Smoke tests and mission-critical transactional paths: Agent login → Open customer case → Submit payment arrangement → Verify confirmation and backend webhook reception.

---

## Category 12: Wireframe → Production Ownership

### Q12.1: The Figma to Production Component Pipeline
* **Job Spec Requirement**: Translating wireframes/Figma into production code.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Code Evidence**: Production dashboard components in `axis_clean`.
* **Question**: *"Walk me through your exact process when a UI/UX designer hands you a new Figma wireframe for a complex customer arrangement dashboard."*
* **Core Technical Mechanism**: Decompose layout into component hierarchy → Identify design tokens (colors, spacing, typography) → Define component interfaces & props API → Establish states (Loading, Empty, Error, Partial, Success) → Code semantic HTML structure → Apply responsive breakpoints → Implement keyboard/screen-reader accessibility → Write integration tests.

### Q12.2: The 5 Essential UI States of Every Production Component
* **Job Spec Requirement**: Intelligent UX, resilient component design.
* **Difficulty**: Intermediate | **Current Status**: 🟢 **KNOW**
* **Question**: *"What are the 5 states every production component that fetches data must explicitly handle, and why do junior developers often forget 3 of them?"*
* **Core Technical Mechanism**:
  1. **Loading** (skeleton/spinner)
  2. **Empty** (no records found, with clear next action)
  3. **Error** (actionable retry, plain language)
  4. **Success / Populated** (standard view)
  5. **Stale / Revalidating** (background refresh indicator)

### Q12.3: Building a Flexible, Accessible Design System Component API
* **Job Spec Requirement**: Reusable components, design systems.
* **Difficulty**: Senior | **Current Status**: 🟡 **UNDERSTAND**
* **Code Evidence**: UI primitives in `axis_clean/packages/ui`.
* **Question**: *"Design the props API for a reusable `<Button>` and `<Card>` component in TypeScript. How do you balance flexibility (allowing native button attributes) with strict brand constraints?"*
* **Core Technical Mechanism**: Polymorphic components using `React.ComponentPropsWithRef<'button'>`, variant props (`variant: 'primary' | 'danger' | 'ghost'`), size props, and forwardRef to ensure parent focus control.

### Q12.4: Bridging Gaps When Designers Provide Desktop-Only Wireframes
* **Job Spec Requirement**: Design-to-production ownership, collaboration.
* **Difficulty**: Intermediate | **Current Status**: 🟡 **UNDERSTAND**
* **Question**: *"A designer hands you a desktop wireframe at 1440px with 6 columns, but didn't provide mobile or tablet mockups. How do you handle this without delaying the sprint?"*
* **Core Technical Mechanism**: Collaborate proactively; apply responsive design principles (stacking columns, collapsing tables into summary cards on mobile, touch-friendly 44x44px target sizes); review responsive prototype with designer early in staging.

---

## Category 13: Cross-Cutting Systems Scenarios (The Senior Round)

### Q13.1: The Laggy AI Streaming Contact Centre Dashboard
* **Scenario**: 10,000 agents are answering calls. An AI co-pilot streams live suggestions and customer sentiment during the call. Suddenly, agents report that typing into the customer notes box lags by 300ms while the AI is generating text.
* **Difficulty**: Senior Architect | **Current Status**: 🟡 **UNDERSTAND**
* **Core Diagnosis**: Unbuffered streaming tokens trigger 40 React re-renders/sec on the root or shared parent component, locking the main thread.
* **Solution**: Move streaming state down to an isolated leaf component; buffer incoming tokens via `requestAnimationFrame` (flushing at max 60fps); decouple notes input state using unmemoized local state or `useTransition`.

### Q13.2: The RAG Financial Compliance Leak
* **Scenario**: An AI assistant is deployed to help agents negotiate debts. A customer asks: *"Can I get the same deal my neighbor got?"* The model quotes an internal high-value debt settlement figure from another debtor's private arrangement.
* **Difficulty**: Senior Architect | **Current Status**: 🟢 **KNOW**
* **Core Diagnosis**: Vector search lacked tenant metadata filtering and document authorization boundaries; relying on the LLM to understand privacy.
* **Solution**: Strict backend metadata filtering (`tenant_id`, `account_id`, `agent_clearance_level`); prompt boundary hardening; masking PII before document embedding.

### Q13.3: The 50,000 Record Memory Collapse
* **Scenario**: An agent loads a campaign of 50,000 delinquent accounts. The browser crashes after scrolling for 2 minutes.
* **Difficulty**: Senior Architect | **Current Status**: 🟡 **UNDERSTAND**
* **Core Diagnosis**: Non-virtualized table created 500,000 DOM nodes; event listeners and uncleaned tooltips retained in memory.
* **Solution**: Implement DOM virtualization (`@tanstack/react-virtual`); paginate API fetches (server-side cursor pagination); profile memory heap snapshots to identify detached DOM nodes.

### Q13.4: The Double-Charge Race Condition Under Poor Connectivity
* **Scenario**: An agent is on a 3G mobile connection collecting a debt arrangement payment. They click 'Process R2,000 Payment'. The spinner spins for 8 seconds. The agent assumes it stalled and clicks again. The debtor's bank reports two charges of R2,000.
* **Difficulty**: Senior Architect | **Current Status**: 🟢 **KNOW**
* **Core Diagnosis**: Lack of frontend submit disabling and lack of client-generated idempotency keys.
* **Solution**: Immediate optimistic button disabling; client generates unique `Idempotency-Key` UUID on form mount; backend validates key against Redis transactional cache to guarantee single execution.

---

## Targeted Preparation Priority Sequence

To maximize interview performance between now and Thursday, we will drill answers and hostile follow-ups in this strict order:

```text
PRIORITY 1: High-Yield AI & Streaming Frontend (Categories 6, 7 & 8)
  → LLM state machines, token throttling, SSE vs WS, RAG citations, API security.
  → Rationale: This is Nutun's key differentiator and our highest-visibility area.

PRIORITY 2: Accessibility & Semantic HTML5 (Categories 1 & 9)
  → Form accessibility, WCAG 2.1 AA, aria-live for streaming AI, modal focus trapping.
  → Rationale: Explicitly stated in the job spec; common filter for senior roles.

PRIORITY 3: High-Performance React & Scalability (Categories 3 & 10)
  → Virtualization mechanics, re-render myths, React.memo failure modes, profiler.
  → Rationale: Directly answers "High-performance React web applications".

PRIORITY 4: Automated Testing & Asynchronous Quality (Categories 4 & 11)
  → React Testing Library, MSW, async race condition tests, streaming tests.
  → Rationale: Solidifies engineering maturity and production readiness.

PRIORITY 5: Responsive CSS3 & Wireframe-to-Production (Categories 2 & 12)
  → Flexbox vs Grid, box model, CLS prevention, 5 essential UI states.
```
