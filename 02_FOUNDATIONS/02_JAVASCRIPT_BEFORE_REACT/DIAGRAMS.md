# Lesson 2: Execution & Event Loop Diagrams

## 1. Closure Lexical Memory Retention
```text
HEAP MEMORY
┌────────────────────────────────────────┐
│ Lexical Environment (createCounter)    │
│ let count = 0                          │ <── Closure reference
└────────────────────────────────────────┘
                    ▲
                    │
┌───────────────────┴────────────────────┐
│ Anonymous function () => ++count       │
│ Identifier: counter                    │
└────────────────────────────────────────┘
```

## 2. Event Loop Execution Order
```text
Synchronous Call Stack ──► 1, 4 (Logs immediately)
         │
         ▼ (Stack Empty)
Drain Microtask Queue  ──► 3 (Promise.then callback)
         │
         ▼ (Render opportunity)
Execute Next Macrotask ──► 2 (setTimeout callback)
```
