⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Synchronous vs Asynchronous

### ➡️ Synchronous

- Client sends request → waits → gets response
- Blocking communication (caller is dependent on response)
- REST (HTTP/JSON)
- gRPC:
  - Use for high-performance internal communication
  - Efficient binary protocol and supports streaming

##### 🟦 Usage

- Used for real-time, request response interactions.
- Authentication.
- Payment processing.
- Calling(Fetching details of) another(external dependent) services.

###### 🔵 Real-Time Scenario (E-commerce)

- Payment Flow
- Order Service → Payment Service
- Use: Synchronous (REST/gRPC)
- Reason: Need immediate confirmation

##### 🟦 Challenges

- Network latency (slow response)
- Cascading failures (if one service fails → all fail)
- Tight coupling (services depend on each other)

##### 🟦 Solutions

- Timeouts
- Retries
- Circuit Breaker (e.g., Resilience4j)

### ➡️ Asynchronous

- Service sends message → does NOT wait, Other services processes later
-

##### 🟦 Usage

- Delay is acceptable
- Background tasks
  - Email notifications
  - Logging
  - Order processing pipeline

###### 🔵 Notification Flow

- OrderService publishes an event to queue → Notification Service(Inventory Service) consumes and processes it.
- Use: Asynchronous (Kafka/RabbitMQ)
- Reason: Can be delayed

###### 🔵 Benefits

- Loose coupling (services independent)
- Better scalability
- High resilience (failures don’t propagate immediately)

##### 🟦 Challenges

- Eventual consistency (data not immediately updated)
- Message duplication

##### 🟦 Solutions

- Idempotency (same request → same result)
- Message deduplication
- Dead Letter Queue (DLQ)

### ➡️ When to use which

- **Suppose for an e-commerce system:**
- login/signup(Authentication) and placing an order(OrderService -> PaymentService)
  - Use **Synchronous** REST/gPRC for immediate payment confirmation
- Sending order confirmation email(OrderService -> NotificationService)
  - Use **Asynchronous** messaging (Kafka/RabbitMQ)
