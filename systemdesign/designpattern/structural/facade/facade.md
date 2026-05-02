⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Facade Pattern

- A Facade provides a simple interface to a complex system (multiple classes, services, APIs).
- Instead of calling multiple services step-by-step, you call one method, and it internally handles everything.
- It provided Abstraction

##### 🟦 Real-world idea

- Think of a restaurant waiter:
  - You don’t go to kitchen, billing, chef, inventory separately
  - You just tell the waiter → he handles everything
- Waiter = Facade

### ➡️ Payment Example + Email

- Payemnt uses Facade
- Email uses strategy

##### 🟦 Flow Diagram

```text
Client
  ↓
Controller
  ↓
OrderFacade
  ↓
-----------------------------------
| OrderService                   |
| PaymentService                |
| NotificationService (Strategy)|
-----------------------------------
  ↓
Notification Channels:
   → EmailSender
   → SmsSender
```

##### 🔵 Execution Flow

- Controller → calls Facade
- Facade:
  - Order created
  - Payment done
  - Notification triggered
- NotificationService:
  - EmailSender.send()
  - SmsSender.send()

##### 🟦 Folder Structure

```text
com.app
 ├── controller
 │     └── OrderController
 │
 ├── facade
 │     └── OrderFacade
 │
 ├── service
 │     ├── OrderService
 │     ├── PaymentService
 │     └── NotificationService
 │
 ├── strategy
 │     ├── NotificationSender (interface)
 │     ├── EmailSender
 │     └── SmsSender
 │
 ├── model
```

##### 🔵 Controller

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    private final OrderFacade orderFacade;

    public OrderController(OrderFacade orderFacade) {
        this.orderFacade = orderFacade;
    }

    @PostMapping
    public String placeOrder() {
        orderFacade.placeOrder();
        return "Order placed";
    }
}
```

##### 🔵 Facade (Orchestration Layer)

```java
@Service
public class OrderFacade {

    private final OrderService orderService;
    private final PaymentService paymentService;
    private final NotificationService notificationService;

    public OrderFacade(OrderService orderService,
                       PaymentService paymentService,
                       NotificationService notificationService) {
        this.orderService = orderService;
        this.paymentService = paymentService;
        this.notificationService = notificationService;
    }

    public void placeOrder() {

        orderService.createOrder();

        paymentService.processPayment();

        notificationService.notifyUser(); // decoupled

    }
}
```

##### 🔵 Order Service

```java
@Service
public class OrderService {

    public void createOrder() {
        System.out.println("Order created");
    }
}
```

##### 🔵 Payment Service

```java
@Service
public class PaymentService {

    public void processPayment() {
        System.out.println("Payment done");
    }
}
```

- **Notification**

##### 🔵 Strategy Interface

```java
Strategy Interface
```

##### 🔵 Email Implementation

```java
@Service
public class EmailSender implements NotificationSender {

    @Override
    public void send() {
        System.out.println("Email sent");
    }
}
```

##### 🔵 SMS Implementation

```java
@Service
public class SmsSender implements NotificationSender {

    @Override
    public void send() {
        System.out.println("SMS sent");
    }
}
```

##### 🔵 Notification Service (Decoupler)

```java
@Service
public class NotificationService {

    private final List<NotificationSender> senders;

    public NotificationService(List<NotificationSender> senders) {
        this.senders = senders;
    }

    public void notifyUser() {
        for (NotificationSender sender : senders) {
            sender.send(); // all channels triggered
        }
    }
}
```

##### 🟦 Facade vs Strategy

- Facade = workflow orchestration & Strategy = pluggable behavior
- Facade → "Do all steps" & Strategy → "How to do one step"
- Facade is Abstraction & Strategy is Encapsulation

### ➡️ In production systems 🔴

- `Notification` is often **async** (**Kafka** / **Queue**)
- `Payment` is **transactional**
- `Facade` becomes **Orchestrator / Saga**
