🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️
☑️
•
‣
→
⁕

# Two Levels Where Load Balancing Is Applied:
### 🔹 1. At the Edge (Client → System)
##### External Load Balancer
    •Placed in front of the API Gateway or microservices directly.
    •Distributes incoming client traffic across multiple instances of the API Gateway or frontend service.
    •Common tools: AWS ELB/ALB, NGINX, HAProxy, Cloud Load Balancers.
    **Flow:** Client ──► Load Balancer ──► API Gateway ──► Microservices
### 🔹 2. Inside the System (Service → Service)
##### Internal Load Balancer
    •Used when one microservice calls another.
    •Distributes traffic across multiple instances of a target service.
    •This is often handled by:
       Service discovery + load balancer (e.g., Netflix Eureka + Ribbon).
       Service mesh (e.g., Istio, Linkerd) which includes built-in load balancing.
    **Flow:** Service A ──► Internal Load Balancer ──► Service B (multiple instances)

# 🔄 Related Patterns
   **•Client-side Load Balancing:** The client (or a library) picks a service instance using service discovery 
      (e.g., Netflix Ribbon).
   **•Server-side Load Balancing:** A reverse proxy or gateway distributes the load (e.g., NGINX, AWS ALB).
   **•Service Mesh Load Balancing:** Handled by sidecars in service mesh (e.g., Envoy in Istio).
