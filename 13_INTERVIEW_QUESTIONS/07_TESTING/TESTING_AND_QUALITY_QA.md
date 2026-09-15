# Automated Testing Discipline & Quality Assurance: Interview Questions & Verified Answers

> **ROLE CONTEXT**: Nutun Front-End Engineer (OfferZen Facilitated)  
> **OPERATIONAL SCALE**: 18.5M monthly customer interactions, up to 10,000 concurrent agents, mission-critical financial debt negotiation workflows, and resilient async state.

---

## Q11.1: React Testing Library — User Behavior vs. Implementation Details

### Question
> *"A junior developer writes:*
> ```tsx
> expect(component.state().isSubmitting).toBe(true);
> expect(wrapper.find('.loading-spinner').exists()).toBe(true);
> expect(component.instance().handleSubmit).toHaveBeenCalled();
> ```
> *Would you accept this as a good React Testing Library test? Why or why not? How would you test the same behavior from the user's perspective?"*

### Verified Candidate Answer

#### 1. Why I Would Reject This Test: The Implementation Detail Trap
No, I would **reject this test** in code review. 

This test uses legacy Enzyme-style patterns that test **how the component is implemented internally**, rather than **what the component actually does for the user**:
1. **Coupled to Internal Variable Names (`isSubmitting`)**: If tomorrow I refactor the component from a boolean `isSubmitting` flag to a state machine (`status: 'SUBMITTING'`) or adopt TanStack Query (`isPending`), the test breaks immediately—even though the application still functions identically for the user. This creates **brittle tests** that resist refactoring.
2. **Coupled to CSS Class Names (`.loading-spinner`)**: If a designer renames the CSS class to `.spinner-ring` or changes the visual indicator to an SVG or text, the test fails. CSS class names are implementation styling details, not user-perceivable contracts.
3. **Coupled to Component Methods (`instance().handleSubmit`)**: Users do not invoke JavaScript class methods; they click buttons or press keys. Testing that a method was called tells you nothing about whether the form actually dispatched the right payload or updated the UI.

The governing principle of modern React testing (Kent C. Dodds) is:
> **"The more your tests resemble the way your software is used, the more confidence they can give you."**

---

#### 2. How to Test from the User's Perspective (The Production Standard)
Instead of inspecting internal state or CSS classes, a production test queries the **Accessibility Tree** using accessible roles and user actions:

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { SettlementArrangementForm } from './SettlementArrangementForm';

