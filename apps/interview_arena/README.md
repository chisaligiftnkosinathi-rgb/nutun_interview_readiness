# Nutun Front-End Engineer — Interview Arena (Simulation App)

> **CLASSIFICATION**: `INTERVIEW PREPARATION SIMULATION`  
> **DISCLAIMER**: This application is a self-testing tool built strictly for interview readiness and exit testing. It is **NOT** internal Nutun software.

---

## 1. Product Overview
The **Nutun Front-End Engineer — Interview Arena** is a focused interactive interview rehearsal game built directly on the **Nutun Job Specification Competency Matrix** (`01_CONTEXT/ROLE/JOB_SPEC_COMPETENCY_MATRIX.md`).

Instead of passive flashcards or multiple-choice quizzes, the Arena acts as an **AI Technical Evaluator**:
1. It presents realistic interview prompts derived directly from the job spec.
2. It enforces a strict **timed pressure window** (30s / 60s / 90s).
3. The candidate inputs their technical answer.
4. The evaluator grades across 6 dimensions (Technical Accuracy, Underlying Mechanism, Job-Spec Terminology, Trade-Off Awareness, Evidence Discipline, and Clarity).
5. It surfaces **Precision Corrections** (identifying common interview misconceptions like *"props changing independently triggers a re-render"*), awards structured XP, and immediately generates a **hostile technical follow-up / trap** to test candidate resilience.

---

## 2. Vertical Slice #1 Scope: High-Performance React Web Applications
The first vertical slice tests **Requirement #1**:
* **Question 1**: React Re-Render Triggers & The Props Myth
* **Question 2**: `React.memo` Failure Modes & Shallow Equality
* **Question 3**: Large Debtor Tables (5,000 records) & DOM Virtualization
* **Question 4**: Component Composition (`children`) vs Memoization

---

## 3. Architecture & Mechanics
* **Frontend**: Pure reactive UI simulation using semantic HTML, accessible keyboard navigation (`Enter` to submit, `Esc` to reset, accessible ARIA live regions), reactive state management, countdown timer with visual pressure warning, and offline-capable heuristic evaluation engine.
* **Resilience Mechanisms Tested**:
  * AbortController simulation & request cancellation on question switch.
  * Stale AI response prevention (ignoring evaluations from previous turns).
  * Accessible form validation and keyboard focus trapping.
  * Score and XP persistence with clear gap analysis.
