🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️
☑️
•
‣
→
⁕
# Two Main Category Of Load Balancer

## ➡️1. Deployment Location = Internal or External
→ Focused on where the load balancer operates in your network.

#### 🔵 External Load Balancer
    • Placed in front of the API Gateway or microservices directly.  
    • External load balancers are typically (almost always) server-side, as they are deployed as centralized infrastructure  
    (hardware, software, or cloud-managed services) that handle incoming traffic and distribute it to backend servers.  
    • They sit between clients and servers, managing traffic flow transparently to the client.  
     Common Tools: AWS ELB/ALB, NGINX, HAProxy, Cloud Load Balancers.
**Example:** `Server Side Load Balancer.`
**Flow:** `Client ──► Load Balancer ──► API Gateway ──► Microservices.`

#### 🔵 Internal Load Balancer
    •Used when one microservice calls another.
    •An internal load balancer distributes network traffic across multiple servers or resources within a private 
         network ( e.g. virtual private cloud (VPC), Kubernetes Cluster).
    •Distributes traffic across multiple instances of a target service.
**Example:** `Client-Side Load Balancing, Server Side Load Balancer, Service Mesh Load Balancer.`
**Flow:** `Service A ──► Internal Load Balancer ──► Service B (multiple instances).`

| **Type**                   | **Description**                                                           | **Use Case**                                            | **Decision Made By**                          | **Tools/Examples**                                                                |
| -------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------- |
| **External Load Balancer** | Load balancer exposed to the **outside world** (Internet)                 | Route incoming user traffic to backend services or APIs | Load balancer (centralized)                   | AWS ALB/ELB (public), Cloudflare, Azure Front Door, API Gateway                   |
| **Internal Load Balancer** | Load balancer used **within private network** (VPC, data center, cluster) | Route traffic **between microservices** (internal-only) | Load balancer, sidecar proxy, or client logic | AWS NLB (internal), NGINX (inside VPC), Istio (Envoy), Ribbon, Kubernetes Service |

## ➡️2.Traffic Routing Mechanism
      This is how traffic is routed — based on who decides where the traffic goes.
#### 🔵 Client-side Load Balancer
- The client looks up service instances from service discovery
- It then chooses which instance to send the request to
- No central load balancer in between

**📦 Common Tools:**
- Netflix Ribbon + Eureka
- gRPC with round-robin logic
- Spring Cloud LoadBalancer

**🔍 Used When:**
- You want lightweight, decentralized balancing
- You control the client-side logic
- No need for complex routing or observability
  **Flow:**
- Client (Service A)
  │
  ├─► Queries Service Discovery (e.g., Eureka)
  │
  └─► Picks an instance itself using load balancing logic (e.g., Round Robin)
  ↓
  ┌──────────────────────────────┐
  │   Service B Instance (e.g., B1)   │
  └──────────────────────────────┘
- 🧠 Routing logic is inside the client — no external proxy or load balancer.

#### 🔵 Server-side Load Balancer
  - A reverse proxy or gateway sits between the client and services.
  - The proxy/load balancer decides which instance to route to.
  - Clients only see the load balancer, not the actual services.

   **📦 Common Tools:**
         - NGINX, HAProxy, AWS ELB/ALB, Kubernetes Services, API Gateway

   **🔍 Used When:**
   - •Centralized control of traffic.
   - •Front-door to external traffic.
   - •Load balancing + TLS termination, rate limiting, etc.
     **Flow:**
     Client (Service A or External User)
     │
     └─► Sends request to Central Load Balancer (e.g., NGINX / ELB)
     │
     ├─► Service B - Instance 1
     ├─► Service B - Instance 2
     └─► Service B - Instance 3
  - 🧠 Load balancer (proxy or gateway) chooses which backend instance to send the request to.
  #### 🔵 Service Mesh Load Balancer
   - →Each service has a sidecar proxy (like Envoy) next to it.
   - →🔁 Service Mesh = Advanced Internal Load Balancer + more
   - →Requests are routed via these sidecars, which also handle:
     - •Load balancing
     - •Retries, timeouts
     - •Circuit breaking
     - •Security (mTLS)
     - •Observability (metrics/traces)
    
**📦 Common Tools:**
- •Istio (Envoy sidecar)
- •Linkerd, Consul Connect

 🔍 **Used When:**
  - •You need full control, reliability, and observability for internal communication
  - •You want to separate business logic from networking logic
  - •You’re operating at Kubernetes scale
  **Flow:**  Service A → Envoy A → Envoy B → Service B
  - The Envoy proxy does:
  -  Internal Load Balancing to Service B instances
  -  Automatic retries
  -  Timeouts
  -  mTLS (encryption)
  -  Observability (logs/traces/metrics)
     **Flow:** 
  - Service A
     │
     └─► Sends request to its local sidecar proxy (Envoy)
     │
     └─► Envoy decides and routes to:
     │
     ┌────────────┬────────────┐
     │            │            │
     Service B Sidecar → B1   B2   B3
  - 🧠 Sidecar proxies (e.g., Envoy) handle routing, retries, observability, etc. — not the services themselves.

| **Type**                       | **Description**                                                                                                    | **Use Case**                                                              | **Decision Made By**              | **Tools/Examples**                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------- |
| **Client-side Load Balancer**  | The **calling service (client)** chooses which instance to call using service discovery                            | When you want lightweight in-app balancing without centralized proxy      | **The client itself**             | Netflix Ribbon, Spring Cloud LoadBalancer, gRPC, Feign client |
| **Server-side Load Balancer**  | A **central load balancer** or reverse proxy chooses the backend instance                                          | Entry-point load balancing (e.g., web apps, APIs), centralized control    | **The proxy/load balancer**       | AWS ALB/ELB, NGINX, HAProxy, Kubernetes Service               |
| **Service Mesh Load Balancer** | Each service has a **sidecar proxy** which performs load balancing with advanced features (mTLS, retries, tracing) | Full observability, security, and control over service-to-service traffic | **Sidecar proxies (e.g., Envoy)** | Istio, Linkerd, Consul Connect, Kuma                          |


      
    



