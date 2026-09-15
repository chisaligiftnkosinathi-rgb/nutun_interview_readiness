# Production MVP Build Specification: Nutun Front-End Learning & Interview Engine

> **GOVERNING PRINCIPLE**: The Learning Engine must precede the Arena.  
> Every question must link directly to a `lessonId`, an underlying foundation lesson, expected core concepts, and an explicit evaluation rubric.  
> **No orphan questions.**

---

## 1. The 4-Phase System Architecture

```text
FOUNDATION CURRICULUM (02_FOUNDATIONS)
          │
          ▼
PHASE 1: LEARNING ENGINE (Study → Concept Breakdown → Code Evidence → Checkpoint)
          │
          ▼
PHASE 2: AI TUTOR (Real-time Precision Corrections → Sequence Validation → Challenge Prompts)
          │
          ▼
PHASE 3: INTERVIEW ENGINE (Job-Spec Questions → 60s Pressure Timer → 6-Dimension Rubric)
          │
          ▼
PHASE 4: INTERVIEW ARENA (Gamified Mastery → XP → Streaks → Boss Rounds → Exit Tests)
```

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
