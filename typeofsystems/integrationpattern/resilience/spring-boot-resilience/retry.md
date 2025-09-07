🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ Retry

### ➡️ Retry Logic

Determine when and how many times to retry an operation.

### ➡️ Backoff Strategy

- **Exponential Backoff**: e.g: 1, 2, 4 etc.

### ➡️ Circuit Breaker Integration

- Consider combining the retry pattern with the Circuit Breaker pattern.

### ➡️ Idempotent Operations

- Ensure that the retried operation is idempotent, meaning it produces the same result regardless of how many times it is invoked.

## ➡️ Two Steps involved to use the retry pattern

### 🟦 Spring Cloud Gateway Filter(Edge Server/Routing Gateway)

##### 🔵 A. Add Retry filter:

Inside the method where we are creating a bean of `RouteLocator`, add a filter of retry.

- **Adding these two lines**
  `.retry (retryConfig -> retryConfig.setRetries (3).setMethods (HttpMethod.GET)
  setBackoff (Duration. ofMillis (100),Duration.ofMillis (1000), 2, true)))
`
- **Adding above two lines in the RouteLocator**
  `
  @Bean
  public RouteLocator myRoutes (RouteLocatorBuilder builder) {
  return builder.routes ()
  .route (p -> p.path("/eazybank/loans/\*_")
  filters (f -> f.rewritePath("/eazybank/loans/(?<segment>. _) ","/${segment}")
  addResponseHeader ("X-Response-Time", new Date().toString())
  .retry (retryConfig -> retryConfig.setRetries (3).setMethods (HttpMethod.GET)
  setBackoff (Duration. ofMillis (100),Duration.ofMillis (1000), 2, true)))
  .uri ("lb://LOANS")).build();
  }

`

### 🟦 Normal Spring Boot Service(Microservices)

##### 🔵 A. Add Retry pattern annotations:

Choose a method and mention retry pattern related annotation along with the below configs. Post that create a **fallback** method matching the same method signature like we discussed inside the course(For example in the Circuit Breaker).

`@Retry (name = "getBuildInfo", fallbackMethod = "getBuildInfoFallBack") @GetMapping("/build-info") 
public ResponseEntity<String> getBuildInfo() { 
} 
private ResponseEntity<String> getBuildInfoFallBack (Throwable t) { 
} 
`

##### 🔵 B. Add properties:

- Add the below properties inside the application.yml file,

`
resilience4j.retry:
configs:
default:
maxRetryAttempts: 3
waitDuration: 500
enableExponentialBackoff: true
exponentialBackoffMultiplier: 2
retryExceptions:

- java.util.concurrent.TimeoutException
  ignoreExceptions:
- java.lang.NullPointerException

`
