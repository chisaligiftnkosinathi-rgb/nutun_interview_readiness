# Nutun Front-End Engineer Readiness — Study Constitution

## 1. The Governing Purpose
This course prepares for real-world engineering examinations and technical evaluations for the **Front-End Engineer** role at Nutun, facilitated via OfferZen.

We do not memorize interview clichés. We learn:
```text
What is happening?
      ↓
Why is it happening?
      ↓
How is it implemented?
      ↓
What can go wrong?
      ↓
How would an engineer improve it?
      ↓
How does that matter to Nutun?
```

### The 10-Layer Pedagogical Pipeline
Analogy provides the mental scaffold; technical mechanism provides the engineering rigor. We never start with abstract terminology; we build understanding progressively:

```text
1. REAL-WORLD SCENARIO      (Contact centre, telephone queue, company hierarchy)
        ↓
2. INTUITIVE MENTAL MODEL   (The CEO & department tree, the receptionist desk)
        ↓
3. INTERACTIVE CHALLENGE    (What happens when an agent types while 5,000 records update?)
        ↓
4. TECHNICAL TRANSLATION    (Fiber nodes: child, sibling, return; work loop)
        ↓
5. CODE MECHANISM           (while (workInProgress !== null && !shouldYield()))
        ↓
6. COMMON MISCONCEPTION     ("Is commit phase also interruptible?")
        ↓
7. CHECKPOINT               (Sequence & mechanic verification)
        ↓
8. TIMED INTERVIEW DEFENSE  (60-second pressure response)
        ↓
9. HOSTILE FOLLOW-UP / TRAP (The edge cases that trip up mid-level engineers)
        ↓
10. INTERVIEW READY 🛡️      (Demonstrated spoken mastery)
```

> **The Golden Rule of Analogies**: The analogy (e.g. "CEO and department tree") is the teaching scaffold. But the candidate must always translate the scaffold into authoritative engineering reality (*"Fiber nodes linked via child, sibling, and return pointers allocated on the JavaScript heap driven by an interruptible work loop"*). Never answer an interview question with just an analogy.

---

## 2. The Four Competency Status Levels

* 🔴 **NOT YET**: Material has not been adequately learned, verified, or demonstrated.
* 🟡 **UNDERSTAND**: The candidate can explain the concept theoretically, but lacks sufficient implementation, debugging, codebase evidence, or defense depth under follow-up questioning.
* 🟢 **KNOW**: The candidate can explain the mechanical reality, connect it to verified codebase evidence in `C:\Projects`, and answer standard engineering interview questions.
* 🛡️ **INTERVIEW READY**: The candidate has passed the defined, observable **exit test**—including rapid-fire questions, technical follow-up traps, and timed verbal responses under pressure without looking at notes.

> **Absolute Rule**: A well-written lesson or accurate answer document alone must **NEVER** promote a competency to 🛡️ INTERVIEW READY. The status requires an actual demonstrated exit test.
> **Evidence Classification**:
> * `DOCUMENTED` — Explicitly cited in external/internal specs.
> * `OBSERVED` — Directly verified in existing codebase files.
> * `INFERRED` — Reasonable technical deduction, explicitly labeled.
> * `SIMULATION` — Interview preparation code only; never represented as Nutun internal architecture.
> * `UNKNOWN` — Not yet verified.

---

## 3. The 6 Levels of Evaluation
1. **Level 1 — Fundamentals**: HTML, CSS, JavaScript, HTTP, Browser, DOM.
2. **Level 2 — React**: Components, Props, State, Hooks, Effects, Context, Rendering, Reconciliation, Performance.
3. **Level 3 — Production Frontend**: TypeScript, APIs, Async State, Caching, Errors, Testing, Accessibility, Security.
4. **Level 4 — AI Frontend**: LLM APIs, Prompts, Streaming, SSE/Fetch Streams, Conversational UI, RAG, Citations, Latency, Fallbacks.
5. **Level 5 — Nutun Operational Context**: Workflows, Financial/Transactional Truth, High-Volume Systems (18.5M monthly interactions, up to 10k agents), AI + Human workflows.
6. **Level 6 — Engineering Judgement**: *"Why did you choose that? What happens if it fails? What are the trade-offs?"*
