🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# 🧰 API Gateway

An API Gateway is a single entry point for all client requests to your microservices. It acts as a reverse proxy,
forwarding requests to the appropriate microservice and handling concerns like **routing**, **authentication**, **rate limiting**, etc.

- **Gateway = Routing + Load Balancer**
  - **Routing** → (Choose Service) - decides which microservice should handle the request.
  - **Load Balancer** → (Choose Instance) - distributes traffic across multiple instances of that microservice.

### ➡️Use Cases: Cross-Cutting Concerns Solved by API Gateway

##### 🟦 1. Authentication & Authorization

Centralized security (JWT, OAuth2, API keys).

Example: Validate JWT once at gateway → don’t repeat in every microservice.

##### 🟦 2. Logging & Monitoring

Capture request/response logs, performance metrics, request tracing.

Helps in observability and debugging.

##### 🟦 3. Rate Limiting & Throttling

Control how many requests a client can make → protect services from overload.

##### 🟦 4. Load Balancing

Distribute requests across multiple service instances.

Can integrate with Eureka/Consul for service discovery. 5. Request Routing

Route requests to correct microservice based on path, headers, etc.

##### 🟦 6. Resilience (Circuit Breakers, Retries, Fallbacks)

If a service is down, API Gateway can return fallback responses instead of crashing.

##### 🟦 7. CORS & Security Headers

Apply CORS policy (which frontend domains can access APIs).
Add/remove security headers at one place.

##### 🟦 8. Transformation

Modify requests/responses (headers, payload) without touching services.

### ➡️Tools:

| **Tool**                   | **Ecosystem**                                             | **Notes**                                            |
| -------------------------- | --------------------------------------------------------- | ---------------------------------------------------- |
| **🔴Spring Cloud Gateway** | Spring Boot / Spring Cloud                                | Native for Spring apps; supports reactive stack.     |
| **Netflix Zuul** (legacy)  | Spring Boot (deprecated in favor of Spring Cloud Gateway) | Older Netflix OSS gateway; less performant.          |
| **Kong**                   | Open-source / Enterprise                                  | Lightweight and fast; plugin-based; Lua scripts.     |
| **NGINX / HAProxy**        | Infra-level gateways                                      | Needs custom routing logic; fast but not Java-based. |
| **AWS API Gateway**        | Cloud-based                                               | Great for serverless/microservices on AWS.           |
| **Istio Ingress Gateway**  | With Service Mesh                                         | Works with Envoy; advanced service mesh routing.     |

### Dependency ➡️

`
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>

`
