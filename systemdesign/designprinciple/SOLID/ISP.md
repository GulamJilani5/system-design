# Interface Segregation Principle (ISP)

The **ISP** states that clients should not be forced to depend on interfaces they don’t use. In a Spring Boot microservices context, ISP ensures that interfaces are focused, reducing coupling and improving maintainability.

---

### Payment Example Demonstrating Interface Segregation Principle (ISP) in Spring Boot

This example splits a bloated interface into smaller, focused ones for payment processing and reporting, ensuring clients only depend on what they need.

##### PaymentProcessor.java (Interface)

```java
package com.example.payment;

public interface PaymentProcessor {
    void processPayment(Payment payment);
}
```

---

##### PaymentReporter.java (Interface)

```java
package com.example.payment;

public interface PaymentReporter {
    String generateReport(Payment payment);
}
```

---

##### PayPalProcessor.java

```java
package com.example.payment;

import org.springframework.stereotype.Component;

@Component
public class PayPalProcessor implements PaymentProcessor, PaymentReporter {
    @Override
    public void processPayment(Payment payment) {
        // PayPal-specific logic
        payment.setStatus("PROCESSED_BY_PAYPAL");
    }

    @Override
    public String generateReport(Payment payment) {
        return "PayPal Report: Payment of " + payment.getAmount() + " processed.";
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
    private final PaymentReporter paymentReporter;
    private final PaymentRepository paymentRepository;

    public PaymentService(PaymentProcessor paymentProcessor, PaymentReporter paymentReporter, PaymentRepository paymentRepository) {
        this.paymentProcessor = paymentProcessor;
        this.paymentReporter = paymentReporter;
        this.paymentRepository = paymentRepository;
    }

    public Payment processPayment(Payment payment) {
        paymentProcessor.processPayment(payment);
        return paymentRepository.save(payment);
    }

    public String generatePaymentReport(Payment payment) {
        return paymentReporter.generateReport(payment);
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

##### ISP Adherence

- **Focused Interfaces**: `PaymentProcessor` and `PaymentReporter` are separate interfaces. Clients needing only processing (e.g., `PaymentService` for processing) don’t depend on reporting methods.
- **Example Benefit**: `StripeProcessor` implements only `PaymentProcessor` because it doesn’t support reporting, avoiding forced implementation of unused methods.
- **Spring Boot Feature**: Use Spring Data’s custom repository interfaces or Feign clients to define focused interfaces:
  ```java
  public interface PaymentQueryRepository {
      List<Payment> findByStatus(String status);
  }
  ```
- **Notes**: Avoid bloated interfaces (e.g., combining processing and reporting). Use ISP to keep microservices APIs clean and focused, especially for inter-service communication.
