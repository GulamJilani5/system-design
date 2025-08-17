🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️
☑️ • ‣ → ⁕

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

## ➡️ 3. Java ExecutorService

- Detailed explanation is find in the **java-fundamentals` multithreading**
- It is high level concurrency utility
- It abstract away the complexity of thread management, making usage of threads easier.
- Provides a way to manage and execute task asynchronously in a pool of threads.
-

## ➡️ 4. Background / Async Tasks - @Async + TaskExecutor

- **@Async + TaskExecutor** → used selectively, usually for **legacy/blocking** integrations.
- Parallelizing blocking calls
- Background processing in the same service
- Deal with blocking calls or background fire-and-forget tasks inside the service.

##### 🔵@Async

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

## ➡️ 5. (Message Broker or Broker) = (RabbitMQ, Apache Kafka)

- A Message Broker is a middleware that facilitates communication between different services or applications by
  translating messages between messaging protocols, routing, and queuing.

##### 🔵 Why called Broker ?

- Because like a broker in real life, RabbitMQ sits in between two parties (producer and consumer),
  managing and facilitating the exchange of data without the two needing to know about each other directly.

##### 🔵 What type of data it can manage(Receive & Send)?

- JSON, XML, Text, Binary Files, Java Objects.

### 🟦 5.1. RabbitMQ

    RabbitMQ is a popular open-source message broker that implements the Advanced Message Queuing Protocol (AMQP).
    It acts as a middleman that routes, buffers, and delivers messages between producers (senders) and consumers
    (receivers) in a reliable and scalable way.

### 🟦 5.2. Apache Kafka

- Kafka is a **distributed event streaming platform** that serves as a high-throughput **message broker**.
- It supports both **real-time streaming** and **message queuing** via **topics** and **consumer groups**.
- **Topics** are logs of messages divided into **partitions** for scalability and parallelism.
- Kafka enables **decoupled, event-driven architectures** by allowing producers and consumers to operate independently.
- Real-time processing is supported using tools like **Kafka Streams** and **ksqlDB**.
