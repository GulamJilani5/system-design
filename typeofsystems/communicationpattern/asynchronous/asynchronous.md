🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Asynchronous Communication

## ➡️ 1. WebClient

- WebClient is a non-blocking, reactive HTTP client introduced in `Spring 5` as part of the `Spring WebFlux` framework. - It is designed for making asynchronous and reactive HTTP requests in a reactive programming model, suitable for modern, scalable applications.
- WebClient leverages Project Reactor's reactive types (`Mono` and `Flux`) to handle asynchronous data streams efficiently.

## ➡️ 2. Java 11 HTTP Client API (synchronous + asynchronous)

- **HttpClient:** The main entry point for sending requests and managing configurations.
- **HttpRequest:** Represents an HTTP request with method, URL, headers, and body.
- **HttpResponse:** Represents the server’s response, including status code, headers, and body.
- **BodyHandlers**
- **BodyPublishers**

## ➡️ 3.1. Java ExecutorService

- Pure Java(**Core Java / JDK level**) concurrency utility (since Java 5, `java.util.concurrent`).
- Detailed explanation is find in the **java-fundamentals` multithreading**
- It is high level concurrency utility
- It abstract away the complexity of thread management, making usage of threads easier.
- Provides a way to manage and execute task asynchronously in a pool of threads.

## ➡️ 3.2. Background / Async Tasks - @Async + TaskExecutor

- Built on top of `ExecutorService`.
- Spring manages the lifecycle of the thread pool for you.
- **@Async + TaskExecutor** → used selectively, usually for **legacy/blocking** integrations.
- Parallelizing blocking calls
- Background processing in the same service
- Deal with blocking calls or background fire-and-forget tasks inside the service.

##### 🔵@Async

- Find More `D:\Jilani\learning\spring boot\spring-framework\springframework-concepts_4.md`
- **Package:** `org.springframework.scheduling.annotation.Async`
- Introduced in **Spring 3.0** (around 2009).
- Works with any Java version starting from Java 5+ (because it relies on java.util.concurrent.Future which was  
  added in Java 5).

##### 🔵TaskExecutor

- **Package:** `org.springframework.core.task.TaskExecutor`
- Introduced even earlier (**Spring 2.x era**).
- It’s Spring’s abstraction over Java’s ExecutorService.
- Implementations like ThreadPoolTaskExecutor are widely used in Spring Boot.
- Commonly used in Spring Boot 1.x, 2.x, 3.x (still valid today).

## ➡️ 4. (Message Broker or Broker) = (RabbitMQ, Apache Kafka)

- A Message Broker is a middleware that facilitates communication between different services or applications by
  translating messages between messaging protocols, routing, and queuing.
- **Event Broker** is a type of message broker specialized for event-driven architecture (**EDA**).

##### 🔵 Why called Broker ?

- Because like a broker in real life, RabbitMQ sits in between two parties (producer and consumer),
  managing and facilitating the exchange of data without the two needing to know about each other directly.

##### 🔵 What type of data it can manage(Receive & Send)?

- JSON, XML, Text, Binary Files, Java Objects.

##### 🔵 Streaming

- Continuous real-time flow of data that is processed sequentially, often in small chunks or events.
- **e.g.:** Kafka, AWS → Amazon Kinesis

### 🟦 4.1. RabbitMQ

- **RabbitMQ** is a popular open-source message broker that implements the **Advanced Message Queuing Protocol (AMQP).**
- It acts as a middleman that routes, buffers, and delivers messages between producers (senders) and consumers
  (receivers) in a reliable and scalable way.

### 🟦 4.2. Apache Kafka

- Kafka is a **distributed event streaming platform** that serves as a high-throughput **message broker**.
- It supports both **real-time streaming** and **message queuing** via **topics** and **consumer groups**.
- **Topics** are logs of messages divided into **partitions** for scalability and parallelism.
- Kafka enables **decoupled, event-driven architectures** by allowing producers and consumers to operate  
  independently.
- Real-time processing is supported using tools like **Kafka Streams** and **ksqlDB**.

## ➡️ Event-Driven Architecture (EDA) (Ex: Gihub, React, Node.js)

Systems communicate by producing, detecting, and reacting to events.

An event is significant change in state (eg: orderPlaced, paymentDone etc.)

Producer publishes the event asynchronously to a message system.

Consumers subscribe to those events & read.

Independently decoupled from the producer.

(Correct state of an entity is event)

👉 Eg: Kafka, RabbitMQ

Act like a event Bus, and this event Bus distribute event by queueing & routing to producers.

#### Event-Driven Architecture (EDA)

| **Advantage**  | **Disadvantage**                                                                    |
| -------------- | ----------------------------------------------------------------------------------- |
| Availability   | Eventual consistency                                                                |
| Easy Roll-back | Not applicable to Gateways                                                          |
| Replacements   | Lesser control on services order                                                    |
| Transactions   | Difficult to reason about flow of data (system) _(Hidden flow → Developer problem)_ |
| Stores Intent  | Migration challenges                                                                |

## ➡️ Pub-Sub vs Message Queue (Asynchronous Event)

| **Aspect**           | **Pub-Sub**                                                               | **Message Queue**                                                          |
| -------------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Message Delivery** | One message → **multiple consumers**                                      | One message → **one consumer** (point-to-point)                            |
| **RabbitMQ**         | Pub-sub with **Topic/Fanout exchanges**                                   | Work queue or **Direct exchange**                                          |
| **Kafka**            | Uses **Topic** (best suited for pub-sub, mostly Kafka is used)            | Acts as a message queue using **Topic with a consumer group**              |
| **AWS**              | —                                                                         | **AWS SQS** (Simple Queue Service)                                         |
| **Use Case**         | Broadcasting events (e.g., notifications, logs, real-time data pipelines) | Task distribution (e.g., order processing, background jobs, worker queues) |
