# Dependency Inversion Principle (DIP)

The **DIP** states that high-level modules should not depend on low-level modules; both should depend on abstractions. Abstractions should not depend on details. In a Spring Boot microservices context, DIP ensures loose coupling, allowing you to swap implementations (**e.g.**, `payment processors`) easily.

---

### Payment Example Demonstrating Dependency Inversion Principle (DIP) in Spring Boot

This example shows how `PaymentService` depends on the `PaymentProcessor` interface, not concrete implementations like `PayPalProcessor`.

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

##### PaymentConfig.java

```java
package com.example.payment;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class PaymentConfig {
    @Value("${payment.processor}")
    private String processorType;

    @Bean
    public PaymentProcessor paymentProcessor(PayPalProcessor payPalProcessor, StripeProcessor stripeProcessor) {
        if ("paypal".equalsIgnoreCase(processorType)) {
            return payPalProcessor;
        } else if ("stripe".equalsIgnoreCase(processorType)) {
            return stripeProcessor;
        }
        throw new IllegalArgumentException("Unknown processor type: " + processorType);
    }
}
```

---

##### DIP Adherence

- **Abstraction Dependency**: `PaymentService` depends on the `PaymentProcessor` interface, not concrete classes like `PayPalProcessor` or `StripeProcessor`.
- **Spring Boot Feature**: Spring’s dependency injection (via constructor injection) supports DIP:
  ```java
  @Bean
  public PaymentService paymentService(PaymentProcessor paymentProcessor, PaymentRepository paymentRepository) {
      return new PaymentService(paymentProcessor, paymentRepository);
  }
  ```
- **Flexibility**: Swap `PayPalProcessor` with `StripeProcessor` via configuration (e.g., `application.yml`) without modifying `PaymentService`.
- **Notes**: Use DIP to decouple microservices components, enabling easy testing (e.g., mocking `PaymentProcessor` with Mockito) and scalability.
