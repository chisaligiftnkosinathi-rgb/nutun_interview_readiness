# Nutun Architecture: Public Documented Facts vs Inferred vs Unknowns

> **THE INTERVIEW PRINCIPLE**: In a senior technical interview, the fastest way to lose credibility is to state an unverified guess as an established corporate fact. Conversely, candidates who clearly demarcate verified corporate disclosures from sound architectural deductions demonstrate elite intellectual maturity.

---

## 1. Architectural Demarcation Matrix

| Architectural Domain | Documented Public Fact `[DOCUMENTED]` | Reasonable Deduction `[INFERRED]` | Unknown Internal Detail `[UNKNOWN]` |
| :--- | :--- | :--- | :--- |
| **Scale & Headcount** | 5,290 employees; 5,000+ seat physical infrastructure; marketing scale to 10,000 agents; 18.5M+ monthly interactions; 2.4M distressed consumers managed annually. | Scaled multi-region deployment across South Africa, UK, Australia, US. | Exact concurrent active agent logins during peak morning vs afternoon shifts. |
| **Data Universe (MDU)** | Proprietary database covering 27 million South African credit-active debtors; used to price portfolios and drive collection models. | Stored in distributed data lakes/warehouses with read-optimized caching layers for UI consumption. | Exact database engine (PostgreSQL, Snowflake, Databricks, BigQuery, or internal legacy SQL Server). |
| **Collections Platform** | Proprietary "Cheetah" collections CRM software engineered for debt recovery and consumer rehabilitation. | Front-end built or migrated using modern React component hierarchies, TypeScript, and state management. | Proprietary microservice APIs, table schemas, or internal Cheetah service endpoints. |
| **AI & Innovation** | Dedicated Technology & Innovation Lab in Durban backed by R100M+ capital commitment; "Zoey" autonomous agentic AI; 100% Auto-QA on speech. | Streaming LLM integrations using SSE/WebSockets; RAG retrieval over internal compliance documentation. | Foundational LLM provider (OpenAI, Anthropic, AWS Bedrock, or self-hosted open-weights models like Llama). |
| **Payment Rails** | Integration with South African authenticated collections (DebiCheck, NAEDO) and digital payment channels. | Client-side idempotency keys and strict state machines guarding debit order mandate creation. | Specific banking payment gateway vendor contracts or proprietary clearing rails. |

---

## 2. How to Frame Answers in the Interview

When discussing Nutun's technology during the interview, Gift should use phrasing that reflects this exact discipline:

### Scenario 1: Discussing Scale & Performance
> *"From Nutun's 2025 integrated reporting, we know the platform supports over 18.5 million monthly interactions across 5,000+ physical seats and up to 10,000 agents. While I don't know the private internal metrics for call duration, from a systems engineering perspective, even a 5-second UI freeze on screen-pop creates massive operational drag across thousands of agents."*

### Scenario 2: Discussing AI & 'Zoey'
> *"Publicly, Nutun highlights Zoey as an autonomous collections agent and emphasizes real-time speech analytics from the Durban Innovation Lab. On the frontend, whether the underlying model is hosted on AWS or OpenAI, my architectural responsibility is ensuring that streaming tokens arrive over Server-Sent Events without locking the main thread, and that legal citations are strictly grounded and auditable by the agent."*

### Scenario 3: Discussing Cheetah & The Master Data Universe
> *"Given that Nutun's 27-million debtor Master Data Universe fuels both portfolio pricing and live agent interactions, the front-end must consume a clean, read-optimized projection. In Cheetah, our forms cannot be simple generic inputs; they must enforce strict financial rounding in integer cents and prevent duplicate DebiCheck submissions through robust idempotency keys."*
