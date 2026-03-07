⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Client-side Load Balancer(~30%)🟥

- In client-side discovery, applications register themselves with a service registry (like Eureka) during startup and send regular heartbeats.
- When a service needs another service, it queries the service registry to get the list of available instance IP addresses.
- The registry returns the list of service instances.
- The client itself selects one instance using a load-balancing strategy and sends the request.🔴
  - client has control of the load balancing.

### 🟦 Tools

- Service Registry(Eureka Server)
- Spring Cloud Load Balancer

### 🟦 Used When:

- You want lightweight, decentralized balancing
- You control the client-side logic
- No need for complex routing or observability

### 🟦 Flow:

- Client (Service A)
  │
  ├─► Queries Service Discovery (e.g., Eureka)
  │
  └─► Picks an instance itself using load balancing logic (e.g., Round Robin)
  ↓
  ┌──────────────────────────────┐
  │ Service B Instance (e.g., B1) │
  └──────────────────────────────┘

- Routing logic is inside the client — no external proxy or load balancer.
- `Client → (gets instance list from service registry) → Chooses instance → Calls service directly.`

### 🟦 Feign Client + LoadBalancer

- **Feign Client** → used to call another microservice just by its service name (**e.g.**, `order-service`).
- **Spring Cloud LoadBalancer** → automatically picks one instance (from multiple registered instances) of that service.
- **Developer (We)** → don’t need to write any code to select which instance; it happens behind the scenes.
