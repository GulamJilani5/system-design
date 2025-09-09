🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Bulkhead

- **In ships**, a bulkhead is a wall that divides the ship into separate compartments. If one compartment floods, the other compartments are still safe. This prevents the entire ship from sinking.
- **In the context of Resilience4j**, a bulkhead is a resilience pattern used to limit the number of concurrent calls to a specific component or service to prevent resource exhaustion and maintain system stability.

- **Bulkhead** = isolate resources so one failure doesn’t affect all.

### ➡️ Use Cases

- Limiting API calls to an external service to avoid exceeding rate limits.
- Protecting a database from too many concurrent queries.
- Isolating a slow or unreliable microservice in a distributed system.

### ➡️ In Resilience4j, the Bulkhead pattern supports two distinct implementations

##### 🟦 SemaphoreBulkhead:

- Does not use a thread pool.
- Uses a semaphore to limit the number of concurrent calls.
- Suitable for `CPU-bound` or `synchronous` operations where you want to restrict concurrent access without managing threads explicitly.
- Configurable parameters include `maxConcurrentCalls` (maximum concurrent executions) and `maxWaitDuration` (time a call can wait if the limit is reached).

##### 🟦 ThreadPoolBulkhead:

- Uses a thread pool.
- Designed for `asynchronous` or `I/O-bound` operations, such as calls to external services or databases.
- Maintains a fixed-size thread pool and a bounded queue for tasks, ensuring that only a limited number of threads are used for the operation.
- Configurable parameters include `maxThreadPoolSize` (maximum threads), `coreThreadPoolSize` (minimum threads kept alive), `queueCapacity` (number of tasks that can wait in the queue), and `keepAliveDuration` (time idle threads are kept alive).
