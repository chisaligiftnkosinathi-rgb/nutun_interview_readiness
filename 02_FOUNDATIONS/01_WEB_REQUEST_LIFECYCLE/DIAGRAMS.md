# Lesson 1: Architectural & Rendering Diagrams

## 1. Network & Protocol Handshake
```text
Client                                Server
  │                                     │
  │───────────── TCP SYN ──────────────►│ (1 RTT TCP)
  │◄─────────── SYN + ACK ──────────────│
  │───────────── TCP ACK ──────────────►│
  │                                     │
  │────────── Client Hello ────────────►│ (1 RTT TLS 1.3)
  │◄── Server Hello + Certificate ──────│
  │                                     │
  │======== ENCRYPTED PIPE OPEN ========│
  │──────────── HTTP GET ──────────────►│
  │◄─────────── HTTP 200 ──────────────│ (TTFB begins)
```

## 2. Browser Critical Rendering Path
```text
HTML Bytes ──► Tokens ──► DOM Tree ────────┐
                                           ├─► Render Tree ──► Layout (Reflow) ──► Paint ──► Composite
CSS Bytes  ──► Tokens ──► CSSOM Tree ──────┘
```

## 3. JavaScript Event Loop Priority
```text
┌────────────────────────┐
│       CALL STACK       │ (Executes current synchronous code)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│    MICROTASK QUEUE     │ (Promises, queueMicrotask)
│  * Emptied COMPLETELY  │
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│     RENDER QUEUE       │ (requestAnimationFrame, Style, Layout, Paint)
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│     MACROTASK QUEUE    │ (setTimeout, I/O, clicks, network events)
│  * Runs 1 task per turn│
└────────────────────────┘
```
