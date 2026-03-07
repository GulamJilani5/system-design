⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Client-side Service Discovery & Load Balancing

- In client-side discovery, applications register themselves with a service registry (like Eureka) during startup and send regular heartbeats.
- When a service needs another service, it queries the service registry to get the list of available instance IP addresses.
- The registry returns the list of service instances.
- The client itself selects one instance using a load-balancing strategy and sends the request.🔴
  - client has control of the load balancing.

### 🟦 Tools

- Service Registry(Eureka Server)
- Spring Cloud Load Balancer
