# Nutun CCaaS, Omnichannel Telephony & Agent Workflow Architecture

> **EVIDENCE CLASSIFICATION KEY**:
> - `[DOCUMENTED]`: 5,000+ seat physical infrastructure (scalable to 10,000 concurrent agents), proprietary telephony & dialler infrastructure, 18.5M+ monthly interactions, omnichannel routing (Voice, SMS, WhatsApp, Web Chat, Email), Auto-QA auditing 100% of calls.
> - `[INFERRED]`: CTI Screen Pop protocols (WebSocket/SIP events), WebRTC browser softphone audio controls, multi-pane agent desktop layout state machines.
> - `[UNKNOWN]`: Specific PBX/SIP hardware switch vendor (Genesys vs Avaya vs Amazon Connect vs proprietary asterisk fork).

---

## 1. CCaaS at Nutun Scale: 18.5 Million Monthly Interactions

Nutun's Contact Centre as a Service (CCaaS) environment operates at immense industrial scale `[DOCUMENTED]`. With thousands of agents operating across shifts in South Africa, the UK, Australia, and the US, telephony and digital channels are unified into a centralized agent workspace.

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CCaaS & AGENT WORKFLOW                             │
├──────────────────────────────┬───────────────────────────────┬──────────────┤
│ 1. TELEPHONY / CTI LAYER     │ 2. AGENT CONTROL SURFACE      │ 3. QA &      │
│                              │                               │    SUPERVISOR│
│ • WebRTC Audio Softphone     │ • Sub-Second Screen Pop       │ • 100% Auto  │
│ • Inbound ACD & Predictive   │ • Debtor Profile & History    │   Speech QA  │
│   Outbound Auto-Dialler      │ • Multi-Tab Arrangement Form  │ • Real-Time  │
│ • Call Hold / Transfer / Aux │ • Wrap-Up & Disposition Code  │   Sentiment  │
└──────────────────────────────┴───────────────────────────────┴──────────────┘
```

---

## 2. The Critical Path: The CTI Screen Pop

When an outbound predictive dialler detects a live human voice (filtering out answering machines), it connects the call to an available agent in milliseconds.

### The Front-End Challenge:
1. **The Dialler Webhook / WebSocket Event**:
   - The backend pushes an `INCOMING_CALL_ESTABLISHED` event containing `{ debtorId: 'xyz', channel: 'VOICE', callId: '123' }`.
2. **Sub-Second Rendering**:
   - The agent workspace must render the debtor's profile, outstanding balances, and delinquency history before the agent says their opening statutory greeting.
   - If the front-end exhibits a 2-second render stall, the debtor hears dead air and hangs up, destroying Right-Party Contact (RPC) yield.

---

## 3. Front-End Architectural Requirements for CCaaS Workspaces

### 1. Robust WebRTC Audio & Media Pipeline Boundary
* **The Browser vs. JavaScript Distinction**:
  - The low-level WebRTC media pipeline—packet decoding, jitter buffering, acoustic echo cancellation (AEC), and OS hardware audio rendering—is managed by **internal browser C++ threads**, outside the JavaScript main thread.
  - A heavy React task does not automatically corrupt OS audio buffers directly.
  - However, long main-thread tasks (>50ms) choke **signaling event dispatching**, UI state synchronization, and user interaction (mute, hold, hang up). The agent experiences an unresponsive workstation while the call is live.
* **Essential WebRTC Vocabulary**:
  - `RTCPeerConnection`: Browser API representing the full peer-to-peer audio connection between the browser softphone and the Nutun telephony gateway/PBX.
  - **Signaling**: The out-of-band mechanism (typically WebSockets or SIP-over-WebSocket) used to exchange connection metadata before media flows.
  - **SDP (Session Description Protocol)**: Formatted metadata describing media codecs, encryption, and bandwidth parameters.
  - **ICE / STUN / TURN**: Framework for establishing NAT traversal. STUN resolves public IP addresses; TURN acts as a fallback relay server if enterprise firewalls block direct UDP media traffic.
  - **MediaStream & MediaStreamTrack**: The browser representations of audio inputs/outputs.
  - **AudioWorklet**: Modern Web Audio API for custom audio processing off the main thread.

### 2. Multi-Tab / Multi-Pane Agent Desktop
Agents simultaneously reference:
* Left Pane: Dialler status, call timer, caller metadata.
* Center Pane: Cheetah Collections transaction form and payment schedule calculator.
* Right Pane: Real-time speech transcription, compliance checklist, and AI copilot suggestions.

**State Isolation**: Each pane must have isolated React render boundaries. Typing in an arrangement amount or receiving streamed AI tokens must never trigger a re-render of the WebRTC audio bar or call disposition dropdown.

### 3. Strict Disposition & Call Wrap-Up State Machine
At the conclusion of a call, the dialler prevents the agent from receiving a new call until a mandatory **disposition code** is logged (e.g. `PTP_TAKEN`, `DEBTOR_REFUSED`, `WRONG_NUMBER`, `DISPUTE_RAISED`).
* The UI must enforce this transition deterministically:
  `ON_CALL` -> `CALL_ENDED` -> `WRAP_UP_MANDATORY` -> `IDLE_READY`.
