🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Bulkhead

- Bulkhead Pattern = Failure Isolation (NOT performance improvement)
- **In ships**, a bulkhead is a wall that divides the ship into separate compartments. If one compartment floods, the other compartments are still safe. This prevents the entire ship from sinking.
- **In the context of Resilience4j**, a bulkhead is a resilience pattern used to limit the number of concurrent calls to a specific component or service to prevent resource exhaustion and maintain system stability.

- **Bulkhead** = isolate resources so one failure doesn’t affect all.

### ➡️ Use Cases

- Limiting API calls to an external service to avoid exceeding rate limits.
- Protecting a database from too many concurrent queries.
- Isolating a slow or unreliable microservice in a distributed system.

### ➡️ In Resilience4j, the Bulkhead pattern supports two distinct implementations

#### 🟦 SemaphoreBulkhead:

- Does not use a thread pool.
- Uses a semaphore to limit the number of concurrent calls.
- Suitable for `CPU-bound` or `synchronous` operations where you want to restrict concurrent access without managing threads explicitly.
- Configurable parameters include `maxConcurrentCalls` (maximum concurrent executions) and `maxWaitDuration` (time a call can wait if the limit is reached).

#### 🟦 ThreadPoolBulkhead:

- Uses a thread pool.
- Designed for `asynchronous` or `I/O-bound` operations, such as calls to external services or databases.
- Maintains a fixed-size thread pool and a bounded queue for tasks, ensuring that only a limited number of threads are used for the operation.
- Configurable parameters include `maxThreadPoolSize` (maximum threads), `coreThreadPoolSize` (minimum threads kept alive), `queueCapacity` (number of tasks that can wait in the queue), and `keepAliveDuration` (time idle threads are kept alive).

### ➡️ Problem : Cascading Failure (Why your whole app goes down)

- Default behavior (Spring Boot + Tomcat)
- Tomcat thread pool ≈ 200 threads (shared)
- Every request (payment, inventory, health, etc.) uses the SAME pool

##### 🟦 What happens when Payment service is slow?

- Payment API becomes slow (DB / external API / network issue)
- Threads get blocked waiting for response
- Gradually:
  - All 200 threads become busy (blocked)

##### 🟦 Solution: Bulkhead Pattern

- Divide resources (threads) per dependency
- Instead of `One shared pool → 200 threads`
- Use:

```text
Payment Pool      → 50 threads
Inventory Pool    → 50 threads
Notification Pool → 30 threads
Health Pool       → 10 threads
```

- What happens now?
- If Payment is slow:
  - Only 50 threads blocked
  - Remaining threads still free
- So:
  - Inventory still works
  - Notifications still work
  - Health endpoint responds
  - Kubernetes does NOT restart pod

##### 🟦 How to implement in Spring Boot

- ###### 🔵 Option 1: Using Resilience4j (Recommended)

```java
@Bulkhead(name = "paymentService", type = Bulkhead.Type.THREADPOOL)
public String callPayment() {
    return restTemplate.getForObject("http://payment/api", String.class);
}
```

- Config:

```yaml
resilience4j.thread-pool-bulkhead:
  instances:
    paymentService:
      coreThreadPoolSize: 10
      maxThreadPoolSize: 20
      queueCapacity: 50
```

- ###### 🔵 Option 2: Separate ExecutorService

```java
ExecutorService paymentExecutor = Executors.newFixedThreadPool(50);
ExecutorService inventoryExecutor = Executors.newFixedThreadPool(50);
```

- ###### 🔵 Option 3: WebClient (Non-blocking - BEST for scaling)
- Instead of blocking threads:
- Uses event-loop (fewer threads, non-blocking)

```java
WebClient.create()
    .get()
    .uri("http://payment/api")
    .retrieve()
    .bodyToMono(String.class);
```

Instead of blocking threads:
