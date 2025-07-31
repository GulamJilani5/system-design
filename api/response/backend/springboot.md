⏺️ # API Response from the Backend (Spring Boot)

In a Spring Boot backend, API responses are crafted using controllers, and the response includes headers, body, status codes, and additional metadata.
🟩 🟢 🔷 🔹 🔵 🟦 ⏺️ ➡️

### ➡️ 1. Headers

The server can send headers to provide metadata, control caching, or manage authentication.

##### 🟦 What Can Be Sent:

Content-Type: Specifies the response body format (e.g., application/json).
Set-Cookie: Sends cookies to the client for session management.
Location: Provides the URL of a newly created resource (e.g., after a POST).
Cache-Control: Controls caching (e.g., no-cache, max-age=3600).
Access-Control-Allow- Headers\*: For CORS (e.g., Access-Control-Allow-Origin, Access-Control-Allow-Credentials).
Custom Headers: For API-specific metadata (e.g., X-Rate-Limit: 100).

##### 🟦 How to Send in Spring Boot:

```java
Using ResponseEntity:
javaimport org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
@GetMapping("/users")
public ResponseEntity<User> getUser() {
HttpHeaders headers = new HttpHeaders();
headers.add("Content-Type", "application/json");
headers.add("X-Custom-Header", "value");
headers.add("Cache-Control", "max-age=3600");

        User user = new User("john_doe");
        return new ResponseEntity<>(user, headers, HttpStatus.OK);
    }

}
Using @ResponseHeader:
javaimport org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
@GetMapping("/users")
@ResponseHeader(name = "X-Custom-Header", value = "value")
public User getUser() {
return new User("john_doe");
}
}

```

##### 🟦 Constraints:

Ensure CORS headers are set for cross-origin requests.
Avoid sensitive data in headers (e.g., tokens should be in HttpOnly cookies or body).
Large headers can impact performance.

### ➡️ 2. Body

The response body contains the data returned to the client, such as resources, error messages, or metadata.

##### 🟦 What Can Be Sent:

JSON: Most common for REST APIs (e.g., a user object or list).
XML: For legacy systems.
Text: For simple responses.
Binary Data: For files (e.g., images, PDFs).

##### 🟦 How to Send in Spring Boot:

- 🔵 **JSON:**

```java
java@GetMapping("/users")
public User getUser() {
return new User("john_doe", "john@example.com");
}
Spring Boot automatically serializes the User object to JSON with Content-Type: application/json.
File Download:
javaimport org.springframework.core.io.ByteArrayResource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class FileController {
@GetMapping("/download")
public ResponseEntity<ByteArrayResource> downloadFile() {
byte[] fileContent = "Sample file content".getBytes();
ByteArrayResource resource = new ByteArrayResource(fileContent);

        HttpHeaders headers = new HttpHeaders();
        headers.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=sample.txt");

        return ResponseEntity.ok()
                .headers(headers)
                .contentType(MediaType.APPLICATION_OCTET_STREAM)
                .body(resource);
    }

}

Error Response:
java@GetMapping("/users/{id}")
public ResponseEntity<?> getUser(@PathVariable Long id) {
if (id == null) {
return ResponseEntity.status(HttpStatus.BAD_REQUEST)
.body(new ErrorResponse("Invalid user ID"));
}
return ResponseEntity.ok(new User("john_doe"));
}
```

##### 🟦 Constraints:

Ensure the Content-Type matches the body format.
Large responses may require pagination or streaming.
Use appropriate status codes (e.g., 204 No Content for empty responses).

### ➡️ 3. Credentials

The server can send credentials or manage authentication in responses.

##### 🟦 What Can Be Sent:

- **Cookies:** For session-based authentication (Set-Cookie header).
- **Tokens:** Returned in the body (e.g., JWT after login) or headers.
- **CSRF Tokens:** For protecting against cross-site request forgery.

##### 🟦 How to Send in Spring Boot:

- **🔵Cookies:**

```java

javaimport javax.servlet.http.Cookie;

@RestController
public class AuthController {
@GetMapping("/login")
public ResponseEntity<String> login() {
Cookie cookie = new Cookie("sessionId", "abc123");
cookie.setHttpOnly(true);
cookie.setSecure(true);
cookie.setMaxAge(3600);

        HttpHeaders headers = new HttpHeaders();
        headers.add(HttpHeaders.SET_COOKIE, cookie.toString());

        return new ResponseEntity<>("Login successful", headers, HttpStatus.OK);
    }

}
```

- **🔵JWT Token in Body:**

```java
  @PostMapping("/login")
  public ResponseEntity<Map<String, String>> login(@RequestBody LoginRequest request) {
  String token = generateJwtToken(request.getUsername());
  Map<String, String> response = Map.of("token", token);
  return ResponseEntity.ok(response);
  }
```

##### 🟦 Constraints:

Use HttpOnly and Secure flags for cookies to prevent XSS and ensure HTTPS.
Avoid sending sensitive data in headers unless necessary.
Include CORS headers (Access-Control-Allow-Credentials) for cookie-based auth.

4. Additional Configurations
   The server can include additional response components:

##### 🟦 Status Codes: Set explicitly to indicate the result.

java@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
return new ResponseEntity<>(user, HttpStatus.CREATED); // 201 Created
}

##### 🟦 CORS Configuration:

```java

javaimport org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "http://localhost:3000", allowCredentials = "true")
public class UserController {
@GetMapping("/users")
public List<User> getUsers() {
return List.of(new User("john_doe"));
}
}

```

#### 🟦 Pagination:

Use headers or body to provide pagination metadata.

```java

@GetMapping("/users")
public ResponseEntity<List<User>> getUsers(@RequestParam int page, @RequestParam int size) {
HttpHeaders headers = new HttpHeaders();
headers.add("X-Total-Count", "100");
headers.add("Link", "<next_page_url>; rel=\"next\"");

    List<User> users = fetchUsers(page, size);
    return new ResponseEntity<>(users, headers, HttpStatus.OK);

}

```

##### 🟦 Rate Limiting:

Use headers to inform clients of limits.

```java
javaheaders.add("X-Rate-Limit-Remaining", "99");
headers.add("X-Rate-Limit-Reset", "1631234567");
```
