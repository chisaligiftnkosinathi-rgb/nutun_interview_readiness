# Production MVP Build Specification: Nutun Front-End Learning & Interview Engine

> **GOVERNING PRINCIPLE**: The Learning Engine must precede the Arena.  
> Every question must link directly to a `lessonId`, an underlying foundation lesson, expected core concepts, and an explicit evaluation rubric.  
> **No orphan questions.**

---

## 1. The 10-Layer Pedagogical System Architecture

The engine uses a 10-layer progression from intuitive real-world reality to hostile technical defense:

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

> **Principle**: Analogy provides the mental scaffold; technical mechanism provides the engineering rigor. Never present a question without first grounding the concept in an operational scenario.

---

## 2. Slice A Vertical Scope: Web Request Lifecycle

The primary vertical slice implements the complete flow for **Module 01: Web Request Lifecycle**:

```text
Step 1: Study Lesson
        - Topic: How a browser reaches a server (URL → DNS → TCP/TLS 1.3 → HTTP → Server → DOM/CSSOM → Layout → Paint)
        - Key Mechanism: Why DNS caching matters, TCP vs QUIC, parser-blocking JS vs render-blocking CSS.
        ↓
Step 2: Interactive Checkpoint
        - Checkpoint Prompt: "What happens before an HTTP request can reach the application server?"
        - User responds.
        - AI Tutor evaluates conceptual sequence, issues precision corrections (e.g. "DNS resolves IP; connection precedes HTTP").
        ↓
Step 3: Interview Question (Timed Pressure)
        - Question: "Explain TTFB (Time to First Byte) to me: what constitutes it, and how do you diagnose whether high TTFB is caused by the network, DNS, TLS, or backend application execution?"
        - 60-Second Pressure Clock.
        ↓
Step 4: AI Evaluation & Precision Correction
        - Rubric breakdown (Technical Accuracy /25, Mechanism /20, Job Terminology /15, Trade-offs /20, Clarity /20).
        - Highlights technical precision (e.g., separating server processing time from network round-trip transit).
        ↓
Step 5: Senior Follow-Up Trap
        - "If TTFB is 2,500ms, but backend APM shows database query execution took only 15ms, what network and infrastructure layers would you investigate first?"
        ↓
Step 6: Progress & Mastery Tracking
        - Awards structured XP.
        - Updates Readiness Status in repository.
```

---

## 3. Application Directory Structure

```text
apps/interview_arena/
├── README.md                      # Architecture & classification disclaimer
├── index.html                     # Unified reactive SPA interface (Study + Checkpoint + Interview + Arena)
├── src/
│   ├── content/                   # Modular foundation lessons & rubrics
│   │   ├── web_request_lifecycle.js
│   │   └── react_performance.js
│   ├── engine/                    # Evaluation & scoring heuristics
│   │   ├── evaluator.js           # Sequence and terminology checking
│   │   └── progress.js            # XP and Exit Test state management
│   └── ui/                        # Reusable component templates
└── tests/                         # Unit tests for evaluation rubrics
```

---

## 4. Frontend Engineering Guarantees
1. **Network Boundary & State Validity**: Prevents stale AI responses when switching tabs or retrying answers.
2. **Accessible Interaction**: Semantic HTML5 forms, ARIA live status regions (`aria-live="polite"`), and keyboard navigation (`Tab`, `Enter`, `Escape`).
3. **Resilient UX**: Graceful handling of empty submissions, timer expiration, and client-side evaluation fallback.
