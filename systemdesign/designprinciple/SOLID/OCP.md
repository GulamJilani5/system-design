# Open/Closed Principle (OCP)

The **OCP** states that classes should be open for extension but closed for modification. In a Spring Boot microservices context, this principle allows you to add new functionality (**e.g.**, `new payment gateways`) without altering existing code, ensuring scalability and maintainability.

---

### Payment Example Demonstrating Open/Closed Principle (OCP) in Spring Boot

This example demonstrates OCP using a **Payment** microservice that supports multiple payment gateways (**e.g.**, `PayPal`, `Stripe`) via the **Strategy pattern**. **New gateways** can be added by creating new implementations without modifying the core service logic.

##### PaymentProcessor.java (Interface)

```java
package com.example.payment;

public interface PaymentProcessor {
    void processPayment(Payment payment);
}
```

- **Responsibility**: Defines the contract for processing payments, allowing different implementations.

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

- **Responsibility**: Implements PayPal-specific payment processing.

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

- **Responsibility**: Implements Stripe-specific payment processing.

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

- **Responsibility**: Coordinates payment processing by delegating to a `PaymentProcessor` and saving the result.

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

## Configuration (application.yml)

```yaml
payment:
  processor: paypal
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

##### OCP Adherence

- **Open for Extension**: To add a new payment gateway (e.g., `Razorpay`), create a new class like `RazorpayProcessor` implementing `PaymentProcessor` and update the configuration. No changes are needed in `PaymentService`.
- **Closed for Modification**: The `PaymentService` class remains unchanged when adding new processors, as it depends on the `PaymentProcessor` interface.
- **Spring Boot Feature**: The `@Configuration` class and `@Bean` definition allow dynamic selection of processors based on properties, supporting OCP.
- **Notes**: Use Spring profiles or `@ConditionalOnProperty` to further enhance flexibility for different environments (e.g., dev, prod).
