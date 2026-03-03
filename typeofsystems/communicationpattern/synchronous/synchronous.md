🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕ 🔵 🟦 ➡️ ⏺️

# ⏺️ Synchronous Communication = (RestTemplate, RestTemplate, Feign Client)

- one microservice sends a request to another and waits (blocks) until it receives a response.

### ➡️ 1. RestTemplate

- **RestTemplate** is a synchronous HTTP client provided by Spring to make HTTP requests to other services.
  It’s a low-level, flexible API for performing RESTful communication.

### ➡️ 2. Rest Client

- Modern alternate of **RestTemplate**(Replacement of `RestTemplate`).
- **RestClient** itself is synchronous by nature, but it can internally use **WebClient** (`reactive engine`) and therefore can perform non-blocking async calls if configured.

### ➡️ 3. Feign Client

Feign Client is a **declarative HTTP client**, part of **Spring Cloud**, that simplifies REST API calls by using
annotations to define client interfaces. It abstracts much of the boilerplate code required in RestTemplate.

### ➡️ 4. Java 11 HTTP Client API (synchronous + asynchronous)

- **HttpClient:** The main entry point for sending requests and managing configurations.
- **HttpRequest:** Represents an HTTP request with method, URL, headers, and body.
- **HttpResponse:** Represents the server’s response, including status code, headers, and body.

## ⏺️Key Considerations

- **Synchronous Nature:** Both RestTemplate and Feign Client block the calling thread, which can lead to performance bottlenecks in high-latency or high-volume scenarios.
- **Scalability:** Feign Client is better suited for microservices ecosystems due to its integration with service discovery and resilience patterns.

## ⏺️RestTemplate vs Feign Client Comparison

| Feature           | RestTemplate                     | Feign Client                              |
| ----------------- | -------------------------------- | ----------------------------------------- |
| Type              | Low-level, programmatic          | Declarative, annotation-based             |
| Ease of Use       | More boilerplate                 | Less code, cleaner interface              |
| Flexibility       | Highly customizable              | Less flexible, abstraction-focused        |
| Spring Cloud      | No dependency                    | Requires Spring Cloud dependencies        |
| Service Discovery | Manual URL configuration         | Native support for Eureka, Consul, etc.   |
| Error Handling    | Manual implementation            | Built-in support via Hystrix/Resilience4j |
| Maintenance       | Deprecated in favor of WebClient | Actively maintained                       |
| Use Case          | Simple, ad-hoc HTTP calls        | Microservices with service discovery      |
