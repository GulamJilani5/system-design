🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Spring Boot's resilience capabilities are primarily enabled through:

- **Hystrix** a library developed by **Netflix** were used earlier. As it entered maintenance mode in 2018 so currently **Resilience4j** is used. It is **reactive**(`Spring Webflux`).
- **Resilience4j:** A lightweight, functional-style library integrated with Spring Boot for fault tolerance. It's configurable via annotations and `YAML` properties, and supports **metrics** via `Spring Boot Actuator`.
- **Other Spring Cloud Tools:** Such as **Service Discovery (Eureka)**, **Load Balancing (Ribbon(Outdated) or Spring Cloud LoadBalancer)**, and API Gateways (`Spring Cloud Gateway`) that enhance resilience indirectly.

## ➡️ Resilience4j

This dependency provides Resilience4j's core libraries and Spring Boot-specific integrations, such as configuration via application.yml and Actuator metrics.

```java
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot3</artifactId>
</dependency>
```

## ➡️ Redis For Rate Limiting

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>
```

- The algorithm used is the Token Bucket Algorithm.

### 🟦 Below three concepts

- replenishRate
- burstCapacity
- requestedTokens

### 🟦 Example Scenario

Imagine an API with a token bucket:

- **replenishRate:** 5 tokens/second.
- **burstCapacity:** 20 tokens.
- **requestedTokens:** A client makes a request needing 8 tokens.

If the bucket has `10 tokens`, the request (`8 tokens`) is allowed, leaving `2 tokens`. The bucket then replenishes at `5 tokens/second` until it reaches the `burstCapacity` of `20 tokens`. If a request needs `25 tokens`, it’s denied unless the bucket accumulates enough tokens over time.

## ➡️ How these apply in Spring Cloud Gateway

- **Retry, Fallback, Circuit Breaker, Rate Limiting** → built-in filters (with Resilience4j).
- **Timeouts** → configurable in Gateway route + Resilience4j.
- **Caching** → custom filter + Redis/Guava integration.
- **Bulkheads, Idempotency, Chaos Testing** → usually handled in backend microservices, but Gateway can help by rejecting excessive requests early.
