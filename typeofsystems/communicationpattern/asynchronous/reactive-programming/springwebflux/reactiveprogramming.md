🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Reactive Programming

## ➡️ Reactive

- **Imperative Programming** (one request = one thread) vs **Reactive Programming** (non-blocking, async, event-driven).

#### Reactive Streams specification → 4 main interfaces:

- **Publisher:** Emits data (like a **JS** event emitter or **Observable**).
- **Subscriber:** Consumes the data (subscribes and reacts to emissions).
- **Subscription:** Manages the flow between them, including requesting data to control the rate (**backpressure**).
- **Processor:** Combines **Publisher** and **Subscriber** for transformations.

## ➡️ Reactor Core

- **Mono<T>** → Emits 0 or 1 value (like Promise in JS).
- **Flux<T>** → Emits 0..N values (like an async iterable or stream).

#### 🟦 Operators

- **map** → transform each value.
- **flatMap** → transform each value asynchronously (may mix order).
- **filter** → keep only matching values.
- **zip** → combine two streams into one (pair elements).
- **concat** → join two streams one after another in order.

## ➡️ Spring Webflux

- **Dependency**
  `
  <dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-webflux</artifactId>
  </dependency>

`

## ➡️ Spring MVC vs Spring Webflux

#### 🟦 Annotation-Based And Functional Routing.

## ➡️ Reactive Data Access

- Reactive Repositories with MongoDB / R2DBC

- **Dependency**

`spring-boot-starter-data-mongodb-reactive.
 spring-boot-starter-data-r2dbc.
`

## ➡️ WebClient (Reactive HTTP Client)

- Call Another Service Asynchronously

`
@Service
public class UserService {

        private final WebClient webClient;

        public UserService(WebClient webClient) {
            this.webClient = webClient;
        }

        // Async call to another service
        public Mono<User> getUserById(String id) {
            return webClient.get()
                    .uri("/api/users/{id}", id) // appends to base URL
                    .retrieve()
                    .bodyToMono(User.class); // returns Mono<User>
        }

    }

`

## ➡️ Advanced WebFlux

#### 🟦 Streaming APIs

- Return Server-Sent Events (SSE)
- SSE is perfect for **Unidirectional Server-To-Client** streaming over HTTP, simpler than WebSocket for cases where clients only need to receive updates. (**e.g.** `Cricket Score App` and a `News App`).

#### 🟦 WebSockets with WebFlux

- Real-Time Chat Application
- **Bidirectional Communication** (client-to-server and server-to-client) over a single, persistent connection, perfect for low-latency, interactive apps.

#### 🟦 Testing Reactive APIs

- Use `StepVerifier` for **Unit Testing**.
- Use `WebTestClient` for **Integration Testing**.
