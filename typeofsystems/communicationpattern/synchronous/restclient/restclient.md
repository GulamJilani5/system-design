⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ RestClient

- RestClient itself is synchronous by nature, but it can internally use **WebClient** (`reactive engine`) and therefore can perform non-blocking async calls if configured.
- Replacement for **RestTemplate** (introduced in **Spring 6** & **Spring Boot 3**).
- A modern synchronous HTTP client but with a more fluent and clean API.
- Supports both blocking and non-blocking usage with better features than RestTemplate.
- To provide a modern & flexible alternative to RestTemplate without forcing reactive style.

### ➡️ RestClient Blocking Usage (default)

- Configure `RestClient` as Bean (Production Way)
- Instead of `RestClient.create()` every time, define a bean.

```java
@Configuration
public class RestClientConfig {

    @Bean
    public RestClient restClient() {
        return RestClient.builder()
                .baseUrl("http://localhost:8081")
                .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                .build();
    }
}
```

##### 🟦 Get Reqeust

```java
public List<UserDto> getAllUsers() {

    return restClient.get()
            .uri("/api/users")
            .retrieve()
            .body(new ParameterizedTypeReference<List<UserDto>>() {});
            // .body(List.class) It will return List<LinkedHashMap>
}
```

- Why ParameterizedTypeReference?
  - Because Java loses generic type info at runtime.
- `body(String.class)` blocks and returns the response.
- **Thread** waits until server responds.

##### 🟦 Post Request

```java
public UserDto createUser(CreateUserDto request) {

    return restClient.post()
            .uri("/api/users")
            .body(request)
            .retrieve()
            .body(UserDto.class);
}
```

- What happens internally?
  - `Object` → Converted to **JSON** (Jackson)
  - Sent in HTTP body
  - Response JSON → Converted back to `UserDto`

##### 🟦 Authentication (JWT Bearer Token)

- In real enterprise systems, services communicate using:
  - OAuth2, JWT, API Keys
- Option 1: Pass Token Per Request

```java
public List<UserDto> getUsers(String token) {

    return restClient.get()
            .uri("/api/users")
            .header(HttpHeaders.AUTHORIZATION, "Bearer " + token)
            .retrieve()
            .body(new ParameterizedTypeReference<List<UserDto>>() {});
}
```

- Option 2: Global Authentication (Best Practice)
  - If all calls need authentication:

```java
@Bean
public RestClient restClient() {
    return RestClient.builder()
            .baseUrl("http://localhost:8081")
            .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
            .requestInterceptor((request, body, execution) -> {
                request.getHeaders().setBearerAuth(getToken());
                return execution.execute(request, body);
            })
            .build();
}
```

##### 🟦 Real Enterprise Scenario (Microservices) 🔴

- Order Service → calls → User Service
- Auth handled via:
  - Keycloak
  - Okta
  - Spring Authorization Server

```java
Order Service
   ↓ (Bearer Token)
User Service
   ↓
Validates JWT
   ↓
Returns Data
```

##### 🟦 Error Handling (Production Important) 🔴

- Never ignore errors

```java
public List<UserDto> getUsers() {

    return restClient.get()
            .uri("/api/users")
            .retrieve()
            .onStatus(HttpStatusCode::is4xxClientError,
                (request, response) -> {
                    throw new RuntimeException("Client Error");
                })
            .onStatus(HttpStatusCode::is5xxServerError,
                (request, response) -> {
                    throw new RuntimeException("Server Error");
                })
            .body(new ParameterizedTypeReference<List<UserDto>>() {});
}
```

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
