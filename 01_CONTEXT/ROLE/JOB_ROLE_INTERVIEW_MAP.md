# Job Specification → Competency & Question Map

Every requirement from [Frontend_Engineer_JobSpec.pdf](file:///c:/Projects/nutun_rsa/Frontend_Engineer_JobSpec.pdf) mapped to technical mechanisms, interview questions, and verified evidence from your code.

---

### 1. React (3–7 Years Experience)
* **What it means**: Deep understanding of component lifecycle, hooks (`useState`, `useEffect`, `useRef`, `useCallback`, `useMemo`), Context, rendering vs. painting, and component composition.
* **Likely Question**: *"How do you structure complex state in React to prevent unnecessary re-renders across deep component trees?"*
* **Strong Answer Contains**: Separation of server-state (TanStack Query) from client UI state (Zustand), derived state instead of redundant state, lifting state up, using component composition (`children`) to avoid prop drilling.
* **My Code Evidence**: [`axis_clean/apps/web`](file:///c:/Projects/axis_clean/apps/web/package.json), [`science-of-our-world`](file:///c:/Projects/science-of-our-world/src/App.tsx).
* **Follow-up / Judgement**: *"What happens if you use `useEffect` with an incomplete dependency array?"* → Stale closures, synchronization bugs, infinite render loops.

---

### 2. Conversational UI & Chatbots
* **What it means**: Building accessible, stateful chat interfaces with message history, auto-scroll management, loading indicators, and user/assistant turn-taking.
* **Likely Question**: *"How do you handle message state and auto-scrolling when an AI assistant is streaming chunks into a chat view?"*
* **Strong Answer Contains**: Normalized message objects (`role`, `content`, `id`, `status`), `useRef` for smooth scrolling to bottom only when user is near bottom (preventing scroll jumping while reading history).
* **My Code Evidence**: [`science-of-our-world/src/components/common/AIAssistant.tsx`](file:///c:/Projects/science-of-our-world/src/components/common/AIAssistant.tsx#L51-L82).
* **Follow-up / Judgement**: *"What if the user scrolls up to read past messages while a new message is streaming in?"* → Auto-scroll must be disabled if the user has manually scrolled away from the bottom.

---

### 3. Streaming Responses & Token-Level UI Updates (Nice to have)
* **What it means**: Receiving incremental token chunks over a persistent stream (Fetch ReadableStream / SSE / Web Worker iterators) and updating React state without frame drops.
* **Likely Question**: *"Why stream AI responses, and how do you implement it in the front-end?"*
* **Strong Answer Contains**: Decreases perceived latency (TTFB to first visible token is ~200ms vs. waiting 5s for full payload). Uses async iterators (`for await (const chunk of stream)`) or `response.body.getReader()`.
* **My Code Evidence**: [`science-of-our-world/src/services/aiService.ts`](file:///c:/Projects/science-of-our-world/src/services/aiService.ts#L53-L67).
* **Follow-up / Judgement**: *"If an LLM streams 50 tokens per second, should you call `setState` 50 times a second?"* → State throttling / batching (via `requestAnimationFrame` or debouncing deltas) prevents UI thread choking.

---

### 4. Vector Search / RAG Front-Ends (Nice to have)
* **What it means**: Building interfaces that display grounded AI answers accompanied by verifiable source citations, document excerpts, and confidence levels.
* **Likely Question**: *"How would you design a RAG interface for contact center agents retrieving compliance policies?"*
* **Strong Answer Contains**: Clear visual distinction between AI synthesis and authoritative retrieved snippets. Expandable source cards showing document name, page number, and similarity score.
* **My Code Evidence**: Document and knowledge retrieval pipelines in `Global_Trade_Atlas/packages/knowledge`.
* **Follow-up / Judgement**: *"What happens if the retrieval step returns low confidence or irrelevant documents?"* → The UI must explicitly warn the agent: *"No authoritative policy match found. Proceed with standard script."* rather than generating an ungrounded hallucination.

---

### 5. Automated Testing Discipline
* **What it means**: Proving UI functionality through automated test suites (unit tests for utility logic, component tests for user interactions, E2E tests for critical user journeys).
* **Likely Question**: *"What is your testing strategy for a mission-critical frontend feature like debt payment submission?"*
* **Strong Answer Contains**: Unit tests for validation schemas (`zod`), component integration tests with React Testing Library and User Event to verify loading/disabled states, and Playwright E2E tests simulating real browser form submission and error recovery.
* **My Code Evidence**: [`axis_clean/apps/web/package.json`](file:///c:/Projects/axis_clean/apps/web/package.json#L44-L60) (Vitest, React Testing Library, Playwright).
* **Follow-up / Judgement**: *"Why test user behavior rather than component implementation details?"* → Testing implementation details (like internal state names) makes tests brittle; testing user interactions (`screen.getByRole('button', { name: /submit/i })`) ensures refactors don't break user workflows.
