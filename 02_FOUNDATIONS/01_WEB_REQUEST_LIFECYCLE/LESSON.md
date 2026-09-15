# Lesson 1: The Lifecycle of a Web Request

## Governing Question
*What actually happens when an agent or customer enters a URL (e.g. `https://portal.nutun.com/cases/123`) and hits Enter?*

---

## The Journey
```text
User Action (Enter)
       ↓
1. URL Parsing (Scheme, Hostname, Path, Port)
       ↓
2. DNS Resolution (Browser cache → OS cache → Recursive resolver → Authoritative name server)
       ↓
3. Connection Establishment (TCP 3-Way Handshake + TLS 1.3 Negotiation / QUIC in HTTP/3)
       ↓
4. HTTP Request & Server Processing (Headers, Cookies, Load Balancer, Reverse Proxy, Microservices)
       ↓
5. Critical Rendering Path (Byte stream → Tokenizer → DOM Tree + CSSOM Tree → Render Tree)
       ↓
6. Layout & Paint (Geometry calculation → Rasterization → GPU Compositing)
       ↓
7. JavaScript Runtime & Event Loop (Call Stack, Microtasks, Tasks, Event handling)
       ↓
8. React Mounting & Reconciliation (Fiber tree construction, DOM mutations, UI = f(state))
```
