🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Caching:

Reduce new calls.
Avoid repeated computations.
Reduce DB load.
Scale independently:
Deployments are independent.
Multiple results
Can use same code, all the logic is same.

### ➡️ cache placement:

- Client device
- Distributed caching (caching DB data)
- In-memory cache (caffeine) - Data lost on restart
- DB cache (automatic caching)

### ➡️ cache policy:

LRU → least recently used

LFU → least frequently used

### ➡️ Eventual Consistency

When data is updated in one place (e.g., DB), the cache might not reflect the update immediately. Over time cache will ‘catch up’ & become consistent with source data.

### ➡️ Cacheable
