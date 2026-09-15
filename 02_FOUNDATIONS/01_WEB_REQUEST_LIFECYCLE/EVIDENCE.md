# Lesson 1: Verified Evidence from My Code

## 1. Web Workers for Off-Thread Execution
* **Project**: `science-of-our-world`
* **Files**:
  * [`science-of-our-world/src/services/aiService.ts`](file:///c:/Projects/science-of-our-world/src/services/aiService.ts)
  * [`science-of-our-world/src/services/aiWorker.ts`](file:///c:/Projects/science-of-our-world/src/services/aiWorker.ts)
* **What was implemented**: Moved computationally heavy local AI execution into a dedicated Web Worker via `CreateWebWorkerMLCEngine`, preventing the browser main thread and React rendering loop from freezing during token generation.

## 2. Microtask / Promise Handling & Async State
* **Project**: `axis_clean`
* **Files**:
  * [`axis_clean/apps/web/package.json`](file:///c:/Projects/axis_clean/apps/web/package.json)
* **What was implemented**: TanStack Query v5 (`@tanstack/react-query`) for asynchronous server-state management, handling background fetching, query deduping, and microtask scheduling without blocking rendering.

## 3. High-Performance GPU Compositing & Animations
* **Project**: `axis_clean`
* **Libraries**: `framer-motion`, Tailwind CSS
* **What was implemented**: Leveraged GPU-accelerated CSS properties (`transform`, `opacity`) to animate UI elements smoothly at 60fps without triggering layout recalculations.
