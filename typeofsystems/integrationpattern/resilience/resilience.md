🔵 🟢 🔴 ➡️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# Resilience

Resilience in the context of Spring Boot microservices refers to the ability of a distributed system to handle failures gracefully, prevent cascading errors, and maintain overall availability.

- Microservices architectures, by design, involve inter-service communication over networks, which introduces risks like network latency, service outages, or overloads.
- Spring Boot, combined with **Spring Cloud** and libraries like `Resilience4j` (the modern successor to **Netflix Hystrix**), provides built-in tools and patterns to build fault-tolerant applications.

## Resilience Patterns and Features

### ➡️ 1. Circuit Breaker

A circuit breaker prevents repeated calls to a failing service by "opening" the circuit after a certain number of failures (e.g., timeouts or errors).

- **What:**
  Monitors service failures and stops requests to a failing service after a threshold, redirecting to a fallback.
  A circuit breaker is typically configured to trigger on specific errors (e.g., timeouts, HTTP 500 or 503 errors), not any error. It’s designed for handling service failures or unavailability

- **Triggers:**
  Service errors (e.g., timeouts, 500, 503), not rate limit violations or retries directly (though retries can contribute to failures).
- **Problem Solved:**
  Prevents cascading failures and provides graceful degradation.

##### 🟦 Circuit Breaker working:

A circuit breaker sends users to a fallback page when a service fails and checks service health in the background. Once the service recovers, new requests go back to the original page/service. Users aren’t auto-redirected from the fallback unless extra logic (e.g., polling/auto-refresh implemented or configured) is added.

##### 🟦 Circuit Breaker vs. Rate Limiting:

Rate limiting enforces client quotas and isn’t a failure condition, so it doesn’t trigger a circuit breaker.

##### 🟦 Circuit Breaker vs. Retry:

Retries attempt to recover from transient failures. If retries fail, a circuit breaker can step in to provide a fallback.

### ➡️ 2. Retry

- **What:**
  Automatically retries a failed request a set number of times (**e.g.**, 3) with configurable time intervals (**e.g.**, exponential backoff: 100ms, 200ms, 400ms).

- **On Max Attempts:**
  Returns the original error (**e.g.**, 503) unless combined with a circuit breaker for a fallback.

- **Problem Solved:**
  Handles transient failures like network issues or temporary service unavailability

##### 🟦 Retries + Circuit Breakers

We can Combine retries with a circuit breaker to trigger a fallback after retries fail.

##### 🟦 Remember!!! 🔴

- If all retries fail, the gateway returns the original error (**e.g.**, 503) to the client, not a fallback unless combined with a circuit breaker.
- Retries don’t inherently trigger a fallback. They return the original error if all attempts fail.
- If all retries fail, the circuit breaker detects the failure, opens the circuit, and routes to the /fallback endpoint.

### ➡️ 3. Rate Limiting / Throttling

- **What:**
  Limits the number of requests a client can make in a time window (**e.g.**, 100 requests per minute).

- **Response on Limit Exceeded:**
  Returns 429 Too Many Requests with a custom message (not a fallback like circuit breaker).

- **Problem Solved:**
  Prevents API abuse, server overload, or DDoS attacks.

### 4. Fallback

### 5. Bulkhead Isolation

### 6. Timeouts

### 7. Caching

### 8. Idempotency

### 9. Load Balancing

### 10. Graceful Degradation

### 11. Chaos Engineering Support
