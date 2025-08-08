🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️
☑️ • ‣ → ⁕

# ⏺️ Asynchronous Communication

## ➡️ 1. WebClient

- WebClient is a non-blocking, reactive HTTP client introduced in Spring 5 as part of the Spring WebFlux framework. - It is designed for making asynchronous and reactive HTTP requests in a reactive programming model, suitable for
  modern, scalable applications.
- WebClient leverages Project Reactor's reactive types (`Mono` and `Flux`) to handle asynchronous data streams efficiently.

#### 🟦Features

- Non-Blocking and Reactive:
- Client-Side Load Balancing:🔴🔴
- WebSocket Support:🔴🔴
- Functional API:
- Customizable
- Error Handling:
- Streaming Support:
- Retry and Timeout Support:

#### 🟦 Dependency (pom.xml)

```java
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
  </dependency>
```

#### 🟦 How use Webclient

##### 🔵 Example

```java
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

public class WebClientExample {

    // Define a simple User class
    static class User {
        private Long id;
        private String name;
        private String email;

        // Getters and setters
        public Long getId() { return id; }
        public void setId(Long id) { this.id = id; }
        public String getName() { return name; }
        public void setName(String name) { this.name = name; }
        public String getEmail() { return email; }
        public void setEmail(String email) { this.email = email; }

        @Override
        public String toString() {
            return "User{id=" + id + ", name='" + name + "', email='" + email + "'}";
        }
    }

    public static void main(String[] args) {
        // Create WebClient instance
        WebClient webClient = WebClient.create("https://api.example.com");

        // Make a GET request to fetch a user
        Mono<User> userMono = webClient.get()
                .uri("/users/1") // Specify the endpoint
                .header("Authorization", "Bearer your-token") // Add headers if needed
                .retrieve() // Execute the request
                .bodyToMono(User.class) // Map response to User class
                .onErrorResume(e -> {
                    // Handle errors (e.g., 404, 500)
                    System.err.println("Error occurred: " + e.getMessage());
                    return Mono.just(new User()); // Fallback to empty User
                });

        // Subscribe to the Mono to process the result
        userMono.subscribe(user -> {
            System.out.println("Fetched User: " + user);
        });

        // Block for demonstration purposes (avoid in production reactive apps)
        User user = userMono.block();
        System.out.println("Blocking call result: " + user);
    }
}

```

##### 🔵 Steps

- **Create a WebClient Instance:** Use `WebClient.create()` or `WebClient.builder()` to instantiate WebClient.
- **Build a Request:** Chain methods like `.get()`, `.post()`, `.uri()`, `.header()`, and `.body()` to construct the request.
- **Execute the Request:** Use `.retrieve()` or `.exchangeToMono()` to execute the request and process the response.
- **Handle the Response:** Use `Mono` or `Flux` to handle single or streaming responses, respectively.
- **Error Handling:** Use methods like `onStatus()` or `onErrorResume()` for handling errors.

## ➡️ 2. Java 11 HTTP Client API (synchronous + asyncchronous)

- **HttpClient:** The main entry point for sending requests and managing configurations.
- **HttpRequest:** Represents an HTTP request with method, URL, headers, and body.
- **HttpResponse:** Represents the server’s response, including status code, headers, and body.

```java

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HttpClientAsyncExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .header("Accept", "application/json")
            .GET()
            .build();

        CompletableFuture<HttpResponse<String>> futureResponse =
            client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

        futureResponse.thenApply(HttpResponse::body)
                      .thenAccept(body -> {
                          System.out.println("Response Body: " + body);
                      })
                      .exceptionally(ex -> {
                          System.err.println("Request failed: " + ex.getMessage());
                          return null;
                      });


          //No join() or manual delays in real-world Spring Boot apps.
        // Keep the program alive until async task finishes
        futureResponse.join();

         // Prevent the main thread from ending immediately
        // by keeping the program alive until async task finishes
        CompletableFuture.delayedExecutor(5, java.util.concurrent.TimeUnit.SECONDS)
                         .execute(() -> System.out.println("Main thread done."));

    }
}

```

- `sendAsync()` returns a `CompletableFuture` immediately instead of blocking until the request finishes.
- You can chain `.thenApply()` and `.thenAccept()` to process the result when it arrives.
- `exceptionally()` lets you handle errors without `try-catch`.

### In Real World Spring Boot

```java

// Controller
@RestController
public class ApiController {

    @Autowired
    private final ApiService apiService;

    @GetMapping("/data")
    public CompletableFuture<String> getData() {
        return apiService.fetchData();
    }
}

// Service
@Service
public class ApiService {
    private final HttpClient client = HttpClient.newHttpClient();

    public CompletableFuture<String> fetchData() {
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .header("Accept", "application/json")
            .GET()
            .build();

        return client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                     .thenApply(HttpResponse::body);
    }
}

```

## ➡️ 3. (Message Broker or Broker) = (RabbitMQ, Apache Kafka)

- A Message Broker is a middleware that facilitates communication between different services or applications by
  translating messages between messaging protocols, routing, and queuing.

##### 🔵 Why called Broker ?

- Because like a broker in real life, RabbitMQ sits in between two parties (producer and consumer),
  managing and facilitating the exchange of data without the two needing to know about each other directly.

##### 🔵 What type of data it can manage(Receive & Send)?

- JSON, XML, Text, Binary Files, Java Objects.

---

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

### ➡️ 4. Java 11 HTTP Client API (synchronous + asyncchronous)

- **HttpClient:** The main entry point for sending requests and managing configurations.
- **HttpRequest:** Represents an HTTP request with method, URL, headers, and body.
- **HttpResponse:** Represents the server’s response, including status code, headers, and body.

```java

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class AsyncHttpClientExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        String json = "{\"name\": \"John\", \"age\": 30}";
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/users"))
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(json))
            .build();

        CompletableFuture<HttpResponse<String>> future = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

        future.thenAccept(response -> {
            System.out.println("Status Code: " + response.statusCode());
            System.out.println("Response Body: " + response.body());
        }).exceptionally(throwable -> {
            System.err.println("Error: " + throwable.getMessage());
            return null;
        }).join(); // Wait for completion
    }

}

```
