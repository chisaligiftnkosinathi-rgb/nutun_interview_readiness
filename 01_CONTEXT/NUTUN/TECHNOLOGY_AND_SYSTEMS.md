# Nutun Technology & Systems Architecture Study

> **EVIDENCE CLASSIFICATION KEY**:
> - [DOCUMENTED]: Verified via Transaction Capital / Nutun 2025 Integrated Annual Report, official technology whitepapers, official product filings, and verified 2026 Gartner recognition.
> - [INFERRED]: High-probability architectural deductions based on published capabilities, standard modern BPO/fintech engineering paradigms, and job specification requirements.
> - [UNKNOWN]: Unverified internal implementation details (specific proprietary microservice names, internal table schemas, unannounced vendors).

---

## 1. Executive Summary: The Technology-Enabled Platform

Nutun is not an orthodox call centre that buys off-the-shelf software; it operates as a **vertically integrated, technology-enabled collections, customer experience (CX), and credit lifecycle operating system** [DOCUMENTED].

`	ext
┌──────────────────────────────────────────────────────────────────────────────┐
│                    THE NUTUN VALUE ACCELERATION ENGINE                       │
│                                                                              │
│  [27M Master Data Universe] ──► [Proprietary Scoring] ──► [Campaign Engine]  │
│                                                                   │          │
│  ┌─────────────────────── FRONT-END INTERFACE LAYER ──────────────┴───────┐  │
│  │                                                                        │  │
│  │  [Human Agent Control Surface]       [Consumer Self-Service Portals]   │  │
│  │  • Unified CCaaS Telephony Bar       • PWA / Mobile Responsive Pay     │  │
│  │  • Cheetah Collections CRM UI        • Instant EFT / Card Payment      │  │
│  │  • Streaming AI Copilot / Zoey       • Dynamic Restructuring Slider    │  │
│  │                                                                        │  │
│  └─────────────────────────────────┬──────────────────────────────────────┘  │
│                                    ▼                                         │
│                      [TRANSACTIONAL SETTLEMENT ENGINE]                       │
│                     • DebiCheck / NAEDO Mandate Creation                     │
│                     • Real-Time Payment Gateway Webhooks                     │
│                     • Automated Settlement Letter Generation                 │
└──────────────────────────────────────────────────────────────────────────────┘
`

The technology stack serves two distinct operational masters:
1. **Capital Recovery on Purchased NPL Books**: Maximizing cash recovery curves across distressed debt ledgers purchased at a discount [DOCUMENTED].
2. **Global Omnichannel BPO / CCaaS**: Delivering customer service, voice, chat, dispute resolution, and compliance QA for enterprise clients in South Africa, the UK, Australia, and the US [DOCUMENTED].

---

## 2. Core Architectural Pillars

### Pillar 1: The Master Data Universe (MDU) [DOCUMENTED]
* **Dataset Scope**: Proprietary data assets containing records on **27 million credit-active South African consumers**.
* **Operational Role**:
  - Fuels propensity-to-pay predictive scoring models.
  - Informs contact strategy (best time to dial, best channel: SMS vs WhatsApp vs Outbound Voice).
  - Used by data science teams to price non-performing loan (NPL) portfolios during bidding wars with banks.
* **Front-End Integration**: Delivered into the agent workspace as pre-computed scoring indicators, risk tiering, and optimal repayment plan suggestions.

### Pillar 2: Cheetah Collections CRM [DOCUMENTED]
* **Operational Role**: Nutun's core collections execution platform.
* **Capabilities**:
  - Account history, delinquency aging, outstanding balance breakdown (capital, interest, collection fees).
  - Legal status tracking (Section 129 notices, garnishee orders, dispute holds).
  - Promise-to-Pay (PTP) recording and automated DebiCheck mandate submission.
* **Front-End Boundary**: The front-end engineer builds or modernizes the unified single-pane interface that renders Cheetah account records concurrently with live dialler state and omnichannel chat.

### Pillar 3: CCaaS & Omnichannel Engagement Engine [DOCUMENTED]
* **Operational Role**: Inbound/outbound dialler, WebRTC softphone integration, automatic speech recognition (ASR), and omnichannel customer interactions (Voice, WhatsApp, SMS, Web Chat, Email).
* **Capabilities**:
  - Predictive and preview auto-diallers.
  - Automated Quality Assurance (Auto-QA) analyzing 100% of recorded calls for regulatory compliance (NCA disclosures, greeting protocols).
  - Real-time sentiment and speech analytics.
