# Nutun Data Assets & The Master Data Universe (MDU)

> **EVIDENCE CLASSIFICATION KEY**:
> - [DOCUMENTED]: Master Data Universe (MDU) contains 27 million South African credit-active debtors (Transaction Capital / Nutun 2025 Integrated Annual Report), used for pricing purchased books, informing collection propensity, and powering algorithmic contact strategies.
> - [INFERRED]: Data ingestion mechanisms, bureau integration pipelines (TransUnion/Experian/XDS), feature store abstractions, and front-end REST/GraphQL data projection boundaries.
> - [UNKNOWN]: Specific internal database technologies, schema definitions, and internal proprietary pricing equations.

---

## 1. What is the Master Data Universe (MDU)?

The **Master Data Universe (MDU)** is Nutun’s most prized proprietary intellectual capital asset [DOCUMENTED]. 

With credit records spanning over **27 million unique consumer profiles in South Africa**, the MDU contains historical credit performance, debt payment trends, contact history, and employer data accumulated over decades of operations across banking, retail, and telecommunications portfolios.

`	ext
┌─────────────────────────────────────────────────────────────────────────────┐
│                       MASTER DATA UNIVERSE (MDU)                            │
│                     [27 Million Debtor Records]                             │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
            ┌──────────────────────────┼──────────────────────────┐
            ▼                          ▼                          ▼
   PORTFOLIO PRICING          PROPENSITY-TO-PAY          CONTACTABILITY
    & ACQUISITION                 SCORING                   STRATEGY
 • Cash-flow forecasting     • Liquidity prediction    • Valid mobile numbers
 • NPL bid valuations        • Settlement propensity   • Optimal dial times
 • Risk-adjusted hurdle rate • Preferred channel       • Employer / work info
            │                          │                          │
            └──────────────────────────┼──────────────────────────┘
                                       │
                                       ▼
                       FRONT-END CONTROL SURFACE (UI)
                      • Instant Debtor 360 Summary
                      • Color-Coded Propensity Badges
                      • Tailored Restructuring Recommendations
`

---

## 2. Business Function: How Data Creates Operating Leverage

### 1. Portfolio Bidding Advantage (Capital Allocation)
When commercial banks auction off billions in non-performing loans, competitors bid blind or with rudimentary historical averages. Nutun references incoming account ledgers against the **27M MDU** to identify:
* Debtors already paying on another Nutun-managed book.
* Debtors who recently re-entered formal employment.
* Debtors with verified bank accounts ready for DebiCheck integration.

This allows Nutun to price books with unmatched statistical confidence, avoiding unprofitable portfolios and aggressively winning high-yield assets [DOCUMENTED].

### 2. Propensity-to-Pay (Dynamic Operational Prioritization)
Not all delinquent accounts should be called by human agents:
* **High Propensity, High Digital Affinity**: Routed to automated WhatsApp bots or self-service SMS payment links at minimal cost.
* **Medium Propensity, Complex Dispute**: Routed to experienced senior agents in the CCaaS dialler queue.
* **Low Propensity, Untraceable**: Routed to automated credit bureau tracing and data enrichment queues.

---

## 3. Front-End Engineering Implications: Serving MDU Data to the UI

Serving insights derived from a 27-million-record universe into an agent workspace handling 18.5M monthly interactions introduces distinct technical challenges:

### 1. The Debtor 360 Projection Boundary
An agent cannot wait for an analytics cluster to aggregate 10 years of cross-portfolio history during a live call.
* **Architecture**: The front-end consumes a **materialized, read-optimized Debtor 360 projection** [INFERRED].
* **State Management**:
  `	s
  interface DebtorProfileSummary {
    debtorId: string;
    fullName: string;
    maskedIdNumber: string;
    totalExposureAcrossBooks: number;
    currentBookBalance: number;
    propensityTier: 'HIGH' | 'MEDIUM' | 'LOW';
    suggestedRestructureOptions: Array<{
      months: number;
      monthlyInstalment: number;
      settlementDiscountPercent: number;
    }>;
    lastContactOutcome: string;
  }
  `

### 2. Privacy, Masking & POPIA Compliance in the UI [DOCUMENTED]
Under South Africa's **Protection of Personal Information Act (POPIA)** and international data protection laws:
* The front-end must never render full South African National ID numbers or unmasked bank account numbers in plain text unless explicitly unlocked with role-based credentials.
* **DOM Security**: Form inputs handling banking details must have utocomplete= off and sensitive attributes masked to prevent shoulder-surfing, browser extension leaks, or un-sanitized analytics logging.

### 3. Asynchronous Cache Invalidation
When an agent captures an updated phone number or logs a successful debit order, that interaction feeds back into the MDU. The front-end must optimistically reflect the local change without stale cache overwrites if the agent switches between related accounts.
