⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Builder Pattern in Spring Boot

- In Spring Boot, builder is mostly used for:
  - DTOs
  - Response objects
  - Request objects
  - Kafka events
  - Entity creation
  - Immutable objects
  - Test objects

### ➡️ Step 1 — DTO Class

- Using **Lombok**

```java
import lombok.Builder;
import lombok.Getter;

@Getter
@Builder
public class OrderResponse {

    private String orderId;
    private String status;
    private String message;
    private double amount;
}
```

##### 🟦 What @Builder Does Internally

- Lombok generates ALL builder code automatically during compilation.
- Lombok automatically generates:
  - builder() method
  - Builder class
  - chaining methods
  - build() method
  - constructor wiring

###### 🔵 builder() Method

- It is inside the **OrderResponse** class

```java
public static Builder builder() {
    return new Builder();
}
```

###### 🔵 Builder Class

- It is static internal class in the **OrderResponse** class

```java
public static class Builder {

    private String orderId;
    private String status;
    private String message;
    private double amount;


    public Builder orderId(String orderId) {
        this.orderId = orderId;
        return this;
    }

    public Builder status(String status) {
        this.status = status;
        return this;
    }

    public Builder message(String message) {
        this.message = message;
        return this;
    }

    public Builder amount(double amount) {
        this.amount = amount;
        return this;
    }

    public OrderResponse build() {
        return new OrderResponse(this);
    }
}
```

### ➡️ Step 2 — Controller Layer

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    @Autowired
    private OrderService orderService;

    @PostMapping
    public OrderResponse createOrder() {

        return orderService.createOrder();
    }
}
```

### ➡️ Step 3 — Service Layer

```java
@Service
public class OrderService {

    public OrderResponse createOrder() {

        OrderResponse response = OrderResponse.builder()
                .orderId("ORD-101")
                .status("SUCCESS")
                .message("Order created")
                .amount(5000)
                .build();

        return response;
    }
}
```

### ➡️

```java

```
