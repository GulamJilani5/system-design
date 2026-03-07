⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Server-side Service Discovery & Load Balancing

- In server-side discovery, the Kubernetes discovery server monitors application instances and maintains their details.
- A microservice simply calls the service URL exposed by the Kubernetes Service (ClusterIP / Service DNS) without worrying about the individual Pod instance details.
- Kubernetes automatically discovers services and endpoints using the Kubernetes API.
- The Kubernetes Service receives the request through its ClusterIP and forwards it to one of the available Pods, performing load balancing. 🔴
  - Infrastructure (Kubernetes) controls load balancing.

### 🟦 Tools

- Kubernetes(Kubernetes Service)
