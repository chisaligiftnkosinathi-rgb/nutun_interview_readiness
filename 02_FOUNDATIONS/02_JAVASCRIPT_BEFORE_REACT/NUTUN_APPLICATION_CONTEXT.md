# Lesson 2: Nutun Operational Context

## Race Conditions in Customer Case Desks
At Nutun, contact center agents rapidly switch between customer accounts during outbound campaigns or live incoming queues.
* If an agent navigates from Customer A to Customer B while an AI case summary or credit bureau lookup for Customer A is still in-flight, unhandled asynchronous responses will overwrite Customer B's screen with Customer A's financial data.
* This is a **catastrophic operational failure**: a POPIA/privacy breach and incorrect financial advice.
* Frontend resilience requires:
  1. Aborting previous in-flight requests using `AbortController`.
  2. Ignoring stale asynchronous responses via effect cleanup flags or query keys (e.g. TanStack Query).
