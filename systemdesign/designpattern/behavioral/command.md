⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Command Pattern

- This design falls under Behavioral patterns. It is primarily similar to the Command pattern, where the request (Query) is encapsulated as an object and executed via a handler.
- It can also resemble Strategy pattern depending on how different handlers are implemented.

### ➡️ V execute(Q, var1)

- This Pattern used in our Project(KMP)
- This usually represents a generic command/handler pattern (often seen in CQRS / Service Layer / Strategy-like designs).

```java
V execute(Q query, Var1 var1)
```

- Q → Input (Query / Request / Command)
- V → Output (Response / Result)
- var1 → Execution context / helper / dependency
  - ogged-in user, Unique ID, Auth info

##### 🟦 execute() → Orchestrator method

- Orchestrator Layer, Inside it, We typically do:
  - Validation
  - Business logic
  - DB calls
  - External API calls
  - Mapping DTO → Entity
