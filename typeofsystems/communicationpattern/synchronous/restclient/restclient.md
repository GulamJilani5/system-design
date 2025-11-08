⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ RestClient

- RestClient itself is synchronous by nature, but it can internally use **WebClient** (`reactive engine`) and therefore can perform non-blocking async calls if configured.
- Replacement for **RestTemplate** (introduced in **Spring 6** & **Spring Boot 3**).
- A modern synchronous HTTP client but with a more fluent and clean API.
- Supports both blocking and non-blocking usage with better features than RestTemplate.
- To provide a modern & flexible alternative to RestTemplate without forcing reactive style.

### ➡️ RestClient Blocking Usage (default)

- This is the normal way to use RestClient — similar to RestTemplate.

```java
  RestClient client = RestClient.create();

String response = client.get()
        .uri("http://localhost:8081/api/users")
        .retrieve()
        .body(String.class);  // BLOCKS until response is received

System.out.println(response);

```

- `body(String.class)` blocks and returns the response.
- **Thread** waits until server responds.

### ➡️ RestClient Non-Blocking Usage (Async)

- **RestClient** can be configured to use **WebClient** as backend to make it **asynchronous**.

```java
  RestClient client = RestClient.builder()
        .requestFactory(new WebClientAdapter(WebClient.create()))
        .build();

Mono<String> responseMono = client.get()
        .uri("http://localhost:8081/api/users")
        .retrieve()
        .bodyToMono(String.class);  // NON-BLOCKING

responseMono.subscribe(response -> System.out.println("Response: " + response));

System.out.println("Request Sent!");

```

## ➡️ Advantages over RestTemplate:

- Fluent API (similar to WebClient)
- Supports filters & easier customization
- Future-proof replacement
