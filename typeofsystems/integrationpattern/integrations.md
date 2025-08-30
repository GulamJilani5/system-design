# Service discovery/registry:

- “Kubernetes Services (registry in API server) + CoreDNS (discovery)”

# API gateway:

- “NGINX Ingress Controller” (important nuance) and/or Managed (Apigee/AWS API Gateway/Azure APIM)

# Load Balancer:

- Add cloud LB (ALB/NLB/etc.) for entry; NGINX Ingress does L7 load-balancing; in-cluster Service LB handled by K8s

# Observability:

- “Prometheus, Grafana” — consider adding OpenTelemetry + Jaeger/Loki.

# CI/CD:

- “GitHub Actions / GitLab CI”

# Reverse Proxy

- A reverse proxy is a server that sits in front of one or more backend servers and forwards client requests to them.
- A reverse proxy usually performs routing.

- **Examples:**
  - NGINX, HAProxy, Envoy, Traefik.
