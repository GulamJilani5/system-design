⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️

##### 🟦 Critical

- We can use Asyhc to Offload the execution of the task from **Main thread** to the **wroker thread/Thread Pool** but it should be completed before moving ahead so we use `.get()`.
- It is not truly `async` because we wait using `.get()`, so it behaves like synchronous
- `.get()` is very much similar to `await` in javascript.

- Example: DB/API call
-

##### 🟦 Non-critical

- fire-and-forget, no need to use `.get()`
- Example: Email, logs

##### 🟦 For fully Async(Event-driven)

- For Large Scale System
- Example: RabbitMQ & Kafka

### ➡️ Folder Structure

```text

```

### ➡️ Flow Diagram

```text
✅✅ 🔥 FINAL FLOW (CRYSTAL CLEAR)

┌──────────────────────────────────────────────────────────────┐
│ STEP 1 (PARALLEL ✅)                                         │
│                                                              │
│   → fetchUser()        (async)                               │
│   → checkInventory()   (async)                               │
└──────────────────────────────────────────────────────────────┘

                ↓

┌──────────────────────────────────────────────────────────────┐
│ STEP 2 (WAIT)                                                │
│                                                              │
│   → join()                                                   │
└──────────────────────────────────────────────────────────────┘

                ↓

┌──────────────────────────────────────────────────────────────┐
│ STEP 3 (CRITICAL ✅)                                         │
│                                                              │
│   → processPayment()                                         │
│                                                              │
│     (async internally BUT .get() → behaves sync)             │
└──────────────────────────────────────────────────────────────┘

                ↓

┌──────────────────────────────────────────────────────────────┐
│ STEP 4 (CRITICAL ✅)                                         │
│                                                              │
│   → saveOrder()                                              │
│                                                              │
│     (async internally BUT .get())                            │
└──────────────────────────────────────────────────────────────┘

                ↓

┌──────────────────────────────────────────────────────────────┐
│ STEP 5 (NON-CRITICAL ✅)                                     │
│                                                              │
│   → sendEmail()                                              │
│                                                              │
│     (fire-and-forget async)                                  │
└──────────────────────────────────────────────────────────────┘

                ↓

┌──────────────────────────────────────────────────────────────┐
│ STEP 6                                                       │
│                                                              │
│   → return response                                          │
└──────────────────────────────────────────────────────────────┘
```

### ➡️ Code Execution

##### 🟦 Thread Pool Config

```java
@Configuration
@EnableAsync
public class ThreadPoolConfig {

    @Bean("fintechExecutor")
    public Executor fintechExecutor() {

        ThreadPoolTaskExecutor executor =
                new ThreadPoolTaskExecutor();

        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);

        executor.setThreadNamePrefix("Fintech-");

        executor.initialize();

        return executor;
    }

    @Bean("notificationExecutor")
    public Executor notificationExecutor() {

        ThreadPoolTaskExecutor executor =
                new ThreadPoolTaskExecutor();

        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(5);

        executor.setThreadNamePrefix("Notify-");

        executor.initialize();

        return executor;
    }
}
```

### ➡️ Controller

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    @Autowired
    private OrderOrchestratorService orderService;

    @PostMapping("/create")
    public ResponseEntity<String> createOrder(
            @RequestParam Long userId,
            @RequestParam Long productId,
            @RequestParam int quantity) throws Exception {

        String response =
                orderService.createOrder(userId, productId, quantity);

        return ResponseEntity.ok(response);
    }
}
```

### ➡️ Service

##### 🟦 OrderOrchestratorService

```java
@Service
public class OrderOrchestratorService {

    @Autowired
    private UserService userService;

    @Autowired
    private InventoryService inventoryService;

    @Autowired
    private PaymentService paymentService;

    @Autowired
    private OrderPersistenceService orderPersistenceService;

    @Autowired
    private EmailService emailService;

