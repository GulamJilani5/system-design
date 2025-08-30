# Service Registry (Eureka Server || Server Side)

- Think of it like a **phonebook(registry)** where all microservices register themselves.
- Every **Service** (`Client | Microservice`) tells **Eureka Server**: “I’m alive, my address is `http://ip:port.`”
- Other **Services** (`Clients | Microservices`) then query **Eureka Server** to find where to call.

## Dependency

`<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
`

## Usage

- Add @EnableEurekaServer in the main Spring Boot application class.
- Acts as the registry center.

### Eureka Self-Preservation Mode

- Server-Side protection mechanism.
- If the Eureka Server suddenly loses too many heartbeats (e.g., because of a network partition, not actual service failure), it assumes it’s a network problem instead of mass service failure.
- To prevent wiping out the entire registry, Eureka enters self-preservation mode.
- In this mode, it stops expiring instances and keeps serving stale data, ensuring availability.
