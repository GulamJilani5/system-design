### 🟦 3.2. Apache Kafka

- Kafka is a **distributed event streaming platform** that serves as a high-throughput **message broker**.
- It supports both **real-time streaming** and **message queuing** via **topics** and **consumer groups**.
- **Topics** are logs of messages divided into **partitions** for scalability and parallelism.
- Kafka enables **decoupled, event-driven architectures** by allowing producers and consumers to operate independently.
- Real-time processing is supported using tools like **Kafka Streams** and **ksqlDB**.

##### 🧰 Kafka Features for Managing Queues

- **Durable Storage** – Messages are written to disk and replicated across brokers.
- **Scalability** – Topics can be partitioned and spread across multiple brokers.
- **Retention Policies** – Keep messages for a configurable time, even if consumed.
- **Consumer Groups** – Scale consumers horizontally for load balancing.
- **Exactly Once Semantics** – Prevents duplicate message processing.
- **High Throughput** – Handles millions of messages per second with low latency.

##### 🔹 Key Points About Kafka Topics

- **Topic as Stream** – Acts as a category where producers send and consumers read messages.
- **Partitions** – Topics are split into partitions for horizontal scalability and parallel processing.
- **Message Order** – Order is guaranteed within a partition, not across partitions.
- **Retention** – Messages are stored for a set time and can be replayed using offsets.
- **Offsets** – Unique IDs track message position; consumers use them to resume processing.
- **Decoupling** – Producers and consumers are independent; consumers can scale via groups.

##### 🚀 Use Cases of Kafka

- **Real-Time Analytics** – Process clickstream or IoT data in real-time.
- **Microservices Communication** – Event-driven architecture with decoupled services.
- **Log Aggregation** – Collect logs from different services into one pipeline.
- **Data Pipeline** – Stream data between databases, systems, or services.
- **Monitoring & Alerting** – Send and process metrics for alerting tools.
- **Order/Event Tracking** – Track user activity, orders, or inventory events reliably.

##### 🔄 Message Flow Example (Apache Kafka):

Producer → Topic → [Partition] → Consumer Group → Consumer

---

# 🟦 Kafka vs. Traditional Brokers (like RabbitMQ):

| Feature               | **Kafka**                             | **RabbitMQ** (Traditional Broker)         |
| --------------------- | ------------------------------------- | ----------------------------------------- |
| **Model**             | Distributed **log-based** pub/sub     | Message **queue** with exchanges & queues |
| **Use Case**          | High-throughput, real-time streaming  | Task queues, background jobs              |
| **Message Retention** | Configurable (e.g., 7 days, forever)  | Messages deleted after consumption        |
| **Storage**           | Persists messages to disk (durable)   | Optional persistence (default in-memory)  |
| **Order Guarantee**   | Per-partition                         | No strict guarantee across consumers      |
| **Scalability**       | Extremely scalable (partitioned logs) | Limited vertical scaling                  |
| **Consumer Model**    | Pull-based                            | Push-based                                |
| **Built for**         | Streaming, Big Data, Microservices    | Simple messaging and queuing              |
