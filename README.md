# Nutun Front-End Engineer Interview Readiness

Comprehensive, evidence-based technical interview preparation curriculum for the **Front-End Engineer** role at **Nutun**, facilitated via OfferZen.

## Curriculum Architecture

```text
00_GOVERNANCE/
├── STUDY_CONSTITUTION.md          # 6 evaluation levels, 3 knowledge categories (KNOW/UNDERSTAND/NOT YET)
└── INTERVIEWER_MODEL.md           # Talent screen (Nozipho) vs Technical Evaluation models

01_CONTEXT/
├── NUTUN/                         # Company profile, scale (18.5M interactions, 10k agents)
├── OFFERZEN/                      # Talent partner research, hiring funnel
└── ROLE/                          # Job spec, role map, and master COMPETENCY MATRIX (Control Document)
    ├── FRONTEND_ENGINEER_JOB_SPEC.pdf
    ├── JOB_SPEC_EXTRACTED.md
    ├── JOB_ROLE_INTERVIEW_MAP.md
    └── JOB_SPEC_COMPETENCY_MATRIX.md   # Master control document linking every lesson to job requirements

02_FOUNDATIONS/
├── 00_TECHNOLOGY_ORIGINS/         # Origins → Problem → Solution → Evolution across web stack
│   ├── README.md                  # Unified architectural taxonomy (Languages, Markup, CSS, Protocols, APIs, Libraries)
│   ├── WEB_HISTORY.md             # From CERN hypertext to real-time AI contact centers
│   ├── HTML_ORIGIN.md             # Semantic documents, hyperlinking graph, accessibility tree
│   ├── HTTP_ORIGIN.md             # Stateless protocol, HTTP/1.1 -> HTTP/2 -> HTTP/3 QUIC, caching & ETags
│   ├── CSS_ORIGIN.md              # Cascade algorithm, box model, layout/paint/composite pipeline
│   ├── JAVASCRIPT_ORIGIN.md       # First-class functions, single-threaded event loop, JIT engines
│   ├── DOM_ORIGIN.md              # Object tree, event bubbling & delegation, layout thrashing
│   ├── NODEJS_ORIGIN.md           # Ryan Dahl & C10k, libuv non-blocking I/O, unified runtime
│   ├── TYPESCRIPT_ORIGIN.md       # Compile-time type erasure, structural typing, discriminated unions
│   ├── REACT_ORIGIN.md            # Facebook state explosion, declarative UI (UI = f(state)), fiber reconciliation
│   └── MODERN_WEB_EVOLUTION.md    # Hooks, TanStack Query server cache, Web Workers, SSE vs WebSockets, streaming AI
├── 01_WEB_REQUEST_LIFECYCLE/      # DNS, TLS 1.3 handshake, TCP, HTTP request/response, critical rendering path
├── 02_JAVASCRIPT_BEFORE_REACT/    # Lexical scope, closures, event loop, microtasks, async/await, AbortController, stale closures
└── 03_REACT/                      # React from first principles
    ├── LESSON.md                  # 8 distinct architectural layers, UI = f(state), Fiber work units, Render vs Commit
    ├── QUESTIONS.md               # Checkpoint questions (A through G)
    ├── ANSWERS.md                 # Normalized technical answers & precision evaluations (🟢 KNOW)
    ├── CONCEPTS.md                # 13 core concepts (Declarative UI, Fiber, Reconciliation heuristics, keys != security)
    └── EVIDENCE.md                # Codebase evidence links (science-of-our-world, axis_clean)

13_INTERVIEW_QUESTIONS/
├── 01_RECRUITER/
│   └── TALENT_SCREEN_QA.md        # Conversational narrative screen Q&A
└── 03_REACT/
    └── 01_WHY_REACT_EXISTS.md     # 60–90s pitch, follow-up defenses, technical traps to avoid
```

## Governing Knowledge Categories
* 🟢 **I KNOW**: Direct, verifiable evidence from production code and hands-on implementations.
* 🟡 **I UNDERSTAND**: Technically sound architectural understanding, but not personally built yet.
* 🔴 **I DON'T KNOW YET**: Identified gaps under targeted study.
