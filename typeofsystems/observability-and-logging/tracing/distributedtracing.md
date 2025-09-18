🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Distributed Tracing in Microservices

Event logs, health probes, and metrics provide valuable insights into the internal condition of an application.  
However, these sources alone fall short in capturing the distributed nature of cloud-native applications.  
Since a single user request often traverses multiple services, it becomes challenging to **correlate data across application boundaries**.

### 🟦 **Distributed tracing** is a technique used in microservices or cloud-native applications to:

- Track the flow of requests across multiple services and components.
- Provide visibility into how requests are processed.
- Identify performance bottlenecks.
- Diagnose issues in complex, distributed systems.

---

## ➡️ Key Solution

A common solution is to generate a **Correlation ID** at the entry point of the system.

- This Correlation ID is propagated across all services handling the request.
- It allows linking of logs, events, and traces associated with the same transaction.
- By leveraging it, developers can track and analyze a request end-to-end across multiple services.

---

## ➡️ Core Concepts of Distributed Tracing

### 🟦 1. Tags

- Metadata that adds context to a span.
- Examples include:
  - Request URI
  - Username of the authenticated user
  - Tenant identifier

---

### 🟦 2. Trace

- Represents the **entire lifecycle of a request or transaction**.
- Identified by a **Trace ID**.
- Contains multiple spans, covering the full journey across various services.

---

### 🟦 3. Span

- Represents a **single operation** or stage within a request lifecycle.
- Contains start and end timestamps.
- Identified uniquely by the combination of:
  - **Trace ID**
  - **Span ID**

---

✅ In summary, **Trace = full request**, **Span = single step**, **Tags = extra details**.

## ➡️ Sleuth + Zipkin

### 🟦 Sleuth

- **Spring Cloud Sleuth:** this project has been deprecated and only older systems are using it.
- Adds **trace IDs** & **Span IDs** to logs and propagates them between services.
- **Sleuth = Tracer (producer)**

```java
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-sleuth</artifactId>
    </dependency>
```

### 🟦 Zipkin

- Tool provided by the Twitter.
- Collects those(Produced by Spring cloud Sleuth) traces(**trace IDs** & **Span IDs**) and shows them in a nice dashboard.
- **Zipkin = Collector + UI (consumer/visualizer)**

```java
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-sleuth-zipkin</artifactId>
    </dependency>
```

## ➡️ Modern Solution For distributed Tracing

### 🟦 Micrometer

- It's only used with java

### 🟦 OpenTelemetry

- It's been used with many languages

### 🟦 Storage and Query Layer (Backend)

This is where **Tempo** fits—as the backend for **storing**, **querying**, and **analyzing traces**.
