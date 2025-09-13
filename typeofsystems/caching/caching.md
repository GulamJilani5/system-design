🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Caching:

Reduce new calls.
Avoid repeated computations.
Reduce DB load.
Scale independently:
Deployments are independent.
Multiple results
Can use same code, all the logic is same.

### ➡️ Communication Flow with Caching(Request-Response)

##### 🟦 1. User → Server

- **Action:** User sends a request.
- **Time taken:** ~50 ms (request + response cycle).

---

##### 🟦 2. Server → Cache (Redis or Distributed Cache like SSD/In-Memory)

- **Action:** Server first checks if data exists in cache.
- **If data found:** Response returned quickly.
- **Time taken:** ~1 ms (super fast).

---

##### 🟦 3. If Cache Miss → Server → DB

- **Action:** If data not present in cache, server queries the database.
- **Query execution time:** ~10 ms.
- **Response:** Database response goes back to server.

---

##### 🟦 4. DB → HDD (if needed)

- **Action:** If DB needs to fetch data from disk (HDD).
- **Note:** This is costly & slow compared to cache.

---

##### 🟦 5. Optimizations

- DB queries optimized with **indexing**.
- Small cache used for **frequent queries (hot data)**.

---

##### 🟦 6. Response

- Once data is retrieved (either from **Cache** or **DB**),
- The **Server** sends back the **response to the User**.

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
