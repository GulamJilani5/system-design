⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Server-side Load Balancer (~70%)🟥

- A reverse proxy or gateway sits between the client and services.
- The proxy/load balancer decides which instance to route to.
- Clients only see the load balancer, not the actual services.
- In Kubernetes, the default Service + kube-proxy behaves more like a server-side LoadBalancer.

- **Analogy:** You go to the restaurant reception. The receptionist decides which waiter (service instance) will serve you.

### 🟦 Used When

- Front-door to external traffic.
- Load balancing + TLS termination, rate limiting, etc.

### 🟦 Common Tools

- 🔴Spring Cloud Gateway
- 🔴Kubernetes Services
- NGINX,
- AWS ELB/ALB,
- HAProxy, API Gateway

### 🟦 Flow

- Client (Service A or External User)
  │
  └─► Sends request to Central Load Balancer (e.g., Spring Cloud Gateway / NGINX / ELB)
  │
  ├─► Service B - Instance 1
  ├─► Service B - Instance 2
  └─► Service B - Instance 3
- 🧠 Load balancer (proxy or gateway) chooses which backend instance to send the request to.
- `Client → Server-Side Load Balancer → Backend Service Instance.`

## ➡️ Server-side Service Discovery & Load Balancing (Highly Used)

- In server-side discovery, the Kubernetes discovery server monitors application instances and maintains their details.
- A microservice simply calls the service URL exposed by Kubernetes without worrying about instance details.
- Kubernetes automatically discovers services and endpoints using the Kubernetes API.
- The Kubernetes service forwards the request to one of the available instances and performs load balancing.🔴
  - Infrastructure (Kubernetes) controls load balancing.

#### 🟦 Tools

- Kubernetes(Kubernetes Service)

## ➡️ Service Mesh Load Balancer

- →Each service has a sidecar proxy (like Envoy) next to it.
- →🔁 Service Mesh = Advanced Internal Load Balancer + more
- →Requests are routed via these sidecars, which also handle:
  - Load balancing
  - Retries, timeouts
  - Circuit breaking
  - Security (mTLS)
  - Observability (metrics/traces)
- **Used When:**
  - You need full control, reliability, and observability for internal communication
  - You want to separate business logic from networking logic
  - You’re operating at Kubernetes scale

#### 🟦 Common Tools:

- Istio (Envoy sidecar)
- Linkerd, Consul Connect

#### 🟦 Flow:

- Service A → Envoy A → Envoy B → Service B
- The Envoy proxy does:
- Internal Load Balancing to Service B instances
- Automatic retries
- Timeouts
- mTLS (encryption)
- Observability (logs/traces/metrics)
- **Flow:**
  - Service A
    │
    └─► Sends request to its local sidecar proxy (Envoy)
    │
    └─► Envoy decides and routes to:
    │
    ┌────────────┬────────────┐
    │ │ │
    Service B Sidecar → B1 B2 B3
- 🧠 Sidecar proxies (e.g., Envoy) handle routing, retries, observability, etc. — not the services themselves.

| **Type**                       | **Description**                                                                                                    | **Use Case**                                                              | **Decision Made By**              | **Tools/Examples**                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------- |
| **Client-side Load Balancer**  | The **calling service (client)** chooses which instance to call using service discovery                            | When you want lightweight in-app balancing without centralized proxy      | **The client itself**             | Netflix Ribbon, Spring Cloud LoadBalancer, gRPC, Feign client |
| **Server-side Load Balancer**  | A **central load balancer** or reverse proxy chooses the backend instance                                          | Entry-point load balancing (e.g., web apps, APIs), centralized control    | **The proxy/load balancer**       | AWS ALB/ELB, NGINX, HAProxy, Kubernetes Service               |
| **Service Mesh Load Balancer** | Each service has a **sidecar proxy** which performs load balancing with advanced features (mTLS, retries, tracing) | Full observability, security, and control over service-to-service traffic | **Sidecar proxies (e.g., Envoy)** | Istio, Linkerd, Consul Connect, Kuma                          |
