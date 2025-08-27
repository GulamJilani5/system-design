## Service Registry (Eureka Server || Server Side)

- Think of it like a **phonebook(registry)** where all microservices register themselves.
- Every **Service** (`Client | Microservice`) tells **Eureka Server**: “I’m alive, my address is `http://ip:port.`”
- Other **Services** (`Clients | Microservices`) then query **Eureka Server** to find where to call.

### Dependency

`
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

`

## Usage

Add @EnableEurekaServer in the main Spring Boot application class.
Acts as the registry center.

## Service Discovery (Eureka Client || Client Side)

- Think of it like users of the phonebook(registry), services will discover(Search) other services and fetch the data.
- **Example:** `Order-Service` looks up `Payment-Service` from the registry and calls it.

### Dependency

`
<dependency>
<groupId>org.springframework.cloud</groupId>
<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

`

### Usage

- Add **@EnableEurekaClient** (or just **@EnableDiscoveryClient** since Spring Boot auto-configures it).
- Service automatically registers itself with Eureka Server.
