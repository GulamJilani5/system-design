🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Event-Driven Architecture (EDA)

## ➡️ What is it?

- A software design pattern where systems react to events (something happening in the system).
- **Example:** When you place an order → an "OrderPlaced" event is generated → other services (like payment, shipping, notification) react to it.

## ➡️ Key Ideas

- **Event:** A significant occurrence (e.g., "UserRegistered", "PaymentCompleted").
- **Producer:** Creates (publishes) events.
- **Consumer:** Listens for (subscribes to) events and reacts.
- **Event Broker:** A middle system (like Kafka, RabbitMQ) that delivers events from producers to consumers.

## ➡️ Why use it?

- **Decoupling:** Services don’t directly depend on each other.
- **Asynchronous:** No need to wait for immediate responses → improves performance.
- **Scalability:** Easy to add new services reacting to the same event.

## ➡️ Common Problems Solved

- Avoid **Temporal Coupling**.
  - Temporal coupling happens when Service A can only work if Service B is available right now.
    - **Service A** calls **Service B** synchronously (expects an immediate response).
    - If **Service B** is slow or down → **Service A** is blocked or fails.
- With events, **A** just publishes and moves on. **B** can process later.

## ➡️ How to build it?

- Use async communication with tools like Kafka, RabbitMQ.
- In **Java** (**Spring Boot**): Use **Spring Cloud Stream**, **Event Brokers**, **Spring Cloud Function** to publish/consume events.

## ➡️ Pub/Sub vs Event Streaming

| Feature              | **Publisher/Subscriber Model (RabbitMQ)**                            | **Event Streaming Model (Kafka)**                                   |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Event handling**   | Events are pushed to subscribers. Once consumed, they’re gone.       | Events are stored in a log. Consumers can read them multiple times. |
| **Replayability**    | ❌ Events cannot be replayed. If you miss it, it’s lost.             | ✅ Events can be replayed anytime (since stored in a log).          |
| **New subscribers**  | New subscribers only get new events, not past ones.                  | New subscribers can consume past + new events.                      |
| **Use cases**        | Real-time messaging, notifications, task queues, short-lived events. | Event history, analytics, auditing, streaming pipelines, big data.  |
| **Message delivery** | Point-to-point (queues) or broadcast (topics).                       | Sequential event log (partitioned topics).                          |
| **Durability**       | Focused on delivery guarantee at that moment.                        | Focused on storing + replaying events for long-term.                |
| **Popular tool**     | RabbitMQ                                                             | Apache Kafka                                                        |

### Easy way to answer in the interview

- **RabbitMQ** is good for real-time messaging where you only care about current events (like sending notifications).
- **Kafka** is better for event streams where you need history, replay, and large-scale data pipelines (like processing clickstreams or transactions).
- **RabbitMQ** delivers messages to subscribers and forgets, while **Kafka** stores events in a log so they can be replayed anytime.
