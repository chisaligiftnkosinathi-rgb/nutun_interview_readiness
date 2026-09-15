# Nutun AI, Automation & The Durban Innovation Lab

> **EVIDENCE CLASSIFICATION KEY**:
> - `[DOCUMENTED]`: R100M+ Durban Technology and Innovation Lab, "Zoey" autonomous digital collections agent, real-time speech analytics & auto-QA analyzing 100% of calls, 2026 Gartner recognition in CX & AI-enabled operations.
> - `[INFERRED]`: Streaming LLM copilot architecture, RAG retrieval boundaries over internal compliance manuals, SSE/WebSocket transport for live token streams, client-side safety guardrails.
> - `[UNKNOWN]`: Specific foundational LLM vendors (OpenAI vs Anthropic vs self-hosted open-weights models), internal vector database engine.

---

## 1. The Durban Technology & Innovation Lab

Nutun established a dedicated **Technology & Innovation Lab in Durban** backed by over **R100 Million in capital commitments** `[DOCUMENTED]`. 

The lab is tasked with engineering proprietary intellectual property across:
1. **Autonomous Conversational AI ("Zoey")**: Digital collections without human agents.
2. **Real-Time Speech & Interaction Analytics**: Live acoustic and NLP processing on calls.
3. **Agentic Copilots**: Augmenting human agents during complex debt negotiations.
4. **Predictive Workforce Management (WFM)**: Algorithmic staffing models matching agent skill to debt book complexity.

---

## 2. "Zoey": The Autonomous Digital Collections Agent `[DOCUMENTED]`

"Zoey" represents Nutun's deployment of conversational AI directly to debtors across WhatsApp, Web Chat, and SMS.

```text
[ DEBTOR ON WHATSAPP / WEB ]
            │
            ▼
   [ "ZOEY" AGENTIC AI ] ◄──► [ Master Data Universe & Propensity Model ]
            │
    ┌───────┴────────────────────────────────────────┐
    ▼                                                ▼
[ CASE 1: STRAIGHTFORWARD RESOLUTION ]     [ CASE 2: HIGH EMOTION / DISPUTE ]
• Zoey negotiates payment plan             • Zoey detects distressed consumer sentiment
• Discloses NCA-compliant terms            • Gracefully hands off session to human agent
• Sends DebiCheck / Ozow payment link      • Streams conversation history to agent UI
```

### Front-End Engineering for Digital Portals:
* **Zero-Install Web Clients**: Debtors engage via mobile links. The UI must be an ultra-lightweight Progressive Web App (PWA) with instant Time-to-Interactive (TTI) on low-end mobile devices.
* **Accessible Negotiation Sliders**: Interactive UI components allowing consumers to adjust payment terms (months vs instalment) must be fully accessible and touch-friendly, reflecting real-time amortization schedules.

---

## 3. Human Copilot: Augmenting the 5,000+ Seat Floor `[DOCUMENTED]`

For complex voice calls handled by human agents, AI acts as an invisible, real-time copilot:

```text
       [ LIVE CALL AUDIO ]
               │
               ▼
   [ AUTOMATIC SPEECH RECOGNITION (ASR) ] (Transcribes debtor & agent)
               │
               ▼
   [ REAL-TIME COMPLIANCE & INTENT ENGINE ]
               │
               ▼
┌──────────────┴──────────────────────────────────────────┐
│              STREAMING INTO REACT AGENT UI              │
│                                                         │
│ • Live Compliance Checklist: [✓] Greeting  [✓] NCA Warn │
│ • Suggested Response: "Customer qualifies for 20% waiver│
│   if 6-month debit order is signed today [Policy 4.2]"   │
│ • Knowledge Base Citation: [1] Credit Act Sec 103(5)    │
└─────────────────────────────────────────────────────────┘
```

### Front-End Architecture for AI Copilots:
1. **Decoupled Asynchronous Streaming**:
   - Streaming LLM tokens must arrive over **Server-Sent Events (SSE)** or WebSockets without triggering layout thrashing or re-rendering the transactional arrangement form.
2. **Citation Auditing & Grounding (Q8.3)**:
   - Every AI assertion must carry clickable badge citations (e.g. `[1]`, `[2]`). Clicking an anchor opens the exact policy excerpt in a side drawer without losing the agent's cursor position in the payment form.
3. **Transactional Safety Boundary**:
   - The AI can *suggest* a settlement amount, but the UI must prevent the agent from submitting that amount without verifying it against the hard deterministic rules engine. The LLM is never the transactional system of record.
