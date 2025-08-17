## ➡️ 2. Java 11 HTTP Client API (synchronous + asyncchronous)

- **HttpClient:** The main entry point for sending requests and managing configurations.
- **HttpRequest:** Represents an HTTP request with method, URL, headers, and body.
- **HttpResponse:** Represents the server’s response, including status code, headers, and body.
- **BodyHandlers**
- **BodyPublishers**

```java

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HttpClientAsyncExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .header("Accept", "application/json")
            .GET()
            .build();

        CompletableFuture<HttpResponse<String>> futureResponse =
            client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

        futureResponse.thenApply(HttpResponse::body)
                      .thenAccept(body -> {
                          System.out.println("Response Body: " + body);
                      })
                      .exceptionally(ex -> {
                          System.err.println("Request failed: " + ex.getMessage());
                          return null;
                      });


          //No join() or manual delays in real-world Spring Boot apps.
        // Keep the program alive until async task finishes
        futureResponse.join();

         // Prevent the main thread from ending immediately
        // by keeping the program alive until async task finishes
        CompletableFuture.delayedExecutor(5, java.util.concurrent.TimeUnit.SECONDS)
                         .execute(() -> System.out.println("Main thread done."));

    }
}

```

- `sendAsync()` returns a `CompletableFuture` immediately instead of blocking until the request finishes.
- You can chain `.thenApply()` and `.thenAccept()` to process the result when it arrives.
- `exceptionally()` lets you handle errors without `try-catch`.

### In Real World Spring Boot

```java

// Controller
@RestController
public class ApiController {

    @Autowired
    private final ApiService apiService;

    @GetMapping("/data")
    public CompletableFuture<String> getData() {
        return apiService.fetchData();
    }
}

// Service
@Service
public class ApiService {
    private final HttpClient client = HttpClient.newHttpClient();

    public CompletableFuture<String> fetchData() {
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .header("Accept", "application/json")
            .GET()
            .build();

        return client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                     .thenApply(HttpResponse::body);
    }
}

```
