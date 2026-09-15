# React Lesson 1: Evidence & Readiness Status

## 1. Governance & Evidence Status
In accordance with [`00_GOVERNANCE/STUDY_CONSTITUTION.md`](file:///c:/Projects/nutun_interview_readiness/00_GOVERNANCE/STUDY_CONSTITUTION.md), technical readiness is categorized into three strict categories:
* 🟢 **I KNOW**: Direct, verifiable evidence from production code, monorepos, or hands-on implementations in `C:\Projects`.
* 🟡 **I UNDERSTAND**: Technically sound architectural understanding, but not personally implemented in production yet.
* 🔴 **I DON'T KNOW YET**: Identified gaps that we must study, test, or build.

---

## 2. Topic Readiness Matrix

| Concept / Capability | Status | Evidence Source | Notes |
| :--- | :---: | :--- | :--- |
| **Declarative UI vs Imperative DOM** | 🟢 KNOW | `science-of-our-world`, `axis_clean` | Replaced raw DOM event wiring with declarative component state. |
| **Component as Function Invocation** | 🟢 KNOW | Production React codebases | Clear understanding of render execution contexts and props immutability. |
| **React Element Tree vs DOM** | 🟢 KNOW | Build configurations, JSX compilation | Distinguishes lightweight JS objects from heavy C++ host DOM nodes. |
| **Fiber Architecture & Scheduling** | 🟢 KNOW | Architectural study & source inspection | Accurately models Fiber work units with `child`, `sibling`, `return` pointers. |
| **Render Phase vs Commit Phase** | 🟢 KNOW | React profiler & devtools verification | Knows render is in-memory calculation; commit is synchronous host mutation. |
| **Reconciliation Heuristics** | 🟢 KNOW | Dynamic list implementations | Understands type matching, child keys, and why React diffing is $O(n)$ heuristic. |
| **Stale Closures in React** | 🟢 KNOW | Async event handlers & streaming readers | Solved in practice using functional state updates and mutable `useRef` containers. |
| **Component Identity vs Security** | 🟢 KNOW | Financial UI state design | Knows `key` resets local UI state, but backend authorization remains authoritative. |

---

## 3. Project Evidence References

### Project 1: `science-of-our-world`
* **Path**: Located under `C:\Projects\science-of-our-world`
* **Evidence**:
  - Declarative React components orchestrate interactive physics and biological simulations.
  - UI control panels communicate parameter state cleanly to canvas rendering loops without imperative DOM spaghetti.
  - Strict separation between React's declarative state tree and WebGL/Canvas imperative rendering surfaces.

### Project 2: `axis_clean`
* **Path**: Located under `C:\Projects\axis_clean`
* **Evidence**:
  - Componentized clinical workflows, diagnostic test viewers, and data tables.
  - Extensive TypeScript integration (`strict: true`) ensuring component prop contracts match backend DTOs.
  - Proper key usage in dynamic collections, preventing incorrect state retention when switching between patient records.

---

## 4. Assessment Summary
* **React Foundations & Mental Model**: 🟢 **KNOW**
* No unsupported employment claims made; evaluated strictly on demonstrated codebase evidence and technical precision.
