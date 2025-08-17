### 🟦 3.1. RabbitMQ

    RabbitMQ is a popular open-source message broker that implements the Advanced Message Queuing Protocol (AMQP).
    It acts as a middleman that routes, buffers, and delivers messages between producers (senders) and consumers
    (receivers) in a reliable and scalable way.

##### ⚙️ How RabbitMQ Manages Message Queuing

1. **Producer** – Sends messages to an exchange.
2. **Exchange** – Routes messages to queues based on routing rules.
3. **Queue** – Temporarily stores messages until consumed.
4. **Consumer** – Receives and processes messages from queues.
5. **Bindings** – Link exchanges to queues with routing keys.
6. **Acknowledgments** – Ensure reliable message delivery and reprocessing.

###### 🧰 RabbitMQ Features for Managing Queues

- **Durable Queues** – Persist messages even after broker restarts.
- **Dead Letter Queues (DLQ)** – Route failed messages for later analysis.
- **Message TTL** – Set expiration time for messages in a queue.
- **Priority Queues** – Deliver higher priority messages first.
- **Prefetch Limit** – Limit unacknowledged messages per consumer.

##### ✅ Exchange Types (Routing Rules):

| Exchange Type | Description                                        | Example Use                                |
| ------------- | -------------------------------------------------- | ------------------------------------------ |
| **Direct**    | Routes to queues with exact routing key match.     | Notify one queue based on message type.    |
| **Fanout**    | Broadcasts message to **all** bound queues.        | System-wide notifications, logs.           |
| **Topic**     | Routes based on wildcard patterns in routing keys. | Complex routing like `order.*.email`.      |
| **Headers**   | Routes based on message headers instead of keys.   | Dynamic filtering (e.g., by region, type). |

##### 🚀 Use Cases of RabbitMQ

- **Task Queues** – Handle background jobs like email or image processing.
- **Microservices** – Enable asynchronous communication between services.
- **Load Buffering** – Absorb high traffic to prevent system overload.
- **Reliable Event Delivery** – Ensure message delivery even on failure.
- **IoT Messaging** – Route and manage messages from devices/sensors.
- **Order Processing** – Queue customer orders for backend workflows.

##### 🔄 Message Flow Example(RabbitMQ):

Producer → Exchange → [Binding] → Queue → Consumer

---
