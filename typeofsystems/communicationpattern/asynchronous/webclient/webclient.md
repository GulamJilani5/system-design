## ➡️ 1. WebClient

- WebClient is a non-blocking, reactive HTTP client introduced in `Spring 5` as part of the `Spring WebFlux` framework. - It is designed for making asynchronous and reactive HTTP requests in a reactive programming model, suitable for modern, scalable applications.
- WebClient leverages Project Reactor's reactive types (`Mono` and `Flux`) to handle asynchronous data streams efficiently.

#### 🟦Features

- Non-Blocking and Reactive:
- Client-Side Load Balancing:🔴🔴
- WebSocket Support:🔴🔴
- Functional API:
- Customizable
- Error Handling:
- Streaming Support:
- Retry and Timeout Support:

#### 🟦 Dependency (pom.xml)

```java
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-webflux</artifactId>
  </dependency>
```

#### 🟦 How use Webclient

##### 🔵 Example

```java
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

public class WebClientExample {

    // Define a simple User class
    static class User {
        private Long id;
        private String name;
        private String email;

        // Getters and setters
        public Long getId() { return id; }
        public void setId(Long id) { this.id = id; }
        public String getName() { return name; }
        public void setName(String name) { this.name = name; }
        public String getEmail() { return email; }
        public void setEmail(String email) { this.email = email; }

        @Override
        public String toString() {
            return "User{id=" + id + ", name='" + name + "', email='" + email + "'}";
        }
    }

    public static void main(String[] args) {
        // Create WebClient instance
        WebClient webClient = WebClient.create("https://api.example.com");

        // Make a GET request to fetch a user
        Mono<User> userMono = webClient.get()
                .uri("/users/1") // Specify the endpoint
                .header("Authorization", "Bearer your-token") // Add headers if needed
                .retrieve() // Execute the request
                .bodyToMono(User.class) // Map response to User class
                .onErrorResume(e -> {
                    // Handle errors (e.g., 404, 500)
                    System.err.println("Error occurred: " + e.getMessage());
                    return Mono.just(new User()); // Fallback to empty User
                });

        // Subscribe to the Mono to process the result
        userMono.subscribe(user -> {
            System.out.println("Fetched User: " + user);
        });

        // Block for demonstration purposes (avoid in production reactive apps)
        User user = userMono.block();
        System.out.println("Blocking call result: " + user);
    }
}

```

##### 🔵 Steps

- **Create a WebClient Instance:** Use `WebClient.create()` or `WebClient.builder()` to instantiate WebClient.
- **Build a Request:** Chain methods like `.get()`, `.post()`, `.uri()`, `.header()`, and `.body()` to construct the request.
- **Execute the Request:** Use `.retrieve()` or `.exchangeToMono()` to execute the request and process the response.
- **Handle the Response:** Use `Mono` or `Flux` to handle single or streaming responses, respectively.
- **Error Handling:** Use methods like `onStatus()` or `onErrorResume()` for handling errors.
