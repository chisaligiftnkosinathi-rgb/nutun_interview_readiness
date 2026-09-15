# Nutun Job Specification: Competency Matrix & Curriculum Control Document

## 1. The Governing Purpose
This matrix is the **governing control document** for the entire interview preparation curriculum. 
Every concept we study in React, JavaScript, CSS, TypeScript, APIs, Testing, and AI is anchored to a specific requirement from the Nutun Front-End Engineer job specification.

We do not study React generically. We study what Nutun requires to build, optimize, and maintain high-performance enterprise web applications integrated with AI and financial workflows.

---

## 2. Evaluation Status Definitions
* 🟢 **I KNOW**: Direct, verifiable evidence from production code, monorepos, or hands-on implementations in `C:\Projects`. Can explain mechanics, edge cases, failure modes, and code trade-offs.
* 🟡 **I UNDERSTAND**: Technically sound architectural understanding, but requires stronger implementation evidence, explanation practice, or defense under follow-up questions.
* 🔴 **I DON'T KNOW YET**: Identified gaps that are explicitly required or valuable for the role, scheduled for targeted learning.
* ⚠️ **CLAIM BOUNDARY**: What we must strictly not claim unless backed by verifiable evidence. We never fabricate years of professional employment; we present demonstrable code evidence and deep technical competence truthfully.
* 🛡️ **INTERVIEW-READY**: A competency is ONLY marked interview-ready when the candidate can explain the mechanism, show real code evidence, build/debug it, and survive hostile follow-up questions under time pressure.

---

## 3. The 9-Layer Lesson Evaluation Rubric
Every curriculum module and competency walkthrough must explicitly satisfy these 9 layers:

| Layer | Question / Standard |
| :--- | :--- |
| **1. Job requirement** | Why is Nutun asking for this? |
| **2. Mechanism** | How does it actually work under the hood? |
| **3. Evidence** | Where have we actually implemented or observed it in `C:\Projects`? |
| **4. Practice** | Can we build or debug it live? |
| **5. Interview** | Can we explain it clearly in 60–90 seconds? |
| **6. Defense** | Can we survive senior engineering follow-ups and traps? |
| **7. Claim boundary** | What must we strictly NOT claim? |
| **8. Status** | KNOW / UNDERSTAND / NOT YET |
| **9. Exit test** | What concrete proof confirms we are ready to advance? |

---

## 4. Master Competency Matrix

