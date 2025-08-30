# Service Discovery (Eureka Client || Client Side)

- Think of it like users of the phonebook(registry), services will discover(Search) other services and fetch the data.
- **Example:** `Order-Service` looks up `Payment-Service` from the registry and calls it.

## Dependency

`<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
`

## Usage

- Add **@EnableEurekaClient** (or just **@EnableDiscoveryClient** since Spring Boot auto-configures it).
- Service automatically registers itself with Eureka Server.

### Heartbeat Mechanism

- Each registered client (microservice) sends a heartbeat (a renewal request) to the Eureka Server at regular intervals (default: every 30 seconds).
- If the server(Eureka Server) does not receive a heartbeat within the configured time (default: 90 seconds), it marks the instance as down/unavailable.
