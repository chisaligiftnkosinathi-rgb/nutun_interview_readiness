# Accessibility Auditing & WCAG Compliance: Interview Questions & Verified Answers

> **ROLE CONTEXT**: Nutun Front-End Engineer (OfferZen Facilitated)  
> **OPERATIONAL SCALE**: 18.5M monthly customer interactions, up to 10,000 concurrent agents, dense CRM dashboards, and mission-critical accessibility compliance (WCAG 2.1 AA).

---

## Q9.6: Automated vs. Manual Accessibility Testing & WCAG 2.1 AA Verification (Identified Gap 🔴 Resolved)

### Question
> *"If your `axe-core` tests pass in CI, how confident are you that the application is WCAG 2.1 AA compliant? What accessibility problems can automated testing miss, and how would you design an accessibility testing strategy for this React application?"*

### Verified Candidate Answer

#### 1. The Reality of Automated Testing (`axe-core`)
Passing an automated `axe-core` or Lighthouse test suite gives confidence regarding **syntactic and rule-based markup properties**, but it does **not** guarantee WCAG 2.1 AA compliance or real-world usability:
* **What Automated Tools Catch**:
  * Missing `alt` attributes on images.
  * Contrast ratio mismatches in static text.
  * Form inputs missing associated `<label>` elements.
  * Invalid ARIA attribute syntax (e.g. misspelled roles).
  * Duplicate HTML `id` attributes.
* **What Automated Tools CANNOT Detect**:
  * **Logical Tab Order**: Automated tools know an element is focusable; they cannot know if the sequence of `Tab` stops makes cognitive sense to an agent.
  * **Meaningful Text Quality**: An image with `alt="image123.jpg"` passes automated tests but is completely useless to a screen-reader user.
  * **Dynamic Interaction Flow**: Whether focus moves inside a modal when opened, or drops into the void when it closes.
  * **Screen Reader Announcement Timing**: Whether an `aria-live` announcement interrupts active typing or announces 40 streaming tokens per second.
  * **State Synchronization**: Whether visible error messages are actually conveyed to assistive tech when form submission fails.

Automated accessibility testing is a **linter for HTML semantics**—it catches low-hanging syntax bugs, but human interaction testing is required to verify actual accessibility.

---

#### 2. The 3-Tier Testing Strategy for Nutun

```text
TIER 1: AUTOMATED CI GATES (Fast, Shift-Left)
  - eslint-plugin-jsx-a11y (Catches syntax/markup errors during coding)
  - @axe-core/playwright in E2E smoke tests (Fails CI builds on markup violations)
        │
        ▼
TIER 2: MANUAL KEYBOARD-ONLY AUDIT PROTOCOL (Engineer Driven)
  - Verify every user journey without touching a mouse or trackpad.
        │
        ▼
TIER 3: SCREEN-READER VERIFICATION (NVDA on Windows / VoiceOver on macOS)
  - Verify auditory announcements, timing, and dynamic state transitions.
```

---

#### 3. The Keyboard-Only Testing Protocol (Step-by-Step)
I test the settlement workflow using only a physical keyboard:
1. **Initial Access**: Navigate to the customer search input using `Tab`. Verify a high-contrast, clearly visible focus ring is present.
2. **Search Execution**: Type customer ID, press `Enter`. Verify focus does not get hijacked while results load.
3. **Table Navigation**: `Tab` through search results. Verify each row action button has an accessible label (e.g. *"Open settlement for John Smith, Account 99214"*, not just *"Open"*).
4. **Modal Launch**: Press `Enter` on "Confirm Settlement".
   - **Focus Capture**: Verify focus immediately moves inside the dialog (to Cancel or heading).
   - **Focus Cycling**: Press `Tab` through terms, Cancel, Confirm. On the Confirm button, pressing `Tab` must cycle back to Cancel.
   - **Reverse Cycling**: Press `Shift+Tab` from Cancel; verify it wraps to Confirm.
   - **Interaction Blocking**: Verify pressing `Tab` never leaks into the background customer table.
   - **Dismissal**: Press `Escape`. Verify modal closes and focus **returns programmatically to the triggering button**.

---

#### 4. Screen-Reader Verification (NVDA on Windows)
Testing with a screen reader evaluates the auditory user experience:
1. **Dynamic Streaming AI**: Verify that incoming LLM tokens do **not** spam the screen reader. Announce only milestones (*"AI is drafting summary..."* and *"Summary completed"*).
2. **Form Errors on Submit**: When 422 errors arrive, verify the screen reader immediately announces the Error Summary alert: *"3 errors found in your payment arrangement"*, and that each error link describes the problem clearly.
3. **No Phantom Content**: Verify that visually hidden text (`.sr-only`) provides necessary context without cluttering speech output.

---

### The Product Manager Curveball Defense

> **Product Manager**: *"We already have axe in CI with 0 violations. Why spend valuable engineering sprint time manually testing with a keyboard and screen reader?"*

### Candidate Defense
*"Because **automated tests test the code's syntax, but human beings experience the workflow's behavior.**

Here are 3 concrete production realities:

#### 1. 'Zero axe violations' Can Still Leave the Product Completely Unusable
A developer can build a payment modal using `<div role="dialog" aria-modal="true">` with clean contrast and valid labels. `axe-core` will report **0 violations**.
* But when an agent tabs through the modal, focus continues tabbing into the background table behind the backdrop.
* When they press `Escape`, nothing happens.
* When they submit, focus is lost in `document.body`.
* To an automated tool, every individual element looks valid. But to an agent using assistive technology, **the workspace is completely broken and unusable**.

#### 2. Regulatory & Commercial Risk
Nutun operates in heavily regulated financial environments with enterprise clients (banks, lenders). 
* Enterprise RFPs and compliance standards mandate **adherence to WCAG 2.1 AA**, not merely 'passing an automated scanner'.
* Relying solely on automated testing exposes the business to accessibility compliance failures, audits, and reputational risk.

#### 3. Operational Velocity for All Agents
Keyboard accessibility is not just for visually impaired users. Contact centre agents handling 50 calls a day rely heavily on keyboard shortcuts and rapid tabbing to input data without reaching for a mouse every 5 seconds.
* When focus restoration breaks or focus jumps unexpectedly, agents lose seconds on every call, driving up Average Handle Time (AHT) across 10,000 agents.

**The Bottom Line**:
> **Axe is our safety net to ensure we don't commit syntax blunders. Manual keyboard and screen-reader testing is how we guarantee the software actually works for human beings."*
