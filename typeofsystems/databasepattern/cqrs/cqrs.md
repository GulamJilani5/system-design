⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ CQRS(Command Query Responsibility Segregation) Pattern

- Real Use Case
  - Find `D:\Jilani\learning\system design\case-studies\case-studies-1.md`
- “Writing and reading are different jobs — separate them”
  - CQRS = Separate Read & Write
- **ATM:** Real Use Case
  - Withdraw money → Write
  - Check balance → Read
- Same system, different purpose

```text
Write (Command) → Payment Service → DB
Read (Query)   → Read Service → Read DB
```

### ➡️ Write API (Command)

```java
@RestController
@RequestMapping("/payments")
public class PaymentController {

    @PostMapping
    public String makePayment(@RequestBody PaymentRequest request) {
        paymentService.processPayment(request);
        return "Payment initiated";
    }
}
```

### ➡️ Read API (Query)

```java
@RestController
@RequestMapping("/transactions")
public class TransactionController {

    @GetMapping("/{id}")
    public Transaction getTransaction(@PathVariable String id) {
        return transactionService.getTransactionById(id);
    }
}
```