| # | Job Spec Requirement | Underlying Technical Knowledge | Curriculum Module | Existing Code Evidence | Status | Claim Boundary |
| :---: | :--- | :--- | :--- | :--- | :---: | :--- |
| **1** | **High-performance React Web Apps** | JS runtime, Event Loop, Virtual DOM / Fiber, Render vs Commit, Reconciliation, memoization (`useMemo`, `useCallback`, `React.memo`), profiler | `02_FOUNDATIONS/03_REACT` & `08_PERFORMANCE` | [`axis_clean/apps/web`](file:///c:/Projects/axis_clean/apps/web), [`science-of-our-world`](file:///c:/Projects/science-of-our-world) | 🟡 UNDERSTAND (Needs defense rehearsal) | Do not claim 7 commercial years; demonstrate deep architecture & production monorepos. |
| **2** | **Semantic HTML5** | Semantic tags (`main`, `nav`, `article`), document outline, forms, accessibility tree, ARIA attributes (`aria-expanded`, `aria-live`) | `00_TECHNOLOGY_ORIGINS/HTML_ORIGIN.md` & `05_HTML_CSS` | Form and layout structures in `axis_clean`, `7MATA` | 🟢 KNOW | Knowledge target is WCAG 2.1 AA; do not claim certified auditor status. |
| **3** | **Modern Responsive CSS3** | Box model, Flexbox, CSS Grid, media queries, CSS variables, container queries, render pipeline (reflow, repaint, composite) | `00_TECHNOLOGY_ORIGINS/CSS_ORIGIN.md` & `05_HTML_CSS` | Responsive dashboards in `axis_clean`, `science-of-our-world` | 🟢 KNOW | Focus on modern CSS Grid/Flexbox and GPU compositor performance (`transform`, `opacity`); do not claim legacy float hacks. |
| **4** | **AI/LLM Web Integration** | HTTP streaming, chunked transfer encoding, `fetch` ReadableStream reader, Async Iterables, prompt structure, token parsing, latency mitigation | `00_TECHNOLOGY_ORIGINS/MODERN_WEB_EVOLUTION.md` & `09_AI_LLM` | [`science-of-our-world/src/services/aiService.ts`](file:///c:/Projects/science-of-our-world/src/services/aiService.ts#L53-L67) | 🟢 KNOW | Frontend integration of LLM streaming endpoints and fallback UX; do not claim training or fine-tuning foundation models. |
| **5** | **Conversational UI & Chatbots** | Message normalization, role modeling (`user`, `assistant`, `system`), auto-scroll heuristics, optimistic turns, error recovery, input locking | `09_AI_LLM` & `03_REACT` | [`science-of-our-world/src/components/common/AIAssistant.tsx`](file:///c:/Projects/science-of-our-world/src/components/common/AIAssistant.tsx#L51-L82) | 🟢 KNOW | Stateful chat engineering; do not claim conversational UX designer title. |
| **6** | **AI-Driven Workflows** | State machines, multi-step agent flows, human-in-the-loop approvals, asynchronous task status polling, failure recovery | `09_AI_LLM` & `11_SYSTEM_DESIGN` | Agent simulation pipelines in `phanda`, `7MATA` | 🟡 UNDERSTAND | Any Nutun debt arrangement workflow built is an *interview preparation simulation*, not internal Nutun architecture. |
| **7** | **REST / Web APIs & Microservices** | HTTP methods, status codes, headers, CORS, TLS 1.3, serialization, error boundaries, request deduplication, optimistic concurrency (ETags) | `01_WEB_REQUEST_LIFECYCLE` & `06_APIS` | Multiple API integrations across `axis_clean`, `science-of-our-world` | 🟢 KNOW | Frontend consumption, contract enforcement, and resilient error recovery; do not claim backend microservices authorship. |
| **8** | **Intelligent UX (Async State & Caching)** | Debouncing, throttling, race conditions, stale closures, query keys, stale-while-revalidate, TanStack Query | `02_JAVASCRIPT_BEFORE_REACT` & `06_APIS` | Async search and data fetching in `axis_clean` | 🟢 KNOW | Articulate query-key identity and stale-result protection; do not claim TanStack Query makes application-level authorization unnecessary. |
| **9** | **Wireframe → Production Implementation** | Translating Figma/wireframes into accessible components, design tokens, layout fidelity, responsive breakpoints | `05_HTML_CSS` & `03_REACT` | Clean dashboard and interactive simulations in `axis_clean` | 🟢 KNOW | Precision engineering implementation of design specifications; do not claim visual graphic designer role. |
| **10** | **Scalability & Performance** | Code splitting (`React.lazy`), bundle analysis, virtualization for large lists, memory leak profiling, layout thrashing prevention | `08_PERFORMANCE` & `03_REACT` | Performance optimization in high-volume canvas simulations in `science-of-our-world` | 🟡 UNDERSTAND | Understand DOM virtualization; need hands-on virtualized 5,000-row contact center table to reach 🟢. |
| **11** | **Accessibility (WCAG 2.1 AA)** | Keyboard navigation, focus management, ARIA roles/states, screen-reader testing, contrast ratios, accessible forms | `05_HTML_CSS` | Accessible form structures and focus traps in `axis_clean` | 🟡 UNDERSTAND | Understand WCAG criteria; treat "adherence to WCAG 2.1 AA" as a knowledge target requiring explicit audit test logs before claiming 🟢. |
| **12** | **Reusable Components & Design Systems** | Component composition (`children`), polymorphic components, props API design, styling encapsulation, shared packages | `03_REACT` & `05_HTML_CSS` | Shared UI primitives in `axis_clean/packages/ui` | 🟢 KNOW | Building clean internal UI component libraries; do not claim authoring public design systems. |
| **13** | **Automated Testing Discipline** | Unit testing, React Testing Library, User Event, Mock Service Worker (MSW), integration tests, E2E (Playwright) | `07_TESTING` | Vitest and React Testing Library setup in `axis_clean/apps/web` | 🟡 UNDERSTAND | Understand test boundaries; need a dedicated suite of async payment edge case tests to reach 🟢. |
| **14** | **Streaming Responses & Token-Level UI** *(Nice-to-Have)* | `ReadableStream`, `TextDecoder`, Server-Sent Events (`EventSource`), incremental state updates, buffer accumulation | `00_TECHNOLOGY_ORIGINS` & `09_AI_LLM` | [`science-of-our-world/src/services/aiService.ts`](file:///c:/Projects/science-of-our-world/src/services/aiService.ts) | 🟢 KNOW | Practical stream consumption; treat advanced socket backpressure as a knowledge target, avoiding claims of low-level C++ network driver engineering. |
| **15** | **Vector Search / RAG Front-Ends** *(Nice-to-Have)* | Embeddings, cosine similarity, hybrid search, displaying source citations, confidence scores, grounded answer cards | `10_RAG` & `09_AI_LLM` | Knowledge retrieval pipelines in `Global_Trade_Atlas/packages/knowledge` | 🟡 UNDERSTAND | Understand RAG retrieval and citations; need a dedicated policy-retrieval RAG component to reach 🟢. |
| **16** | **CI/CD, Cloud & Deployment Lifecycle** | Git branching, pull request reviews, automated CI pipelines, containerization (Docker), CDN deployment, environment configs | `00_GOVERNANCE` & `11_SYSTEM_DESIGN` | Monorepo CI and deployment setups in `axis_clean` | 🟢 KNOW | Disciplined developer usage of Git workflows, CI build pipelines, and production release hygiene; do not claim Lead DevOps title. |

---

## 5. Gap Identification & Targeted Action Plan

To move all competencies from 🟡 to 🟢 before interviews:

1. **AI-Driven Workflows & Contact Center Telephony State (Requirement #6)**:
   - *Gap*: Transitioning from standalone AI chat to complex state-machine workflows (agent handling incoming call → AI live suggestion → debt restructuring workflow → approval).
   - *Action*: Build a dedicated Nutun workflow state-machine simulation in our code examples.
2. **Contact Center Data Scalability & Virtualization (Requirement #10)**:
   - *Gap*: Rendering 5,000+ debtor account records without dropping below 60fps.
   - *Action*: Demonstrate virtualized list rendering (e.g. `@tanstack/react-virtual` or windowing techniques).
3. **Formal Accessibility Audit (Requirement #11)**:
   - *Gap*: Verifiable screen-reader and keyboard-only audit logs.
   - *Action*: Document keyboard navigation and ARIA live regions for customer search and payment modals in `05_HTML_CSS`.
4. **Automated Testing Suite for Mission-Critical Flows (Requirement #13)**:
   - *Gap*: Concrete test files testing race conditions, stale closures, and payment arrangement validation.
   - *Action*: Author comprehensive Vitest / React Testing Library tests in `07_TESTING`.
5. **Interactive RAG Grounding UI (Requirement #15)**:
   - *Gap*: Front-end display of vector retrieval citations and confidence intervals for compliance queries.
   - *Action*: Author a model RAG citation viewer component in `10_RAG`.

---

## 5. Architectural Bridge

Every lesson in our curriculum now traces through this matrix:

```text
NUTUN JOB SPECIFICATION
          │
          ▼
JOB_SPEC_COMPETENCY_MATRIX.md  ◄── [CONTROL DOCUMENT]
          │
          ▼
CURRICULUM LESSON (e.g. 03_REACT / 02_JSX_COMPONENTS_PROPS)
          │
          ▼
TECHNICAL MECHANISM & CODE EVIDENCE
          │
          ▼
INTERVIEW QUESTIONS & DEFENSE (13_INTERVIEW_QUESTIONS)
          │
          ▼
EVALUATION: 🟢 KNOW / 🟡 UNDERSTAND / 🔴 NOT YET
```
