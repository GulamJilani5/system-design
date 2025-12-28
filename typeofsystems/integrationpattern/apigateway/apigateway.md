🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# 🧰 API Gateway

An API Gateway is a single entry point for all client requests to your microservices. It acts as a reverse proxy,
forwarding requests to the appropriate microservice and handling concerns like **routing**, **authentication**, **rate limiting**, etc.

- **Gateway = Routing + Load Balancer**
  - **API Gateway(Routing)** → (Choose Service) - decides which microservice should handle the request.
  - **Load Balancer** → (Choose Instance) - distributes traffic across multiple instances of that microservice.

### ➡️ Use Cases: Cross-Cutting Concerns Solved by API Gateway

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

### ➡️ Tools:

| **Tool**                   | **Ecosystem**                                             | **Notes**                                            |
| -------------------------- | --------------------------------------------------------- | ---------------------------------------------------- |
| **🔴Spring Cloud Gateway** | Spring Boot / Spring Cloud                                | Native for Spring apps; supports reactive stack.     |
| **Netflix Zuul** (legacy)  | Spring Boot (deprecated in favor of Spring Cloud Gateway) | Older Netflix OSS gateway; less performant.          |
| **Kong**                   | Open-source / Enterprise                                  | Lightweight and fast; plugin-based; Lua scripts.     |
| **NGINX / HAProxy**        | Infra-level gateways                                      | Needs custom routing logic; fast but not Java-based. |
| **AWS API Gateway**        | Cloud-based                                               | Great for serverless/microservices on AWS.           |
| **Istio Ingress Gateway**  | With Service Mesh                                         | Works with Envoy; advanced service mesh routing.     |

## ➡️ Edge Server/API Gateway Pattern

### 🟦 API Gateway Pattern

The API Gateway Pattern is a design pattern where a single entry point (the API Gateway, or edge server) handles all client requests to a microservices-based system. It routes requests to appropriate backend services and manages cross-cutting concerns like authentication, rate limiting, circuit breaking, and monitoring.

### 🟦 Gateway Routing Pattern

The Gateway Routing Pattern is a subset of the API Gateway Pattern, focusing specifically on routing requests from the gateway to the appropriate backend service based on rules (e.g., URL paths, headers, or query parameters). It’s about directing traffic efficiently.

##### 🔵 Why Required / Problem Solved

- **Problem:** Clients shouldn’t need to know the URLs of individual microservices (e.g., http://payment-service:8081). This is complex and fragile, especially if services change locations.
- **Solution:** The gateway routes requests to the correct service using predefined rules, hiding service details from clients.

### 🟦 Gateway Offloading Pattern

The Gateway Offloading Pattern involves moving cross-cutting concerns (e.g., authentication, rate limiting, logging, monitoring) from individual services to the API Gateway. This centralizes common logic, reducing duplication in backend services.

##### 🔵 Why Required / Problem Solved

- **Problem:** If every service handles tasks like authentication, rate limiting, or circuit breaking, you end up with duplicated code, inconsistent implementations, and maintenance overhead.
- **Solution:** The gateway handles these tasks centrally, so backend services focus only on business logic.

### 🟦 Backend For Frontend(BFF) Pattern

The Backend for Frontend Pattern (BFF) involves creating a dedicated backend (or gateway) tailored to the needs of a specific client type (e.g., web, mobile, or IoT). Each client gets its own BFF that optimizes the API for its specific requirements, reducing client-side complexity.

##### 🔵 Why Required / Problem Solved

- **Problem:** Different clients (web, mobile, IoT) have different needs (e.g., data formats, response sizes, or endpoints). A single API Gateway serving all clients might return generic responses that are inefficient (e.g., too much data for mobile clients).

- **Solution:**

  - Each client type gets a dedicated BFF that:
    - Tailors APIs to the client’s needs (e.g., simplified data for mobile).
    - Aggregates data from multiple services.
    - Reduces client-side processing.

### 🟦 Gateway Aggregator/Composite Pattern

The Gateway Aggregation/Composite Pattern involves the API Gateway combining data from multiple backend services into a single response for the client. Instead of the client making multiple calls to different services, the gateway aggregates the results.

##### 🔵 **Why Required / Problem Solved**

- **Problem:**

  - Clients often need data from multiple services (e.g., user profile and order history).
    Making multiple API calls from the client increases latency, complexity, and network overhead.

- **Solution:**
  - The gateway calls multiple services, combines their responses, and sends a single response to the client.
    Reduces client-side complexity and improves performance.
