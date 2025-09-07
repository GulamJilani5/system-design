🔵 🟢 🔴 ➡️ ⏺️ ⭕ 🟠 🟦 🟣 🟥 🟧 ✔️ ☑️ • ‣ → ⁕

# ⏺️ RATE LIMITTER PATTERN

- Unrestricted and uncontrolled requests can lead to performance degradation, resource exhaustion, and potential denial-of-service (DoS) attacks.
- When a user surpasses the permitted number of requests within a designated time frame, any additional requests are declined with an HTTP 429 - Too Many Requests status.
- Additionally, the Rate Limiter pattern proves beneficial for providing services to users based on their subscription tiers. For instance, distinct rate limits can be defined for basic, premium, and enterprise users.

## ➡️ Two Steps involved to use the retry pattern

### 🟦 1. Spring Cloud Gateway Filter(Edge Server/Routing Gateway)

##### 🔵 A. Add maven dependency:

`spring-boot-starter-data-redis-reactive`

##### 🔵 B. Add rate limitter filter:

Inside the method where we are creating a bean of `RouteLocator`, add a filter of rate limitter like highlighted below and creating supporting beans of `RedisRateLimiter` and `KeyResolver`.

````java
@Bean
public RouteLocator myRoutes (RouteLocatorBuilder builder) {
return builder.routes ()
.route(p->p.path("/eazybank/cards/\*_")
.filters (f -> f.rewritePath("/eazybank/cards/(?<segment>._)","/${segment}")
.addResponseHeader ("X-Response-Time", new Date().toString()) .requestRateLimiter (config ->
config.setRateLimiter (redisRateLimiter()).setKeyResolver (userKeyResolver ()))) .uri("lb://CARDS")).build();
}
@Bean
public RedisRateLimiter redisRateLimiter() {
}
return new RedisRateLimiter (1, 1, 1);
@Bean
KeyResolver userKeyResolver() {
}
return exchange -> Mono.justOrEmpty (exchange.getRequest().getHeaders().getFirst("user"))
defaultIfEmpty ("anonymous");

`

### 🟦 Normal Spring Boot Service(Microservices)

##### 🔵 A. Add Rate limitter pattern annotations:

- Choose a method and mention rate limitter pattern related annotation along with the below configs. Post that create a **fallback** method matching the same method signature like we discussed inside the course (For example in the Circuit Breaker).

```java
@RateLimiter (name = "getJavaVersion", fallbackMethod = "getJavaVersionFallback") @GetMapping("/java-version")
public ResponseEntity<String> getJavaVersion() {
}
private ResponseEntity<String> getJavaVersionFallback (Throwable t) {
}

````

##### 🔵 B. Add properties:

- Add the below properties inside the `application.yml` file

```java
    resilience4j.ratelimiter:
    configs:
    default:
    timeoutDuration:
    5000
    limitRefreshPeriod: 5000
    limitForPeriod: 1

```
