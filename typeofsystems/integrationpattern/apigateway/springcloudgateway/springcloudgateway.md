🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# 🧰 API Gateway

## Spring Cloud Gateway Concepts

### Route

A route = mapping rule that tells the gateway where to send the request.

- Each route has:
  - **ID** → unique name
  - **Predicate** → condition to match
  - **URI** → destination microservice
  - **Filters** → logic applied before/after request.

### Predicate

A condition that must be true for a route to apply.

- **Examples:**

  - `Path=/orders/\*\*`
  - `Method=GET`
  - `Header=X-Request-Id, \d+`
  - `Host=\*.example.com`

### Filters

Filters modify the request (**before**) and response (**after**).

- **Pre-filters**

  - Run before the request is sent to microservice.
  - Used for:
  - Authentication
  - Logging
  - Adding headers
  - Request validation

- **Post-filters**

  - Run after microservice responds.
  - Used for:
  - Logging response
  - Adding response headers
  - Modifying response body

## Dependency ➡️

```java
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```
