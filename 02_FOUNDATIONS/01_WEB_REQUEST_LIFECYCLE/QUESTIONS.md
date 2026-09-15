# Lesson 1: Checkpoint Questions

Answer these questions in your own words in `ANSWERS.md`:

### Question A: The Payment Button Freeze
An agent clicks **Submit Payment**. The button does not update to show "Loading..." or a spinner for 700 ms.
*Explain exactly what could be happening between `setLoading(true)` and the browser actually displaying the spinner.*

### Question B: The Rendering Pipeline Vocabulary
*In your own words, explain the exact difference and relationship between:*
1. HTML
2. DOM
3. CSS
4. CSSOM
5. Render Tree
6. Layout
7. Paint
8. Composite

### Question C: The High-Level Pipeline
*Explain this end-to-end chain and how each step handsoff to the next:*
```text
DNS → TCP/QUIC → TLS → HTTP → HTML → Browser → JavaScript → React
```

---

### Stage 2 AI Tutor Checkpoint: Sequence Validation
*What exact sequence of technical steps must occur before an HTTP request can reach the application server?*

---

### Stage 3 Timed Interview Defense: TTFB Decomposition & Diagnosis
*Explain TTFB (Time to First Byte): what components make up that number, and how do you determine whether a 2-second TTFB is caused by the client's network, DNS, TLS negotiation, or backend server execution?*

### Follow-Up Curveball: The 1,982ms Infrastructure Mystery
*If DNS, TCP, and TLS are 0ms (HTTP/2 reuse), 'Waiting for server response' is 2,000ms, but backend APM shows database and controller logic took only 18ms total, where did the remaining 1,982ms go between the browser and application server?*

