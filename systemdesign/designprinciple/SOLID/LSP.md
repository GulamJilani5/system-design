# Liskov Substitution Principle (LSP)

The **LSP** states that subtypes must be substitutable for their base types without altering the correctness of the program. In a Spring Boot microservices context, LSP ensures that different implementations of an interface (**e.g.**, `payment processors`) can be swapped without breaking the application.

---

### Payment Example Demonstrating Liskov Substitution Principle (LSP) in Spring Boot

This example reuses the `PaymentProcessor` interface from the OCP example, showing how `PayPalProcessor` and `StripeProcessor` can be substituted in `PaymentService` without affecting its behavior.

##### PaymentProcessor.java (Interface)

```java
package com.example.payment;

public interface PaymentProcessor {
    void processPayment(Payment payment);
}
```

---

##### PayPalProcessor.java

```java
package com.example.payment;

import org.springframework.stereotype.Component;

@Component
public class PayPalProcessor implements PaymentProcessor {
    @Override
    public void processPayment(Payment payment) {
        // PayPal-specific logic
        payment.setStatus("PROCESSED_BY_PAYPAL");
    }
}
```

---

##### StripeProcessor.java

```java
package com.example.payment;

import org.springframework.stereotype.Component;

@Component
public class StripeProcessor implements PaymentProcessor {
    @Override
    public void processPayment(Payment payment) {
        // Stripe-specific logic
        payment.setStatus("PROCESSED_BY_STRIPE");
    }
}
```

---

##### PaymentService.java

```java
package com.example.payment;

import org.springframework.stereotype.Service;

@Service
public class PaymentService {
    private final PaymentProcessor paymentProcessor;
    private final PaymentRepository paymentRepository;

    public PaymentService(PaymentProcessor paymentProcessor, PaymentRepository paymentRepository) {
        this.paymentProcessor = paymentProcessor;
        this.paymentRepository = paymentRepository;
    }

    public Payment processPayment(Payment payment) {
        paymentProcessor.processPayment(payment);
        return paymentRepository.save(payment);
    }
}
```

---

##### PaymentController.java

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

---

##### Payment.java (Entity)

```java
package com.example.payment;

public class Payment {
    private Long id;
    private double amount;
    private String currency;
    private String status;

    // Getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public double getAmount() { return amount; }
    public void setAmount(double amount) { this.amount = amount; }
    public String getCurrency() { return currency; }
    public void setCurrency(String currency) { this.currency = currency; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
}
```

---

##### PaymentRepository.java

```java
package com.example.payment;

import org.springframework.data.jpa.repository.JpaRepository;

public interface PaymentRepository extends JpaRepository<Payment, Long> {
}
```

---

##### LSP Adherence

- **Substitutability**: Both `PayPalProcessor` and `StripeProcessor` implement `PaymentProcessor` and can be injected into `PaymentService` without changing its behavior. Both set the payment status appropriately, adhering to the contract.
- **Spring Boot Feature**: Use `@Qualifier` or `@Primary` to select specific implementations:

  ```java
  @Service
  public class PaymentService {
      private final PaymentProcessor paymentProcessor;

      public PaymentService(@Qualifier("payPalProcessor") PaymentProcessor paymentProcessor, PaymentRepository paymentRepository) {
          this.paymentProcessor = paymentProcessor;
          this.paymentRepository = paymentRepository;
      }
      // ...
  }
  ```

- **Violation Example**: If `StripeProcessor` required a different `Payment` structure or threw unexpected exceptions, it would violate LSP, as substituting it for `PayPalProcessor` would break `PaymentService`.
- **Notes**: Ensure all implementations adhere to the interface contract. Test substitutability using unit tests with mocking frameworks like Mockito.
