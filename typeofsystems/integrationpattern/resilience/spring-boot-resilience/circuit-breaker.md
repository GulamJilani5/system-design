🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Spring Cloud Circuit Breaker

- We can implement **Circuit Breaker** using `Resilience4J`.

## ➡️ In Resilience4J, the Circuit Breaker is implemented via three states.

### 🟦 1. CLOSED(Normal state)

- Calls are short-circuited immediately (fallback is triggered).
- This prevents wasting time and resources on a service that’s already failing.
- The breaker will stay open for a defined waitDurationInOpenState (e.g., 30 seconds).
- **Example:**
  - Service-B is down → Service-A immediately fails fast with fallback instead of waiting for timeouts.

### 🟦 2. OPEN (Service considered unhealthy)

- No requests go to the failing service
- Calls are short-circuited immediately (fallback is triggered).
- This prevents wasting time and resources on a service that’s already failing.
- The breaker will stay open for a defined waitDurationInOpenState (e.g., 30 seconds).
- **Example:**
  - Service-B is down → Service-A immediately fails fast with fallback instead of waiting for timeouts.

##### 🟦 3. HALF_OPEN(Testing state)

- After the wait time, the circuit breaker allows a limited number of trial requests to check if the service has recovered.
- If the trial requests succeed → breaker goes back to CLOSED.
- If they fail → breaker goes back to OPEN.
- Example:
  After 30 seconds, Service-A tries a few requests to Service-B.

  - If successful → resume normal traffic (CLOSED).
  - If not → keep breaker OPEN.

## ➡️ Two Ways to use the Circuit Breaker

### 🟦 1. Spring Cloud Gateway Pattern Filter

In Spring Cloud Gateway, the Resilience4j Circuit Breaker can be applied as a filter to manage requests routed through the gateway. This is useful for protecting downstream services by applying circuit breaker logic at the gateway level.

- **How It Works**

The circuit breaker filter intercepts requests routed through the gateway.
If the downstream service fails (based on configured thresholds), the circuit breaker trips, and a fallback URI or response can be returned.
This approach centralizes resilience logic at the gateway, avoiding the need to implement circuit breakers in each microservice.

- Dependency(Uses Spring Webflux)

`<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-circuitbreaker-reactor-resilience4j</artifactId>
</dependency>`

### 🟦 2. Normal Spring Boot Service

In a standard Spring Boot service, the Resilience4j Circuit Breaker is applied at the service layer using annotations or programmatic APIs. This is useful for protecting specific service methods that call external systems or dependencies.

- **How It Works**
  The @CircuitBreaker annotation wraps a method, monitoring its failures.
  If the failure rate exceeds the configured threshold, the circuit breaker trips, and a fallback method is invoked (if defined).
  This approach is granular, allowing circuit breaker logic to be applied to specific operations within a service.

- Dependency

`<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
`
