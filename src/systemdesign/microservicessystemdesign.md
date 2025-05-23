# Microservices Architecture
    software is broken down into small, independent services that communicate 
    over well-defined APIs. To build scalable, maintainable, and robust systems, several design patterns 
    are commonly used.

### 🧱 1. Decomposition Patterns
    Purpose: Break a monolith into smaller, manageable microservices.
| Pattern                              | Description                                                                    |
| ------------------------------------ | ------------------------------------------------------------------------------ |
| **Decompose by Business Capability** | Split services based on business functions (e.g., Billing, Orders, Inventory). |
| **Decompose by Subdomain**           | Use Domain-Driven Design (DDD) to split services by bounded contexts.          |

### 🔁 2. Integration Patterns
    Purpose: Manage communication between services.
| Pattern                        | Description                                                                                                                    |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| **API Gateway**                | Central entry point that routes requests to appropriate microservices and handles cross-cutting concerns (auth, rate limiting). |
| **Backend for Frontend (BFF)** | Custom APIs for each type of frontend (mobile, web), improving performance and maintainability.                                |
| **Service Mesh**               | Infrastructure layer for handling service-to-service communication, security, and observability (e.g., Istio, Linkerd).        |
| **Load Balancer** 	            | Distributes network traffic across multiple service instances to ensure availability and reliability.                          |

### 📡 3. Communication Patterns
    Purpose: Define how microservices communicate.

| Pattern                                   | Description                                                                          |
| ----------------------------------------- | ------------------------------------------------------------------------------------ |
| **Synchronous (REST/gRPC)**               | Direct API calls between services using HTTP or gRPC.                                |
| **Asynchronous (Messaging/Event-Driven)** | Services communicate via message brokers (Kafka, RabbitMQ) to reduce tight coupling. |


### 💾 4. Database Patterns
    Purpose: Handle data consistency across services.

| Pattern                                             | Description                                                                        |
| --------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Database per Service**                            | Each microservice owns its own database schema.                                    |
| **Shared Database**                                 | Less preferred—multiple services share one database (anti-pattern for most cases). |
| **Event Sourcing**                                  | Capture all state changes as a sequence of events.                                 |
| **CQRS (Command Query Responsibility Segregation)** | Separate models for update (command) and read (query) operations.                  |


### 🔄 5. Reliability Patterns
    Purpose: Ensure system resilience and fault tolerance.
| Pattern             | Description                                                              |
| ------------------- | ------------------------------------------------------------------------ |
| **Circuit Breaker** | Prevent cascading failures by cutting off access to a failing service.   |
| **Bulkhead**        | Isolate failures to a part of the system to prevent full system failure. |
| **Retry**           | Automatically retry failed requests with backoff strategy.               |
| **Timeout**         | Abort requests that take too long.                                       |
| **Failover**        | Automatically switch to a backup service if the main one fails.          |

### 🧩 6. Observability Patterns
    Purpose: Enable monitoring, tracing, and debugging.
| Pattern                 | Description                                                          |
| ----------------------- | -------------------------------------------------------------------- |
| **Log Aggregation**     | Centralized logging system (e.g., ELK Stack).                        |
| **Distributed Tracing** | Trace a request across multiple services (e.g., Jaeger, Zipkin).     |
| **Metrics Collection**  | Monitor service health and performance (e.g., Prometheus + Grafana). |

### 🔐 7. Security Patterns
    Purpose: Secure communication and access.
| Pattern                               | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| **Access Token (JWT/OAuth)**          | Use tokens for secure and stateless authentication.          |
| **Service-to-Service Authentication** | Secure internal traffic using mTLS or identity tokens.       |
| **API Gateway Auth**                  | Centralized authentication and authorization at the gateway. |









