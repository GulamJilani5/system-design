🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️
☑️
•
‣
→
⁕

# 🧰 API Gateway
    An API Gateway is a single entry point for all client requests to your microservices. It acts as a reverse proxy,
    forwarding requests to the appropriate microservice and handling concerns like routing, authentication, rate limiting,etc.
### ➡️Use Cases:
| **Use Case**                        | **Why It's Needed**                                          |
| ----------------------------------- | ------------------------------------------------------------ |
| **Centralized Routing**             | Clients call one URL; gateway forwards to the right service. |
| **Authentication & Authorization**  | Handle security logic (OAuth2, JWT) at a single point.       |
| **Rate Limiting & Throttling**      | Prevent abuse by controlling request rates.                  |
| **Request/Response Transformation** | Modify headers, payloads, or status codes as needed.         |
| **Service Aggregation**             | Combine data from multiple services into one response.       |
| **CORS Handling**                   | Configure CORS policies centrally instead of per service.    |
| **Logging & Monitoring**            | Track, trace, and log traffic from one place.                |


### ➡️Tools:
| **Tool**                  | **Ecosystem**                                             | **Notes**                                            |
| ------------------------- | --------------------------------------------------------- | ---------------------------------------------------- |
| **Spring Cloud Gateway**  | Spring Boot / Spring Cloud                                | Native for Spring apps; supports reactive stack.     |
| **Netflix Zuul** (legacy) | Spring Boot (deprecated in favor of Spring Cloud Gateway) | Older Netflix OSS gateway; less performant.          |
| **Kong**                  | Open-source / Enterprise                                  | Lightweight and fast; plugin-based; Lua scripts.     |
| **NGINX / HAProxy**       | Infra-level gateways                                      | Needs custom routing logic; fast but not Java-based. |
| **AWS API Gateway**       | Cloud-based                                               | Great for serverless/microservices on AWS.           |
| **Istio Ingress Gateway** | With Service Mesh                                         | Works with Envoy; advanced service mesh routing.     |


### ➡️Example(Spring Boot Microservices Architecture)
    Client (Browser/Mobile)
    │
    ▼
    Spring Cloud Gateway (API Gateway)
    ├──► Auth Service
    ├──► Product Service
    ├──► Order Service
    └──► User Service

