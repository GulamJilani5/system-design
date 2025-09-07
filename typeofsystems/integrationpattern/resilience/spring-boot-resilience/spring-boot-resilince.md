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

## ➡️ How these apply in Spring Cloud Gateway

- **Retry, Fallback, Circuit Breaker, Rate Limiting** → built-in filters (with Resilience4j).
- **Timeouts** → configurable in Gateway route + Resilience4j.
- **Caching** → custom filter + Redis/Guava integration.
- **Bulkheads, Idempotency, Chaos Testing** → usually handled in backend microservices, but Gateway can help by rejecting excessive requests early.