test('submits valid settlement arrangement and displays confirmation', async () => {
  const user = userEvent.setup();
  render(<SettlementArrangementForm customerId="ACC-99214" />);

  // 1. Locate fields by their accessible names (exactly as a user or screen reader does)
  const amountInput = screen.getByRole('textbox', { name: /monthly instalment amount/i });
  const submitButton = screen.getByRole('button', { name: /submit arrangement/i });

  // 2. Interact using userEvent (dispatches realistic hover, focus, keydown, input, keyup events)
  await user.type(amountInput, '2500');
  await user.click(submitButton);

  // 3. Assert loading state via accessible UI contract (button disabled, status text)
  expect(submitButton).toBeDisabled();
  expect(screen.getByRole('status')).toHaveTextContent(/processing arrangement/i);

  // 4. Assert async completion (waits for real DOM appearance, not internal state)
  expect(await screen.findByRole('heading', { name: /arrangement confirmed/i })).toBeInTheDocument();
  expect(screen.getByText(/monthly debit of R2,500 scheduled/i)).toBeInTheDocument();
});
```

---

#### 3. Core Testing Architectural Rules
* **Query Priority Hierarchy**:
  1. `getByRole` (Checks the Accessibility Tree: role + accessible name).
  2. `getByLabelText` (For form inputs).
  3. `getByText` (For non-interactive static content).
  4. `getByTestId` (Strictly as an emergency escape hatch for dynamic canvas or unexposed elements).
* **`userEvent` over `fireEvent`**:
  * `fireEvent.change(input)` artificially dispatches a single raw DOM event.
  * `userEvent.type(input, '2500')` accurately simulates real browser event dispatching: clicking the element, firing `focus`, dispatching `keydown`, `keypress`, `input`, and `keyup` for every single character, properly exercising React's event pooling and input masking.
* **Testing Server Validation Failure**:
  ```tsx
  // Assert server 422 error presentation
  expect(await screen.findByRole('alert')).toHaveTextContent(/amount exceeds permitted limit/i);
  expect(amountInput).toHaveAttribute('aria-invalid', 'true');
  ```

---

### The Senior Curveball Defense

> **Interviewer**: *"If we never test internal state in our component tests, how do you verify that our complex reducer or state machine transitions are mathematically correct?"*

### Candidate Defense
*"This is where we strictly distinguish **Component Integration Testing** from **Pure State Machine / Reducer Unit Testing**:

#### 1. The Reducer is a Pure Function (Unit-Test it Directly)
A reducer or state machine (`(state, action) => newState`) is a pure, deterministic JavaScript function with **zero DOM dependencies**:
* We do **not** test reducer logic by inspecting React component instances.
* We test the reducer as an isolated pure function in a dedicated unit test:
  ```typescript
  test('transitions from THINKING to STREAMING on FIRST_CHUNK action', () => {
    const initialState: AIWorkflowState = { 
      status: 'THINKING', 
      requestId: 'REQ-1', 
      prompt: 'Summarise', 
      startTime: 1000 
    };

    const nextState = workflowReducer(initialState, {
      type: 'CHUNK_RECEIVED',
      payload: { requestId: 'REQ-1', chunk: 'Hello' }
    });

    expect(nextState).toEqual({
      status: 'STREAMING',
      requestId: 'REQ-1',
      prompt: 'Summarise',
      accumulatedText: 'Hello'
    });
  });

  test('drops actions from stale requestIds without state modification', () => {
    const currentState = { status: 'STREAMING', requestId: 'REQ-NEW', ... };
    const staleAction = { type: 'CHUNK_RECEIVED', payload: { requestId: 'REQ-OLD', chunk: 'Stale' } };
    
    const result = workflowReducer(currentState, staleAction);
    expect(result).toBe(currentState); // Referential equality preserved
  });
  ```

By separating pure mathematical transitions into pure unit tests and rendering into behavioral tests, we can exhaustively test our defined state transition matrix without making our UI component tests brittle or dependent on internal implementation details."*

---

## Q11.3: Network Mocking — Mock Service Worker (MSW) vs. `global.fetch` Spying

### Scenario — Nutun Settlement Workspace
Your React settlement workspace calls:
```text
POST /api/settlements
```
The backend can return:
* `201 Created` — Settlement created with DebiCheck mandate ID.
* `400 Bad Request` — Financial validation failure (e.g. instalment below legal threshold).
* `401 / 403 Forbidden` — Session expired or agent lacks supervisor override mandate.
* `409 Conflict` — Idempotency conflict / duplicate settlement attempt.
* `500 Internal Server Error` — Core banking / Cheetah API gateway outage.
* Network timeouts, delayed streaming chunks, or malformed JSON payloads.

A developer writes tests using:
```ts
vi.spyOn(global, 'fetch').mockResolvedValue({
  ok: true,
  json: async () => ({ settlementId: 'SET-1029', status: 'CONFIRMED' })
} as Response);
```

### Interview Question
> *"Why would you prefer Mock Service Worker (MSW) over mocking `global.fetch` directly with `vi.spyOn` or `jest.fn`? What catastrophic production bugs slip through when you mock `fetch` directly in complex financial or AI-driven frontends?"*

---

### Verified Candidate Answer

#### 1. Why `global.fetch` Mocking Is Dangerous in Production Systems
Mocking `global.fetch` directly via `vi.spyOn(global, 'fetch')` creates an **artificial, brittle mock boundary** that tests JavaScript stub execution rather than actual network integration:

1. **Bypasses Real Browser Network Semantics**:
   - `fetch` mocking replaces the browser's networking interface with a plain JavaScript Promise.
   - It **does not test** header serialization, CORS behavior, query string encoding, multipart form data, or HTTP request method validation (`POST` vs `PUT`).
   - If your production code accidentally sends `GET` instead of `POST`, or omits `'Content-Type': 'application/json'`, a `global.fetch` mock still resolves happily while production crashes with `415 Unsupported Media Type` or `405 Method Not Allowed`.
2. **Couples Tests to Implementation Plumbing**:
   - If tomorrow the engineering team replaces `fetch` with an HTTP client wrapper, an Axios instance, or TanStack Query with custom request interceptors, every single `global.fetch` mock breaks.
3. **Mocks Lack Protocol Realism (HTTP Semantics)**:
   - When developers mock `fetch`, they almost always mock the happy path: `mockResolvedValue({ ok: true, json: ... })`.
   - They rarely mock real HTTP response structures: `status: 422`, `headers: Headers`, `statusText: 'Unprocessable Entity'`, stream body readers (`body.getReader()`), or network disconnection errors (`TypeError: Failed to fetch`).

---

#### 2. Why Mock Service Worker (MSW) is the Industry Standard
**Mock Service Worker (MSW)** intercepts requests at the **network transport layer** (using a Service Worker in browser dev mode, and NodeJS request interception via `msw/node` in Vitest/Jest):

```text
┌────────────────────────────────────────────────────────────────────────┐
│                          MSW ARCHITECTURE                              │
│                                                                        │
│  [ React Component ] ──► [ TanStack Query / fetch / SDK ]             │
│                                     │                                  │
│                                     ▼                                  │
│                           [ HTTP Request Plane ]                       │
│                                     │                                  │
│                       ┌─────────────┴─────────────┐                    │
│                       ▼                           ▼                    │
│                [ MSW Interceptor ]         [ Real Backend ]            │
│               (Vitest / Jest Tests)      (Production Staging)          │
│                       │                                                │
│                       ▼                                                │
│             [ Real HTTP Response ]                                     │
│         (status, headers, body stream)                                 │
└────────────────────────────────────────────────────────────────────────┘
```

* **Zero Application Code Changes**: Your React components and API clients execute real, unmodified `fetch` calls. The application does not know or care that MSW is running.
* **Declarative Network Contract**: Handlers are defined declaratively as standard HTTP routes matching your real backend endpoints.
* **Multi-Layer Reusability**: The exact same mock handlers run across **Vitest unit tests**, **Storybook visual components**, and **local development mock servers**.

---

#### 3. Production Test Implementation: MSW in Vitest

##### Handlers Definition (`mocks/handlers.ts`):
```typescript
import { http, HttpResponse, delay } from 'msw';

export const handlers = [
  http.post('/api/settlements', async ({ request }) => {
    // 1. Verify real request headers
    const contentType = request.headers.get('Content-Type');
    if (!contentType?.includes('application/json')) {
      return new HttpResponse(null, { status: 415, statusText: 'Unsupported Media Type' });
    }

    const idempotencyKey = request.headers.get('X-Idempotency-Key');
    if (!idempotencyKey) {
      return HttpResponse.json(
        { errorCode: 'MISSING_IDEMPOTENCY_KEY', message: 'Mandatory transaction key absent' },
        { status: 400 }
      );
    }

    const body = await request.json() as { amountCents: number; customerId: string };

    // 2. Validate business rules realistically
    if (body.amountCents < 10000) { // Below R100 minimum threshold
      return HttpResponse.json(
        { errorCode: 'BELOW_MINIMUM_SETTLEMENT', message: 'Instalment cannot be less than R100' },
        { status: 422 }
      );
    }

    return HttpResponse.json({
      settlementId: 'SET-99482',
      status: 'CONFIRMED',
      mandateRef: 'DEBICHECK-88192',
      createdAt: new Date().toISOString()
    }, { status: 201 });
  }),
];
```

##### Test Execution (`SettlementWorkspace.test.tsx`):
```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { server } from '../mocks/server';
import { http, HttpResponse } from 'msw';
import { SettlementForm } from './SettlementForm';

