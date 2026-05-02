⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ SAGA Pattern

- Real Use Case
  - Find `D:\Jilani\learning\system design\case-studies\case-studies-1.md`
- “If something fails → undo what I already did”
  - Saga = Do step-by-step + Undo if failure
- Break transaction into steps
  - If any step fails → run undo (compensation)

```text
Debit → Credit
   ↓       ↓
 Success  Failure
             ↓
         Refund (Undo)
```

### ➡️ Worker consuming Kafka

```java
@KafkaListener(topics = "payment-events")
public void processPayment(PaymentEvent event) {

    try {
        // Step 1: Debit
        accountService.debit(event.getFromAccount(), event.getAmount());

        // Step 2: Credit
        accountService.credit(event.getToAccount(), event.getAmount());

    } catch (Exception e) {
        // Compensation (Undo)
        accountService.refund(event.getFromAccount(), event.getAmount());
    }
}
```

### ➡️ Payment Example

- Order of execution:
- `Controller → Orchestrator → Steps → Failure → Compensation`

##### 🟦 FLOW DIAGRAM (Execution Order)

```text
Client
  ↓
Controller (Entry Point)
  ↓
OrderSagaOrchestrator
  ↓
---------------------------------
| Step 1: Create Order          |
| Step 2: Process Payment       |
| Step 3: Reserve Inventory     |
| Step 4: Confirm Order         |
---------------------------------
          ↓
     SUCCESS ✅

If any step fails ❌
          ↓
---------------------------------
| Compensation (Reverse Order)  |
| Cancel Order                  |
| Refund Payment                |
| Release Inventory             |
---------------------------------
```

##### 🟦 FILE / FOLDER STRUCTURE

```text
com.example.saga
│
├── controller
│   └── OrderController.java
│
├── orchestrator
│   └── OrderSagaOrchestrator.java
│
├── model
│   └── SagaState.java
│   └── OrderRequest.java
│
├── service
│   └── (WebClient logic can be here)
│
└── config
    └── WebClientConfig.java
```

##### 🟦 SagaState Class

- Tracks progress of saga
- Stores intermediate results (orderId)
- Used for compensation decision

```java
public class SagaState {

    private Long orderId;

    private boolean orderCreated;
    private boolean paymentDone;
    private boolean inventoryReserved;

    // Getters
    public boolean isOrderCreated() { return orderCreated; }
    public boolean isPaymentDone() { return paymentDone; }
    public boolean isInventoryReserved() { return inventoryReserved; }

    // Setters
    public void setOrderId(Long orderId) { this.orderId = orderId; }
    public void setOrderCreated(boolean orderCreated) { this.orderCreated = orderCreated; }
    public void setPaymentDone(boolean paymentDone) { this.paymentDone = paymentDone; }
    public void setInventoryReserved(boolean inventoryReserved) { this.inventoryReserved = inventoryReserved; }

    public Long getOrderId() { return orderId; }
}
```

##### 🟦 Controller (Entry Point)

- Entry point of saga
- Receives client request
- Delegates to orchestrator

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    private final OrderSagaOrchestrator orchestrator;

    public OrderController(OrderSagaOrchestrator orchestrator) {
        this.orchestrator = orchestrator;
    }

    @PostMapping
    public String createOrder(@RequestBody OrderRequest request) {
        return orchestrator.placeOrder(request);
    }
}
```

##### 🟦 Orchestrator – OrderSagaOrchestrator

- Calls services sequentially
- Tracks each step using SagaState
- On failure → triggers compensation

```java
public class OrderSagaOrchestrator {

    private WebClient webClient;

    public String placeOrder(OrderRequest request) {

        SagaState saga = new SagaState();

        try {
            // STEP 1
            Long orderId = createOrder(request, saga);

            // STEP 2
            processPayment(request, saga);

            // STEP 3
            reserveInventory(request, saga);

            // STEP 4
            confirmOrder(orderId);

            return "ORDER SUCCESS";

        } catch (Exception e) {
            compensate(saga, request);
            return "ORDER FAILED - ROLLED BACK";
        }
    }
}
```

- **Each Step using WebClient**

- These methods are defined inside the `OrderSagaOrchestrator` class
- WebClient calling respective microservices

###### 🔵 STEP 1: Create Order

- Calls Order Service
- Stores orderId
- Marks step as completed

```java
private Long createOrder(OrderRequest req, SagaState saga) {

    Long orderId = webClient.post()
            .uri("http://order-service/create")
            .bodyValue(req)
            .retrieve()
            .bodyToMono(Long.class)
            .block();

    saga.setOrderId(orderId);
    saga.setOrderCreated(true);

    return orderId;
}
```

###### 🔵 STEP 2: Process Payment

- Calls Payment Service
- Updates saga state
- Throws exception if fails

```java
private void processPayment(OrderRequest req, SagaState saga) {

    webClient.post()
            .uri("http://payment-service/pay")
            .bodyValue(req)
            .retrieve()
            .bodyToMono(String.class)
            .block();

    saga.setPaymentDone(true);
}
```

###### 🔵 STEP 3: Reserve Inventory

- Reserves stock
- Marks inventory step complete
- Failure triggers rollback

```java
private void reserveInventory(OrderRequest req, SagaState saga) {

    webClient.post()
            .uri("http://inventory-service/reserve")
            .bodyValue(req)
            .retrieve()
            .bodyToMono(String.class)
            .block();

    saga.setInventoryReserved(true);
}
```

###### 🔵 STEP 4: Confirm Order

- Final step of saga
- Confirms successful order
- No state update needed

```java
private void confirmOrder(Long orderId) {

    webClient.put()
            .uri("http://order-service/confirm/" + orderId)
            .retrieve()
            .bodyToMono(String.class)
            .block();
}
```

##### 🟦 Compensation Logic (Reverse Order)

- Executes in reverse order
- Uses SagaState flags
- Only completed steps are rolled back

```java
private void compensate(SagaState saga, OrderRequest req) {

    // Step 3 rollback
    if (saga.isInventoryReserved()) {
        webClient.post()
                .uri("http://inventory-service/release")
                .bodyValue(req)
                .retrieve()
                .bodyToMono(String.class)
                .block();
    }

    // Step 2 rollback
    if (saga.isPaymentDone()) {
        webClient.post()
                .uri("http://payment-service/refund")
                .bodyValue(req)
                .retrieve()
                .bodyToMono(String.class)
                .block();
    }

    // Step 1 rollback
    if (saga.isOrderCreated()) {
        webClient.post()
                .uri("http://order-service/cancel/" + saga.getOrderId())
                .retrieve()
                .bodyToMono(String.class)
                .block();
    }
}
```
