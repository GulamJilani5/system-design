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
