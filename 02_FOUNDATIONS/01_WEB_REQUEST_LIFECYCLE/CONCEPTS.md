# Lesson 1: Technical Concepts & Definitions

### 1. DNS (Domain Name System)
Translates human-readable hostnames (`portal.nutun.com`) into machine-routable IP addresses. Operates with hierarchical caching and Time to Live (TTL) values.

### 2. TCP Handshake vs. QUIC
* **TCP (HTTP/1.1 & HTTP/2)**: Three-way handshake (`SYN` → `SYN-ACK` → `ACK`) to establish a reliable, ordered byte stream.
* **QUIC (HTTP/3)**: Runs over UDP, integrating TLS 1.3 directly into the transport handshake to achieve 0-RTT connection establishment without Head-of-Line blocking.

### 3. TTFB (Time to First Byte)
The latency from the client dispatching the HTTP request until the arrival of the first response byte. Reflects network distance, TLS negotiation, server routing, and backend database/microservice execution.

### 4. DOM vs. HTML
* **HTML**: Plain text serialized markup stream sent across the wire.
* **DOM**: Live in-memory tree of object nodes created by the browser's parser, exposeable to JavaScript via DOM APIs.

### 5. Render-Blocking Resources
* **CSS is render-blocking**: Browsers delay rendering until the CSSOM is constructed to prevent FOUC (Flash of Unstyled Content).
* **Synchronous JS is parser-blocking**: Halts HTML tokenization while the script downloads and executes, unless marked `defer`, `async`, or loaded as ES modules (`type="module"`).

### 6. Critical Rendering Pipeline
* **Layout (Reflow)**: Calculating bounding box dimensions (`x, y, width, height`) based on viewport geometry.
* **Paint**: Filling in raster pixels (text, colors, borders, shadows).
* **Composite**: Layer blending executed on the GPU thread.
