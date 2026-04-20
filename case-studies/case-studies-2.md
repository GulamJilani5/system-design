⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Two microservices sharing the same database – is it a good approach?

### ➡️ Why it’s bad:

- Tight coupling
  - Both services depend on same schema → change in one breaks another
- No true independence
  - Cannot deploy services independently
- Schema conflicts
  - Different services may require different data models
- Scaling issues
  - Database becomes a bottleneck

### ➡️ Best Practice:

- Database per service
- Each microservice:
  - Owns its own DB
  - Exposes data via API only
- Example:
  - Order Service → Order DB
  - Payment Service → Payment DB
  - Communication via REST / Kafka

# ⏺️ Two microservices updating the same row simultaneously – how to handle it?

- This is a concurrency + consistency problem

### ➡️ Optimistic Locking (Most common)

- Use version field

```java
@Version
private int version;
```

##### 🟦 Flow:

- Service A reads version = 1
- Service B reads version = 1
- A updates → version becomes 2
- B tries update → fails (version mismatch)
- Prevents lost updates

### ➡️ Pessimistic Locking

- Lock row while updating

```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
```

- Safe but:
  - Slower
  - Can cause deadlocks

### ➡️ Event-driven / Queue-based updates (Best for microservices)

- Use Kafka / RabbitMQ
- Only one service updates:
  - Others send events
- Avoids race conditions completely

### ➡️ Distributed Locking

- Using Redis / Zookeeper
- Only one service allowed at a time

# ⏺️ If one microservice goes down – impact & handling

- Depends on architecture:
  - If tightly coupled → system failure
  - If loosely coupled → partial failure only

### ➡️ How to handle:

##### 🟦 Circuit Breaker Pattern

- Using tools like:
  - Resilience4j
  - Hystrix (Old)

- If service is down:
  - Stop calling it
  - Return fallback response

##### 🟦 Retry Mechanism

- Retry with backoff

```text
Retry → wait → retry → fail
```

##### 🟦 Fallback / Graceful Degradation

- Example:
  - Payment service down → show “Try later”
  - Recommendation service down → skip recommendations

##### 🟦 Bulkhead Pattern

- Isolate failures
- One service failure should not consume all resources

##### 🟦 Eventual Consistency

- Use async communication
- System continues even if one service is temporarily down

##### 🟦 Health Checks & Auto-restart

- Kubernetes / Docker
- Restart failed service automatically
