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

#### 2. The Architectural Boundary
* **Unit Tests (Fast, Pure Logic)**: Test pure reducers, state machines, financial interest calculators, and input formatters directly with raw input/output assertions.
* **Component Tests (Integration / Behavioral)**: Test the component as a black box through user interaction (`userEvent`) and observable DOM/A11y output (`screen.getByRole`).

By separating pure mathematical transitions into pure unit tests and rendering into behavioral tests, we achieve 100% state coverage without making our UI component tests brittle or dependent on internal implementation details."*
