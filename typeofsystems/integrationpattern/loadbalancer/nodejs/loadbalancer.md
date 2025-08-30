- North-South (Internet -> Cluster)
- East-West (service -> service inside cluster)

# NGINX = Server Side Load Balancer + router

# NIGINX Ingress Controller

It is actually doing the both(Routing + Load Balancing)

### 1. Routing

- Ingress rules --> send traffic to the right **Service**.

### 2. Load Balancing

- Distributes requests across **pods** behind that **Service**.
