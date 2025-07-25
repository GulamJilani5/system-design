# Single Responsibility Principle (SRP)

The **SRP** states that a class should have only one reason to change. In the context of a Spring Boot microservices application, this principle ensures that each class (**e.g.**, `controller`, `service`, `repository`) has a distinct role, making the codebase modular, maintainable, and easier to test.

---

### Payment Example Demonstrating Single Responsibility Principle (SRP) in Spring Boot

##### PaymentController.java

**Responsibility**: Handles HTTP requests and responses for payment-related endpoints.

```java
package com.example.payment;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/payments")
public class PaymentController {
    private final PaymentService paymentService;

    public PaymentController(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    @PostMapping
    public Payment createPayment(@RequestBody Payment payment) {
        return paymentService.processPayment(payment);
    }
}
```

- **SRP Adherence**: The `PaymentController` is responsible only for receiving HTTP requests (e.g., POST `/payments`) and delegating the business logic to `PaymentService`. It does not handle validation or data persistence, keeping its role focused.

---

##### PaymentService.java

**Responsibility**: Manages business logic for payment processing, such as validation.

```java
package com.example.payment;

import org.springframework.stereotype.Service;

@Service
public class PaymentService {
    private final PaymentRepository paymentRepository;

    public PaymentService(PaymentRepository paymentRepository) {
        this.paymentRepository = paymentRepository;
    }

    public Payment processPayment(Payment payment) {
        // Business logic for payment processing
        validatePayment(payment);
        return paymentRepository.save(payment);
    }

    private void validatePayment(Payment payment) {
        // Example validation logic
        if (payment.getAmount() <= 0) {
            throw new IllegalArgumentException("Payment amount must be positive");
        }
    }
}
```

- **SRP Adherence**: The `PaymentService` encapsulates the business logic for processing payments, such as validating the payment amount. It delegates data storage to `PaymentRepository` and does not handle HTTP concerns, maintaining a single responsibility.

---

##### PaymentRepository.java

**Responsibility**: Handles data persistence for the `Payment` entity.

```java
package com.example.payment;

import org.springframework.data.jpa.repository.JpaRepository;

public interface PaymentRepository extends JpaRepository<Payment, Long> {
}
```

- **SRP Adherence**: The `PaymentRepository` is responsible solely for database operations (e.g., saving or retrieving payments). It uses Spring Data JPA to abstract data access, keeping its role distinct from business logic or HTTP handling.

---

##### Payment.java

**Responsibility**: Represents the payment data model.

```java
package com.example.payment;

public class Payment {
    private Long id;
    private double amount;
    private String currency;
    private String status;

    // Getters and setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public double getAmount() {
        return amount;
    }

    public void setAmount(double amount) {
        this.amount = amount;
    }

    public String getCurrency() {
        return currency;
    }

    public void setCurrency(String currency) {
        this.currency = currency;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }
}
```

- **SRP Adherence**: The `Payment` class is a simple entity that represents the data structure for a payment. It has no business logic or persistence concerns, focusing solely on holding data.

---

##### How SRP is Applied in This Example

Each class in the payment microservice has a single, well-defined responsibility:

- **PaymentController**: Manages HTTP requests and responses, acting as the entry point for API calls.
- **PaymentService**: Handles business logic, such as validating payment details before saving.
- **PaymentRepository**: Manages database interactions, abstracting data persistence.
- **Payment**: Defines the data model, serving as a container for payment information.

This separation ensures that:

- Changes to one responsibility (e.g., adding new validation rules in `PaymentService`) do not affect other classes (e.g., `PaymentController` or `PaymentRepository`).
- The code is easier to test, as each class can be tested independently (e.g., mocking `PaymentRepository` in `PaymentService` tests).
- The microservice remains modular, aligning with the microservices architecture's goal of small, focused services.

---

##### Notes for Spring Boot Microservices Developers

- **Why SRP Matters**: In microservices, SRP aligns with the principle of building services that "do one thing well." By keeping classes focused, you reduce complexity and improve maintainability.
- **Spring Boot Features Supporting SRP**:
  - Use `@RestController`, `@Service`, and `@Repository` annotations to clearly define the role of each class.
  - Leverage dependency injection (e.g., constructor injection) to decouple classes, as shown in the example.
  - Use Spring Data JPA to keep data access separate from business logic.
- **Testing Tip**: Write unit tests for each class independently. For example, test `PaymentService` by mocking `PaymentRepository` using a framework like Mockito.
- **Extending the Example**: To add more functionality (e.g., integrating with a payment gateway), create a new class or interface (e.g., `PaymentGateway`) with its own responsibility, rather than adding logic to existing classes.
