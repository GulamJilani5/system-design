## Spring Boot's resilience capabilities are primarily enabled through:

- **Spring Cloud Circuit Breaker:** A high-level abstraction for implementing resilience patterns.
- **Resilience4j:** A lightweight, functional-style library integrated with Spring Boot for fault tolerance. It's configurable via annotations and `YAML` properties, and supports **metrics** via `Spring Boot Actuator`.
- **Other Spring Cloud Tools:** Such as **Service Discovery (Eureka)**, **Load Balancing (Ribbon or Spring Cloud LoadBalancer)**, and API Gateways (`Spring Cloud Gateway`) that enhance resilience indirectly.

### Spring Cloud Circuit Breaker with Resilience4j

This dependency provides the integration of Resilience4j with Spring Cloud's circuit breaker abstraction, allowing you to use annotations like @CircuitBreaker, @Retry, etc.

`<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
`
`<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-circuitbreaker-reactor-resilience4j</artifactId>
</dependency>`

### Resilience4j

This dependency provides Resilience4j's core libraries and Spring Boot-specific integrations, such as configuration via application.yml and Actuator metrics.

`<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot3</artifactId>
</dependency>
`

### Redis For Rate Limiting

`
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>

`
