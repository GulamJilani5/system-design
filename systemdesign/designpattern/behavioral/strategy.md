⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Strategy Pattern

- Defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from clients that use it.
- COMBINED UNDERSTANDING Of Command & Strategy used in my KMP code🔴
  - Find `D:\Jilani\learning\system design\systemdesign\designpattern\behavioral\command.md`

### ➡️ Key Components:

##### 🟦 Context

- Uses a strategy.

##### 🟦 Strategy

- Interface for different algorithms.

```java
@Service
public interface ServiceWrapper<V, Q> {
    V execute(Q var1);
}
```

- Q → Input (Query / Request / Command)
- V → Output (Response / Result)

##### 🟦 ConcreteStrategy

- Implements the algorithm.

```java
sendSmsWrapper.execute(SmsQuery)
sendEmailWrapper.execute(EmailQuery)
sendPortalWrapper.execute(PortalQuery)
```

- Same method → Different behavior

### ➡️ Flow (Very Important for Interview)

- Choose strategy
  - SMS / Email / Portal
- Call common method
  - execute()
- Different logic runs internally

### ➡️ Why Use Strategy Pattern

- Avoids large if-else / switch
- Easy to add new behavior
- Clean separation of logic

### ➡️ Real-world Analogy:

- Notification systems (SMS, Email, Portal Message) (Our KMP CASE)
- Payment methods (UPI, Card, NetBanking)
- Sorting algorithms

### ➡️ Shopping Cart Example

- In Strategy Pattern, we always use interface reference (PaymentStrategy) so that different implementations can be swapped dynamically at runtime."

```text
com.example.payment
│
├── controller
│   └── PaymentController.java
│
├── service
│   ├── PaymentService.java
│   ├── PaymentServiceImpl.java
│   │
│   └── strategy
│       ├── PaymentStrategy.java
│       ├── CreditCardPayment.java
│       ├── PayPalPayment.java
│       └── UpiPayment.java   (future)
│
├── factory   (optional but recommended)
│   └── PaymentStrategyFactory.java
│
├── repository   (only if DB is used)
│   └── PaymentRepository.java
│
└── model / dto
    └── PaymentRequest.java
```

##### Flow

- Client = External user / UI / API caller

```text
Client (Frontend / Postman)(POST /payment?type=creditcard&amount=100)
        ↓
Controller
        ↓
Service (Context)
        ↓
Factory
        ↓
Strategy
```

##### 🟦 Controller → API Layer

```java
@RestController
@RequestMapping("/payment")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;

    @PostMapping
    public String pay(@RequestParam String type, @RequestParam int amount) {
        return paymentService.processPayment(type, amount);
    }
}
```

##### 🟦 Service Layer → Uses Strategy

- Interface

```java
public interface PaymentService {
    String processPayment(String type, int amount);
}
```

- Implementation

```java
@Service
public class PaymentServiceImpl implements PaymentService {

    @Autowired
    private PaymentStrategyFactory factory;

    @Override
    public String processPayment(String type, int amount) {
        PaymentStrategy strategy = factory.getStrategy(type);
        strategy.pay(amount);
        return "Payment Successful";
    }
}
```

##### 🟦 Strategy Interface

```java
public interface PaymentStrategy {
        void pay(int amount);
}
```

##### 🟦 Strategy Implementations

```java
@Service("creditcard")
public class CreditCardPayment implements PaymentStrategy {

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}
```

```java
@Service("paypal")
public class PayPalPayment implements PaymentStrategy {

    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

```

##### 🟦 Factory

```java
@Component
public class PaymentStrategyFactory {

    private final Map<String, PaymentStrategy> strategyMap;

    public PaymentStrategyFactory(Map<String, PaymentStrategy> strategyMap) {
        this.strategyMap = strategyMap;
    }

    public PaymentStrategy getStrategy(String type) {
        PaymentStrategy strategy = strategyMap.get(type.toLowerCase());

        if (strategy == null) {
            throw new IllegalArgumentException("Invalid payment type: " + type);
        }

        return strategy;
    }
}
```

##### 🟦 PaymentServiceImpl

- PaymentServiceImpl = Context
- It does not care how payment happens

```java
@Service
public class PaymentServiceImpl {

    private final PaymentStrategyFactory factory;

    public PaymentServiceImpl(PaymentStrategyFactory factory) {
        this.factory = factory;
    }

    public String checkout(String type, int amount) {
        PaymentStrategy strategy = factory.getStrategy(type);
        strategy.pay(amount);
        return "Payment Successful";
    }
}
```

##### 🟦 Runtime Behavior Change

- Client make request
  - POST /payment?type=creditcard&amount=100
  - POST /payment?type=paypal&amount=100
- **PaymentController** read the `type` and `amount` and call service **PaymentServiceImpl**
- Service has **PaymentStrategy**

```java
PaymentStrategy strategy = factory.getStrategy("creditcard");
strategy.pay(100);
```

- OR

```java
PaymentStrategy strategy = factory.getStrategy("paypal");
strategy.pay(100);
```

- Behavior changes at runtime → core idea of Strategy Pattern
- creditCard payment or Paypal payment

##### 🟦 If we wanted to add UpiPayment

- We can add

```java
class UpiPayment implements PaymentStrategy { ... }
```

- No change in Service
- No change in Factory
- Plug-and-play
- Open/Closed Principle (OCP) 🔴
