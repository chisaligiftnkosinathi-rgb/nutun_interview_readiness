# Lesson 1: Nutun Operational Context

## Why Browser Fundamentals Matter at Nutun Scale

### 1. The 18.5 Million Interaction Equation
Nutun handles an average of 18.5 million customer interactions monthly across up to 10,000 agents.
* If an agent workspace freezes for 500ms when opening a case or submitting a payment disposition, that half-second delay repeated 100 times a day across thousands of agents represents hundreds of lost operational hours monthly.
* Front-end performance at Nutun is directly tied to **operational efficiency and customer hold time**.

### 2. The Multi-Monitor Agent Workstation vs. Mobile Consumer Portal
* **The Contact Centre Agent**: Uses desktop monitors with complex, dense multi-pane interfaces (Cheetah CRM, telephony dialler status, live chat queue, customer history). Layout thrashing or unoptimized re-renders can freeze the agent's screen during a live customer call.
* **The Distressed Consumer**: Opens a debt review or self-service payment arrangement link on a budget smartphone over variable 3G/4G connectivity. Heavy, parser-blocking script bundles cause high bounce rates and failed self-service payment arrangements.
