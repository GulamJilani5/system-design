⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Factory Method

- Creates objects without exposing creation logic
- Returns object based on input or condition

## ➡️ Factory in Spring Boot

- Spring internally uses Factory Pattern in:

#### 🟦 Bean Creation

- `ApplicationContext.getBean()`
- Spring decides:
  - Which object to return
  - How to create it

```java
PaymentService service = context.getBean(PaymentService.class);
```

#### 🟦 @Bean Configuration

```java
@Configuration
public class AppConfig {

    @Bean
    public Payment payment() {
        return new UpiPayment(); // factory method
    }
}
```

#### 🟦 FactoryBean Interface (Advanced)

```java
public class MyFactoryBean implements FactoryBean<Payment> {
    public Payment getObject() {
        return new UpiPayment();
    }
}
```

## ➡️ Use Case - Real Project

#### 🟦 Payment Processing System

- `Client → Controller → Service → Factory → Implementation`
- We select: UPI / Card / NetBanking
- System creates correct processor
- Factory → decides which object
- Strategy → defines behavior

```text
com.project
│
├── controller
│   └── PaymentController.java
│
├── service
│   ├── PaymentService.java
│   └── impl
│       └── PaymentServiceImpl.java
│
├── factory
│   └── PaymentFactory.java
│
├── strategy (or payment)
│   ├── Payment.java
│   ├── UpiPayment.java
│   ├── CardPayment.java
│   └── NetBankingPayment.java
│
└── dto
    └── PaymentRequest.java
```

###### 🔵 Strategy Layer

- interface

```java
public interface Payment{
   void pay();
}
```

- Implementations

```java
public class UpiPayment implements Payment {
    public void pay() {
        System.out.println("Paid via UPI");
    }
}
```

```java
public class CardPayment implements Payment {
    public void pay() {
        System.out.println("Paid via Card");
    }
}
```

###### 🔵 Controller

- Responsibility:
  - Accept request (UPI / CARD)
  - Pass to service

```java
@RestController
@RequestMapping("/payment")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;

    @PostMapping("/pay")
    public String pay(@RequestBody PaymentRequest request) {
        return paymentService.process(request.getType());
    }
}
```

###### 🔵 Service

- Responsibility:
  - Business logic
  - Calls factory
  - No object creation logic
- Service Interface

```java
public interface PaymentService {
    String process(String type);
}
```

- ServiceImpl

```java
@Service
public class PaymentServiceImpl implements PaymentService {

    public String process(String type) {
        Payment payment = PaymentFactory.getPayment(type);
        payment.pay();
        return "Payment Successful";
    }
}
```

###### 🔵 Factory Layer

- Responsibility:
  - ONLY object creation
  - Centralized logic

```java
public class PaymentFactory {

    public static Payment getPayment(String type) {

        if (type.equalsIgnoreCase("UPI")) {
            return new UpiPayment();
        } else if (type.equalsIgnoreCase("CARD")) {
            return new CardPayment();
        }

        throw new IllegalArgumentException("Invalid payment type");
    }
}
```

```java
public class PaymentService {

    public void process(String type) {
        Payment payment = PaymentFactory.getPayment(type);
        payment.pay(); // UPI Payment
    }
}
```

- Usage

```java
Payment payment = PaymentFactory.getPayment("UPI");
payment.pay();
```

- Benefits:
  - No if-else in service layer
  - Easy to add new types (UPI, Razorpay, Wallet)

- Use when:
  - Object creation is complex.
  - Multiple implementations exist.
  - You want to follow OCP (**Open/Closed Principle**).

# ⏺️ Static Factory Method

- This pattenr has been used in my own application(KMP)
- `of()` is just a custom static method name used to create objects.
  - It replaces constructors for cleaner code

```java
public class SmsQuery {

    private String smsBody;
    private String phoneNumber;

    private SmsQuery(String smsBody, String phoneNumber) {
        this.smsBody = smsBody;
        this.phoneNumber = phoneNumber;
    }

    public static SmsQuery of(String smsBody, String phoneNumber) {
        return new SmsQuery(smsBody, phoneNumber);
    }
}
```

- usage

```java
SmsQuery query = SmsQuery.of("Hello", "9876543210");

```

- Instead of

```java
SmsQuery query = new SmsQuery("Hello", "9876543210");
```

## ➡️ Benefits of using of()

- More readable
- Can control object creation
- Can return cached objects
- Can hide complex logic

## ➡️ Factory in java(real world - inbuilt)

- `List.of(...)`
- `Map.of(...)`
- `Optional.of(...)`

## ➡️ Naming conventions

- We can use in our applications

| Method          | Meaning                     |
| --------------- | --------------------------- |
| `of()`          | Simple creation             |
| `from()`        | Convert from another object |
| `valueOf()`     | Parse/convert               |
| `getInstance()` | Singleton                   |
