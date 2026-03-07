⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Two Main Category Of Load Balancer

## ➡️ 1. Deployment Location = Internal or External

→ Focused on where the load balancer operates in your network.

### 🟦 External Load Balancer

- External LB = handles client → system traffic (gateway level).
- Sits at the front end of your application, It's responsible for receiving requests from external clients
  and directing them to the appropriate backend servers.
- External load balancers are typically (almost always) server-side, as they are deployed as centralized infrastructure
  (hardware, software, or cloud-managed services) that handle incoming traffic and distribute it to backend servers.
- They sit between clients(Internet) and servers, This means it's intercepting incoming requests and deciding
  which server to send them to.

- **Common Tools:**
  - 🔴Spring Cloud Loadbalancer
  - 🔴ring Cloud Gateway
  - AWS ELB/ALB
  - NGINX, HAProxy

- **Example:** Server Side Load Balancer.
- **Flow:** `Client ──► Load Balancer ──► API Gateway ──► Microservices.`

### 🟦 Internal Load Balancer

- Internal LB = handles service ↔ service traffic (within microservices).
- Used when one microservice calls another.
- An internal load balancer distributes network traffic across multiple servers or resources within a private
  network ( e.g. virtual private cloud (VPC), Kubernetes Cluster).
- Distributes traffic across multiple instances of a target service.

- **Example:** Client-Side Load Balancing, Server Side Load Balancer, Service Mesh Load Balancer.
- **Flow:** `Service A ──► Internal Load Balancer ──► Service B (multiple instances).`
- **Common Tools**
  - 🔴Spring Cloud LoadBalancer (client-side LB, replacement of Ribbon).
  - 🔴Eureka + LoadBalancer (service discovery + traffic distribution).
  - Kubernetes Service (ClusterIP / kube-proxy) if using K8s.

| **Type**                   | **Description**                                                           | **Use Case**                                            | **Decision Made By**                          | **Tools/Examples**                                                                |
| -------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------- |
| **External Load Balancer** | Load balancer exposed to the **outside world** (Internet)                 | Route incoming user traffic to backend services or APIs | Load balancer (centralized)                   | AWS ALB/ELB (public), Cloudflare, Azure Front Door, API Gateway                   |
| **Internal Load Balancer** | Load balancer used **within private network** (VPC, data center, cluster) | Route traffic **between microservices** (internal-only) | Load balancer, sidecar proxy, or client logic | AWS NLB (internal), NGINX (inside VPC), Istio (Envoy), Ribbon, Kubernetes Service |

## ➡️ 2.Traffic Routing Mechanism

- This is how traffic is routed — based on who decides where the traffic goes.

### 🟦 Client-side Load Balancer(~30%)🟥

- find `D:\Jilani\learning\system design\typeofsystems\integrationpattern\loadbalancer\springcloudloadbalancer\client-side-lb\clinet-side-lb.md`

### 🟦 Server-side Load Balancer (~70%)🟥

- find `D:\Jilani\learning\system design\typeofsystems\integrationpattern\loadbalancer\springcloudloadbalancer\server-side-lb\server-side-lb.md`
