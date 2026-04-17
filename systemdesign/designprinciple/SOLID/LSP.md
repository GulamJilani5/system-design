⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Liskov Substitution Principle (LSP)

- LSP means any implementation of an interface should be replaceable without breaking the system.
- In Spring Boot, when we add a new implementation like **RazorpayProcessor**, it works seamlessly if it follows the same contract as existing implementations.
- However, if Razorpay introduces additional constraints like minimum amount validation that other implementations don’t have, then substituting it breaks behavior — which is a violation of LSP.

---

### ➡️ Payment Example Demonstrating Liskov Substitution Principle (LSP) in Spring Boot

This example reuses the `PaymentProcessor` interface from the OCP example, showing how `PayPalProcessor` and `StripeProcessor` can be substituted in `PaymentService` without affecting its behavior.

##### 🟦 PaymentProcessor.java (Interface)

```java
package com.example.payment;

public interface PaymentProcessor {
    void processPayment(Payment payment);
}
```

---

##### 🟦 PayPalProcessor.java

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

##### 🟦 StripeProcessor.java

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

##### 🟦 PaymentService.java

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

##### 🟦 PaymentController.java

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

##### 🟦 Payment.java (Entity)

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

##### 🟦 PaymentRepository.java

```java
package com.example.payment;

import org.springframework.data.jpa.repository.JpaRepository;

public interface PaymentRepository extends JpaRepository<Payment, Long> {
}
```

---

##### 🟦 LSP Adherence - If trying to add Razorpay. 🔴

- **It is correct**:
  - All implementations behave the same way (no surprises)
  - No extra conditions needed
  - No breaking changes in `PaymentService`
  - You can swap: PayPal → Stripe → Razorpay, without modifying service logic

```java
@Service
public class RazorpayProcessor implements PaymentProcessor {
    public void processPayment(double amount) {
        System.out.println("Paid via Razorpay");
    }
}
```

- **Violation Example**:
  - Razorpay introduces additional(extra) constraints like minimum amount validation that other implementations don’t have, then substituting it breaks behavior — which is a violation of LSP.

```java
@Service
public class RazorpayProcessor implements PaymentProcessor {

    public void processPayment(double amount) {
        if (amount < 100) {
            throw new RuntimeException("Minimum amount is 100");
        }
        System.out.println("Paid via Razorpay");
    }
}
```
