# Nutun Collections Technology: Cheetah CRM, Workflows & Financial Interactions

> **EVIDENCE CLASSIFICATION KEY**:
> - [DOCUMENTED]: Cheetah Collections platform (proprietary core debt recovery engine cited across Nutun operational filings), Promise-to-Pay (PTP) arrangement workflows, omnichannel engagement, automated compliance auditing, DebiCheck / authenticated collections rails.
> - [INFERRED]: Micro-frontend architecture for multi-portfolio tenancy, React form state machines for arrangement negotiations, client-side financial validation routines.
> - [UNKNOWN]: Internal backend database structure and proprietary Cheetah API endpoint nomenclature.

---

## 1. Cheetah: The Core Collections Platform

**Cheetah** is Nutun's proprietary debt collections CRM platform [DOCUMENTED]. Unlike generic CRMs (such as Salesforce or HubSpot) that focus on sales funnels, Cheetah is engineered specifically for the regulatory, legal, and operational complexities of debt recovery and credit rehabilitation.

`	ext
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CHEETAH COLLECTIONS ENGINE                         │
├──────────────────────────────┬───────────────────────────────┬──────────────┤
│ 1. DELINQUENCY LIFECYCLE     │ 2. ARRANGEMENT ENGINE         │ 3. LEGAL &   │
│                              │                               │    COMPLIANCE│
│ • Aging Buckets (30/60/90+)  │ • Promise-to-Pay (PTP) Logic  │ • NCA Limits │
│ • Multi-lender Balances      │ • DebiCheck Mandate Creation  │ • Section 129│
│ • Principal vs Fees vs Int   │ • Affordability Assessment    │ • Dispute Log│
└──────────────────────────────┴───────────────────────────────┴──────────────┘
`

---

## 2. The Anatomy of a High-Stakes Collection Interaction

When an agent interacts with a debtor, the conversation follows a legally structured financial pathway:

`	ext
[ 1. Right-Party Contact (RPC) ]
     Agent verifies identity: Full Name, Masked ID, Date of Birth.
     ▼
[ 2. Statutory Disclosure ]
     Agent informs consumer: Call is recorded; collecting on behalf of [Client/Book].
     ▼
[ 3. Balance & Arrears Presentation ]
     UI displays capital balance, accrued statutory interest, and collection costs.
     ▼
[ 4. Negotiation & Affordability Assessment ]
     Agent explores repayment terms: Lump-sum settlement discount vs multi-month plan.
     ▼
[ 5. Promise-to-Pay (PTP) Commitment ] ◄── [CRITICAL FRONT-END INTERACTION]
     Agent selects instalment amount, payment method (DebiCheck, EasyPay, EFT), and first debit date.
     ▼
[ 6. Mandate Capture & Confirmation ]
     Debtor receives bank DebiCheck prompt on phone; agent confirms mandate lock in UI.
`

---

## 3. Front-End Engineering in Cheetah: Transactional Guarantees

The front-end engineer building collections UI operates under strict financial and legal constraints:

### 1. Zero Tolerance for Float Rounding Errors
In debt restructuring, displaying a monthly instalment as R333.3333333333333 or miscalculating the final balancing payment destroys consumer trust and violates National Credit Act (NCA) fee disclosure rules.
* **Engineering Standard**: All monetary state calculations in React must use integer cents (or specialized decimal arithmetic utilities) before formatting:
  `	s
  // Correct financial arithmetic boundary
  const calculateFinalInstalmentCents = (
    totalBalanceCents: number,
    monthlyInstalmentCents: number,
    numberOfMonths: number
  ): number => {
    const regularPaymentsTotal = monthlyInstalmentCents * (numberOfMonths - 1);
    return totalBalanceCents - regularPaymentsTotal;
  };
  `

### 2. Form State Machines & Idempotency
Double-clicking  Submit Arrangement must never initiate duplicate DebiCheck debit orders against a consumer’s bank account.
* **Engineering Standard**:
  - Payment action buttons transition through an explicit state machine: IDLE $\to$ VALIDATING $\to$ SUBMITTING $\to$ AWAITING_MANDATE $\to$ CONFIRMED.
  - Every arrangement request includes a cryptographically unique idempotencyKey generated at the start of the negotiation session.

### 3. Dynamic Statutory Validation
South African credit law caps interest rates and specifies minimum notice periods for debit orders (e.g. DebiCheck mandates must be submitted at least 2–3 business days prior to the debit date depending on the bank profile).
* **Engineering Standard**: The React date picker and validation schema must compute South African public holidays and banking clearing windows client-side to prevent invalid mandate submissions.