* **Front-End Boundary**: WebRTC audio controls, incoming call pop events (CTI screen pop), real-time call transcriptions, and supervisor barge-in indicators.

### Pillar 4: AI & Automation ( Zoey & The Durban Innovation Lab) [DOCUMENTED]
* **R&D Investment**: R100M+ committed to the Durban Technology and Innovation Lab [DOCUMENTED].
* **Agentic AI (Zoey)**: Autonomous digital collection agent capable of conversing with debtors across digital messaging channels, negotiating repayment arrangements within strict credit parameters, and processing debit orders without human intervention [DOCUMENTED].
* **Real-Time Human Copilot**: Streaming LLM assistance to human agents, summarizing prior disputes, drafting compliant settlement letters, and suggesting optimal restructuring terms [DOCUMENTED].

---

## 3. The Front-End Engineer's Role: The Human Control Surface

In a contact centre operating 18.5M+ monthly customer interactions, the front-end application is **not a passive viewer—it is the mission-critical human control surface** that dictates operational speed, compliance adherence, and revenue capture.

`	ext
┌─────────────────────────────────────────────────────────────────────────────┐
│                          AGENT WORKSPACE CONTROL SURFACE                    │
├──────────────────────────────┬───────────────────────────────┬──────────────┤
│ 1. CALL / CHANNEL CONTEXT    │ 2. TRANSACTIONAL WORKSPACE    │ 3. AI / RAG  │
│                              │                               │    COPILOT   │
│ • WebRTC Telephony State     │ • Dynamic Debtor Profile      │ • Streaming  │
│ • Caller ID & Right-Party    │ • Multi-Stage PTP Flow        │   Guidance   │
│   Contact (RPC) verification │ • Affordability Assessment    │ • Policy     │
│ • Live Call Duration Timer   │ • DebiCheck Mandate Capture   │   Citations  │
│ • Sentiment / Tone Warning   │ • Fee Breakdown / Interest    │ • Auto-Notes │
└──────────────────────────────┴───────────────────────────────┴──────────────┘
`

### Critical Engineering Requirements for the Front-End Engineer:
1. **Zero-Latency Screen Pop**:
   - When the dialler connects a call, the debtor's file must render in sub-second time. Layout thrashing or cascading waterfalls freeze the agent during the crucial first 5 seconds of customer contact.
2. **Deterministic Financial State**:
   - In negotiation, calculating repayment terms (e.g. 12 months @ R450 vs 6 months @ R850) requires zero-defect state machines. Rounding errors, race conditions, or unhandled NaN values destroy compliance trust.
3. **Graceful Asynchronous AI Integration**:
   - Streaming tokens from an LLM copilot must never freeze keyboard navigation, block the telephony controls, or bypass human confirmation before committing payment terms to the database.
4. **Resilient Consumer Self-Service**:
   - Digital payment web links must run smoothly on low-spec budget smartphones over erratic 3G connections, requiring strict bundle-size budgeting and accessible, high-contrast touch interfaces.

---

## 4. Key Architectural Trade-Offs

| Decision Area | Enterprise Risk | Front-End Engineering Countermeasure |
| :--- | :--- | :--- |
| **Account Navigation** | Agent switches caller; stale debt data renders for new debtor (POPIA data breach). | Strict AbortController cancellation + epoch/account ID request binding in state layer. |
| **AI Copilot Suggestions** | Model hallucinates unauthorized 80% settlement discount. | Decoupled UI layer: AI output is strictly marked advisory; settlement buttons require independent policy check and manager override pin. |
| **Real-Time Dialler Events** | Network blip drops WebSocket connection; agent cannot terminate or transfer call. | Dual-channel resilience: Heartbeat pings, exponential backoff reconnection, and fallback HTTP polling for call state. |
| **High-Volume Data Tables** | 500+ past payments and ledger entries freeze DOM rendering. | Virtualized scrolling (@tanstack/react-virtual), zero layout recalculations during active calls. |
