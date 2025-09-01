# Cache

## @Cacheable

Result of the method should be cached. Next time the method is called with the same parameters, Spring will return the cached result instead of executing the method again.

- **Java core (JDK)** → ❌ Does NOT have @Cacheable.
- **Spring Framework / Spring Boot** → Provides @Cacheable used in **Service Layer**.
- **Package**
  - `org.springframework.cache.annotation.Cacheable`

## Flow of @Cacheable

- **Client Calls Method** → You call a service method annotated with `@Cacheable`.
- **Spring AOP Interceptor** → Spring intercepts the call before executing the method.
- **Cache Manager** → Spring asks the configured CacheManager (e.g., `Redis`, `Caffeine`, `ConcurrentMap`) if a cached value exists for the given method + parameters.
- **Cache Hit** → If value exists → return directly from cache (method body is skipped).
- **Cache Miss **→ If value does not exist → execute method → store result in cache.
- **Response Returned** → Result goes back to the caller, either from cache or after execution.

## Important annotations

- Caching annotations used in **Service Layer**
- **@EnableCaching:** Used on Main Method's class
- **@Cacheable:** → Fetch and cache if not already cached.
- **@CachePut:** → Always update cache with latest value.
- **@CacheEvict:** → Remove from cache when deleted.
