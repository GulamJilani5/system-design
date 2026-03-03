⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

### ➡️

# ⏺️ 1. RestTemplate

- **RestTemplate** is a synchronous HTTP client provided by Spring to make HTTP requests to other services.
  It’s a low-level, flexible API for performing RESTful communication.

## ➡️ Core Methods of RestTemplate

- exchange()
- getForEntity()
- getForEntity()
- postForObject()
- postForEntity()
- put()
- delete()

### 🟦 exchange()

- Production uses it heavily 🔴
- Fully customizable

```java
ResponseEntity<T> exchange(
    String url,
    HttpMethod method,
    HttpEntity<?> requestEntity,
    Class<T> responseType
)
```

- Handling List response

```java
ResponseEntity<List<User>> response =
    restTemplate.exchange(
        "http://localhost:8081/users",
        HttpMethod.GET,
        null,
        new ParameterizedTypeReference<List<User>>() {}
    );
// Extracting response details
List<User> users = response.getBody();
```

- Calling External Service with Headers (JWT Token)

- User DTO Class

```java
public class User {
    private Long id;
    private String name;
    private String email;
}
```

```java
// Header
HttpHeaders requestHeaders = new HttpHeaders();
requestHeaders.set("Authorization", "Bearer " + token);
requestHeaders.setContentType(MediaType.APPLICATION_JSON);

// Body
User userObject = new User();
userObject.setName("Gulam");
userObject.setEmail("gulam@gmail.com");

// Wrapping Header and body
HttpEntity<User> entity = new HttpEntity<>(userObject, requestHeaders);

ResponseEntity<User> response = restTemplate.exchange(
    "http://localhost:8081/users",
    HttpMethod.POST,
    entity,
    User.class
);

// Extracting response details
HttpStatus status = response.getStatusCode();
HttpHeaders responseHeaders = response.getHeaders();
User user = response.getBody();

```

##### 🔵 HttpEntity

- Wrapper for: `Body`, `Headers`

```java
HttpEntity<String> entity = new HttpEntity<>("body", headers);

// If no body
HttpEntity<Void> entity = new HttpEntity<>(headers);
```

### 🟦 getForObject()

- HTTP type `GET`
- Returns object

```java
<T> T getForObject(String url, Class<T> responseType, Object... uriVariables)
```

- url is API url
- responseType is Expected return class
- uriVariables means Path variables

```java
User user = restTemplate.getForObject(
    "http://localhost:8081/users/1",
    User.class
);
```

- No status code
- No headers

### 🟦 getForEntity()

- HTTP type `GET`
- Returns object
- Prefer for simple cases ⭕

```java
<T> ResponseEntity<T> getForEntity(String url, Class<T> responseType)
```

```java
ResponseEntity<User> response =
    restTemplate.getForEntity("http://localhost:8081/users/1", User.class);
User user = response.getBody();
HttpStatus status = response.getStatusCode();
HttpHeaders headers = response.getHeaders();
```

- Return `Body`, `Status Code`, `Headers`

### 🟦 postForObject()

- HTTP type `POST`
- Returns object

```java
User savedUser = restTemplate.postForObject(
    "http://localhost:8081/users",
    userObject,
    User.class
);
// Parameters: URL, Request body & Response type
```

- Returns: Only body

### 🟦 postForEntity()

- HTTP type `POST`
- Returns full response

```java
ResponseEntity<User> response =
    restTemplate.postForEntity(
        "http://localhost:8081/users",
        userObject,
        User.class
    );
```

- Returns: Full response

### 🟦 put()

- HTTP type `PUT`

```java

```

### 🟦 delete()

- HTTP type `DELETE`

```java

```

## ➡️ Error Handling in Production

- **By default:**
  - **4xx** → throws `HttpClientErrorException`
  - **5xx** → throws `HttpServerErrorException`

- Implement `ResponseErrorHandler`.

```java
try {
    restTemplate.getForObject(url, User.class);
} catch (HttpClientErrorException e) {
    System.out.println("Client Error: " + e.getStatusCode());
}
```

## ➡️ Key Components of RestTemplate Syntax

- **URL:** The endpoint of the REST API (e.g., `"https://api.example.com/data/{id}"`).
- **HTTP Method:** Specified via methods like **getForObject**, **postForEntity**, or **exchange** with **HttpMethod**.
- **Request Body:** The data sent to the server (**e.g.**, a `POJO` serialized to JSON).
- **Response Type:** The expected response type (**e.g.**, `String.class`, `MyResponse.class`).
- **URI Variables:** Placeholders in the URL (**e.g.**, `{id}`) replaced with actual values.
- **HttpEntity:** Encapsulates the request `body` and `headers`.
- **ResponseEntity:** Provides access to the response `body`, `headers`, and `status code`.

```java

//POJO Class
public class MyResponse {
    private int id;
    private String name;

    // Getters and setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}

// restTemplate
ResponseEntity<MyResponse> response = restTemplate.exchange(
    url,              // The URL of the REST endpoint
    HttpMethod.POST,  // The HTTP method (POST in this case)
    requestEntity,    // The HttpEntity containing request body and headers
    MyResponse.class, // The expected response type (class to deserialize the response into)
    123               // URI variables to replace placeholders in the URL
);

// Extracting the values from the response
MyResponse responseBody = response.getBody(); // Get the MyResponse object
    System.out.println("ID: " + responseBody.getId()); // Prints: ID: 1
    System.out.println("Name: " + responseBody.getName()); // Prints: Name: John
    System.out.println("Status: " + response.getStatusCode()); // Prints: Status: 200 OK
```

### 🟦 How It Works

- **Configuration:** Create a RestTemplate bean or instantiate it directly. Spring Boot auto-configures it if needed.
- **Usage:** Use methods like `getForObject`, `postForEntity`, `exchange`, etc., to make HTTP requests (`GET, POST, PUT, DELETE, etc.`).
- **Execution:** The client sends an HTTP request to the target service and waits for the response. The calling thread is blocked
  until the response is received.
- **Customization:** Supports configuration for timeouts, interceptors, and error handling via ClientHttpRequestFactory.

### 🟦 When to Use

- **Simple HTTP calls:** Suitable for straightforward REST API calls in microservices.
- **Legacy applications:** Commonly used in older Spring Boot projects or when fine-grained control over HTTP
  requests is needed.

##### 🔵 Pros

- Offers fine-grained control over HTTP requests (headers, query params, body, etc.).
- Built-in(Part of Spring Framework), no additional dependencies required.

##### 🔵 Cons

- **Boilerplate code:** Requires manual handling of headers, error responses, and serialization/deserialization.
- **Not declarative:** Lacks the clean, annotation-based approach of Feign Client.
- **Error handling:** Needs custom implementation for robust error handling (e.g., retries, circuit breakers).