describe('Settlement Submission Network Scenarios', () => {
  test('handles 409 Conflict gracefully by presenting existing arrangement details', async () => {
    const user = userEvent.setup();

    // Dynamically override MSW handler for this specific failure scenario
    server.use(
      http.post('/api/settlements', () => {
        return HttpResponse.json(
          { errorCode: 'DUPLICATE_PTP', message: 'An active arrangement already exists for this debtor.' },
          { status: 409 }
        );
      })
    );

    render(<SettlementForm debtorId="DEBT-5021" />);

    await user.type(screen.getByRole('textbox', { name: /instalment amount/i }), '500');
    await user.click(screen.getByRole('button', { name: /submit arrangement/i }));

    // Verify accessible error presentation
    expect(await screen.findByRole('alert')).toHaveTextContent(/active arrangement already exists/i);
    expect(screen.getByRole('button', { name: /view existing arrangement/i })).toBeInTheDocument();
  });

  test('handles 504 Gateway Timeout without losing agent form input', async () => {
    const user = userEvent.setup();

    server.use(
      http.post('/api/settlements', () => {
        return HttpResponse.error(); // Simulates network drop / abort / timeout
      })
    );

    render(<SettlementForm debtorId="DEBT-5021" />);

    const input = screen.getByRole('textbox', { name: /instalment amount/i });
    await user.type(input, '1200');
    await user.click(screen.getByRole('button', { name: /submit arrangement/i }));

    expect(await screen.findByRole('alert')).toHaveTextContent(/network connection lost/i);
    // Crucial financial UX guarantee: entered form data is not wiped on network error!
    expect(input).toHaveValue('1200');
  });
});
```

---

#### 4. The Senior Architectural Summary
| Capability | `vi.spyOn(global, 'fetch')` | Mock Service Worker (MSW) |
| :--- | :--- | :--- |
| **Interception Level** | JavaScript variable replacement | Network transport plane (`msw/node` / Service Worker) |
| **Request Inspection** | Requires manual assertion on call arguments | Enforces real HTTP request method, URL, headers, and body |
| **Response Realism** | Plain JavaScript objects | Full `Response` object with headers, statuses, and ReadableStreams |
| **Refactoring Safety** | **Zero**: Breaks if client library changes | **100%**: Decoupled from client-side networking libraries |
| **Edge-Case Simulation** | Extremely tedious (requires simulating stream readers) | Native: supports server-sent events, chunked streams, network drop |

---

## Q11.4: Testing Nondeterministic AI Streams & Token UI 🔴

### Scenario — Nutun Real-Time AI Copilot Workspace
In the Nutun agent workspace, an AI co-pilot streams legal and policy advice during live debtor negotiations.
* The backend streams token chunks over **Server-Sent Events (SSE)**.
* Chunks can arrive rapidly (e.g. 20ms intervals) or stall due to network congestion.
* The LLM's output is **nondeterministic**: the exact wording, token boundaries, and chunk splits vary from run to run.
* The stream may experience network disconnections mid-sentence, mid-word, or terminate abruptly.
* The UI must parse dynamic citations (e.g. `[1]`, `[2]`), update an accessible live region, and maintain smooth rendering without locking the main thread.

### Interview Question
> *"How do you test an AI streaming frontend when the backend response is inherently nondeterministic, chunks arrive asynchronously, and network streams can break mid-transmission? How do you guarantee test determinism and prevent flaky tests in CI?"*

---

### Verified Candidate Answer

#### 1. The Core Testing Paradigm: Decouple Model Evaluation from Frontend Contract Testing
In an automated testing pipeline, we must never test the **linguistic quality or intelligence of the LLM** inside frontend component tests. Model evaluation (hallucination rate, tone, accuracy) belongs in an offline Python/evals pipeline (e.g. Ragas, DeepEval).

The **frontend automated testing suite** is responsible for testing the **client-side streaming contract, state transitions, error boundaries, and accessibility**:

```text
┌────────────────────────────────────────────────────────────────────────┐
│               TESTING NONDETERMINISTIC STREAMING FRONTENDS             │
│                                                                        │
│  [ TEST SCOPE 1: PURE STREAM PARSER ]  ──►  Unit Test (Deterministic)   │
│  • SSE frame parsing (`data: ...\n\n`)      • Test chunk edge-cases    │
│  • Token concatenation & markdown splits    • Test broken multibyte utf│
│                                                                        │
│  [ TEST SCOPE 2: ASYNC STREAM CONTROLLER ] ──► Integration (MSW)      │
│  • Simulated ReadableStream chunks          • Fast controlled timing   │
│  • Connection stall & timeout handling      • AbortController cancel   │
│                                                                        │
│  [ TEST SCOPE 3: ACCESSIBLE UI & CITATIONS ] ──► RTL / UserEvent       │
│  • Markdown & citation anchor rendering     • ARIA live announcements  │
│  • Uninterrupted agent form interaction     • Zero layout thrashing    │
└────────────────────────────────────────────────────────────────────────┘
```

---

#### 2. Strategy 1: Controlled Deterministic Streaming with MSW & `ReadableStream`
To eliminate CI flakiness, our tests use MSW to return a real `ReadableStream` that yields **fixed, predetermined fixtures at controlled microsecond delays**:

```typescript
// mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const createStreamingHandler = (chunks: string[], delayMs = 10) => {
  return http.post('/api/copilot/stream', () => {
    const encoder = new TextEncoder();
    
    const stream = new ReadableStream({
      async start(controller) {
        for (const chunk of chunks) {
          if (delayMs > 0) await new Promise((res) => setTimeout(res, delayMs));
          // Encode as standard SSE event format
          controller.enqueue(encoder.encode(`data: ${JSON.stringify({ text: chunk })}\n\n`));
        }
        controller.enqueue(encoder.encode('data: [DONE]\n\n'));
        controller.close();
      }
    });

    return new HttpResponse(stream, {
      headers: {
        'Content-Type': 'text/event-stream',
        'Cache-Control': 'no-cache',
        'Connection': 'keep-alive'
      }
    });
  });
};
```

---

#### 3. Strategy 2: Testing Mid-Stream Network Drops and Stream Termination
A common production failure occurs when the client’s network drops after receiving 50 tokens. Does the UI crash with an unhandled stream abort exception, or does it preserve the partial message and present an accessible retry banner?

```typescript
test('handles mid-stream network termination gracefully and preserves partial output', async () => {
  const user = userEvent.setup();

  // Simulate a stream that errors after 2 chunks
  server.use(
    http.post('/api/copilot/stream', () => {
      const encoder = new TextEncoder();
      const stream = new ReadableStream({
        start(controller) {
          controller.enqueue(encoder.encode('data: {"text":"Customer qualifies for waiver"}\n\n'));
          // Abruptly terminate the stream with an error
          controller.error(new Error('Connection reset by peer'));
        }
      });

      return new HttpResponse(stream, {
        headers: { 'Content-Type': 'text/event-stream' }
      });
    })
  );

  render(<AICopilotPanel customerId="CUST-1092" />);
  await user.click(screen.getByRole('button', { name: /ask copilot/i }));

  // 1. Assert partial text is preserved in the DOM (not blanked out!)
  expect(await screen.findByText(/customer qualifies for waiver/i)).toBeInTheDocument();

  // 2. Assert error state is surfaced accessibly
  expect(await screen.findByRole('alert')).toHaveTextContent(/stream interrupted/i);

  // 3. Assert retry action is offered
  expect(screen.getByRole('button', { name: /retry generation/i })).toBeEnabled();
});
```

---

#### 4. Strategy 3: Testing User Cancellation (`AbortController`)
In high-volume call centres, an agent may switch customer tabs or click "Cancel" while a 500-token stream is in flight. The UI must abort the underlying HTTP stream immediately to prevent memory leaks and zombie state updates:

```typescript
test('aborts network stream immediately when agent cancels or navigates away', async () => {
  const user = userEvent.setup();
  let streamWasAborted = false;

  server.use(
    http.post('/api/copilot/stream', ({ request }) => {
      request.signal.addEventListener('abort', () => {
        streamWasAborted = true;
      });

      const stream = new ReadableStream({
        async start(controller) {
          // Long delayed stream
          await new Promise((res) => setTimeout(res, 5000));
          controller.close();
        }
      });

      return new HttpResponse(stream, {
        headers: { 'Content-Type': 'text/event-stream' }
      });
    })
  );

  render(<AICopilotPanel customerId="CUST-1092" />);
  await user.click(screen.getByRole('button', { name: /ask copilot/i }));

  // Agent decides not to wait and clicks Stop
  const stopButton = screen.getByRole('button', { name: /stop generating/i });
  await user.click(stopButton);

  // Assert request signal received abort event
  expect(streamWasAborted).toBe(true);
  expect(screen.getByRole('button', { name: /ask copilot/i })).toBeEnabled();
});
```

---

#### 5. Strategy 4: Testing Dynamic Citations & Accessibility Tree Updates
When tokens stream in, citations like `[1]` and `[2]` are progressively parsed and rendered into interactive buttons. The test verifies that:
1. Citations resolve to interactive buttons with accessible names.
2. The live region (`aria-live="polite"`) updates without firing aggressive audio alerts on every single character:

```typescript
test('renders interactive policy citations as tokens arrive', async () => {
  const user = userEvent.setup();

  server.use(
    createStreamingHandler([
      'Penalty interest may be waived up to 50% ',
      'provided debtor commits to 6-month debit order [1].'
    ])
  );

  render(<AICopilotPanel customerId="CUST-1092" />);
  await user.click(screen.getByRole('button', { name: /ask copilot/i }));

  // Wait for stream completion
  expect(await screen.findByText(/penalty interest may be waived/i)).toBeInTheDocument();

  // Verify citation button rendered with accessible label
  const citationBtn = await screen.findByRole('button', { name: /citation 1/i });
  expect(citationBtn).toBeInTheDocument();

  // Clicking citation opens policy drawer without page navigation
  await user.click(citationBtn);
  expect(await screen.findByRole('dialog', { name: /policy citation details/i })).toBeInTheDocument();
});
```

---

#### 6. Summary of Architectural Guardrails for AI Stream Testing
1. **Never test live model APIs in frontend CI**: All streaming network payloads are intercepted via MSW using deterministic string chunks.
2. **Test the streaming state machine in isolation**: Verify transitions (`IDLE` $\to$ `CONNECTING` $\to$ `STREAMING` $\to$ `COMPLETED` / `ERRORED` / `ABORTED`) via pure reducer unit tests.
3. **Assert accessibility contracts**: Verify `aria-live="polite"` handles text updates and keyboard focus is never stolen during active streaming.
4. **Assert cancellation semantics**: Guarantee that unmounting the component or clicking "Cancel" dispatches `AbortController.abort()` to the network layer.
