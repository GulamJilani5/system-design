# Flow Of A Request Using Feign + Eureka + LoadBalancer

## Flow: Service A calls Service B using Feign

1. Service A → Feign Client Call
2. Feign → Service Discovery (Eureka)
3. Feign → Load Balancer (Client-Side)
4. LoadBalancer → Service B Instance
5. Response Returns to Service A

## Diagram

Service A (Feign Client)
│
│ Feign request (service name = "payment-service")
▼
Eureka Server (Service Registry)
│
│ Provides list of Payment-Service instances
▼
Spring Cloud LoadBalancer (Client-Side LB)
│
│ Chooses one instance
▼
Payment-Service Instance (Service B)