    public String createOrder(Long userId,
                              Long productId,
                              int quantity) throws Exception {

        // ✅ ASYNC (parallel tasks)

        CompletableFuture<User> userFuture =
                userService.fetchUser(userId); // async

        CompletableFuture<Boolean> inventoryFuture =
                inventoryService.checkInventory(productId, quantity); // async


        // ✅ Wait for independent tasks

        CompletableFuture
                .allOf(userFuture, inventoryFuture)
                .join();

        User user = userFuture.get();
        Boolean inStock = inventoryFuture.get();

        if (!inStock) {
            throw new RuntimeException("Out of stock");
        }


        // ✅ CRITICAL STEP (logically synchronous)

        String paymentStatus =
                paymentService
                        .processPayment(user, productId, quantity)
                        .get();

        // async execution internally
        // BUT we wait → behaves SYNCHRONOUSLY


        // ✅ SAVE ORDER (critical → wait)

        Order order =
                new Order(userId, productId, quantity, paymentStatus);

        orderPersistenceService
                .saveOrder(order)
                .get(); // async + wait


        // ✅ NON-CRITICAL (fire & forget)

        emailService.sendEmail(
                user.getEmail(),
                "Order placed successfully!"
        ); // async (no wait)


        return "Order success | Payment: " + paymentStatus;
    }
}
```

##### 🟦 UserService

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    @Async("fintechExecutor") // ✅ ASYNC
    public CompletableFuture<User> fetchUser(Long userId) {

        System.out.println(
                "User Thread: " +
                Thread.currentThread().getName()
        );

        User user = userRepository
                .findById(userId)
                .orElseThrow(() ->
                        new RuntimeException("User not found"));

        return CompletableFuture.completedFuture(user);
    }
}
```

##### 🟦 InventoryService

```java
@Service
public class InventoryService {

    @Autowired
    private InventoryRepository inventoryRepository;

    @Async("fintechExecutor") // ✅ ASYNC
    public CompletableFuture<Boolean> checkInventory(
            Long productId,
            int qty) {

        System.out.println(
                "Inventory Thread: " +
                Thread.currentThread().getName()
        );

        boolean available =
                inventoryRepository.checkStock(productId, qty);

        return CompletableFuture.completedFuture(available);
    }
}
```

##### 🟦 PaymentService

- It is logically synchronous but `async` is used only as an execution optimization

```java
@Service
public class PaymentService {

    @Autowired
    private RestTemplate restTemplate;

    @Async("fintechExecutor") // ✅ TECHNICALLY ASYNC
    public CompletableFuture<String> processPayment(
            User user,
            Long productId,
            int qty) {

        System.out.println(
                "Payment Thread: " +
                Thread.currentThread().getName()
        );

        String response = restTemplate.postForObject(
                "https://payment-gateway/api/pay",
                user,
                String.class
        );

        return CompletableFuture.completedFuture(response);
    }
}
```

##### 🟦 OrderPersistenceService

```java
@Service
public class OrderPersistenceService {

    @Autowired
    private OrderRepository orderRepository;

    @Async("fintechExecutor") // ✅ ASYNC
    public CompletableFuture<Void> saveOrder(Order order) {

        System.out.println(
                "Save Order Thread: " +
                Thread.currentThread().getName()
        );

        orderRepository.save(order);

        return CompletableFuture.completedFuture(null);
    }
}
```

##### 🟦 EmailService

```java
@Service
public class EmailService {

    @Async("fintechExecutor") // ✅ ASYNC (fire & forget)
    public void sendEmail(String email, String message) {

        System.out.println(
                "Email Thread: " +
                Thread.currentThread().getName()
        );

        // No blocking required
        System.out.println("Email sent to: " + email);
    }
}
```

### ➡️ ThreadPool Config

```java
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean("fintechExecutor")
    public Executor executor() {

        ThreadPoolTaskExecutor executor =
                new ThreadPoolTaskExecutor();

        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(100);

        executor.setThreadNamePrefix("Fintech-");

        executor.initialize();

        return executor;
    }
}
```
