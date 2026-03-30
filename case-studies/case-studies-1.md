⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ • ‣ → ⁕

# ⏺️ Design a Payment Processing System. Handle 10,000 transactions per second.

- What happens if your database goes down mid-transaction?
- What if the same payment request hits your server twice due to a network retry?

### ➡️ The naive approach (what everyone says):

- User hits Pay button
- API receives request
- Deduct balance from DB
- Return success
- This works in demo. Fails in production.

### ➡️ The real problems at scale:

##### 🟦 Duplicate Payments (Idempotency failure)

- Due to Network timeout, User clicks Pay twice Or your retry logic fires automatically. Same payment processes twice.

- **Fix:** Generate a unique idempotency key per transaction. Before processing — check if this key already exists. If yes → return previous result. Don't process again.

- Client sends: `idempotency_key`
- Store in **Redis**

```text
if (key exists):
   return previous response
else:
   process + store result
```

##### 🟦 Partial Failure (Broken Transactions)

- Money Deducted but Not Credited
  - Step 1: Deduct from sender
  - Step 2: Credit to receiver not happened (Due DB crash).
- **Fix:**
  - if single DB(same db) - Use database **ACID** transactions with rollback .
  - for distributed systems - implement the **Saga Pattern**, each step has a compensating action that reverses it on failure. (Each step has a rollback action)
  ```text
   Step 1: Debit
   Step 2: Credit
   If Step 2 fails:
     Compensation → Refund (undo debit)
  ```

##### 🟦 10,000 TPS(Transaction Per Second) hitting one database

- Your single DB becomes the bottleneck.
- **Fix:**
  - Separate your read and write databases (**CQRS Pattern**).
  - Use an event queue like Kafka between services, Process payments asynchronously — accept the request fast, settle in background.

#### ➡️ FLOW

```text
Client → API Gateway → Payment Service
                               ↓
                        Redis (Idempotency Check)
                               ↓
                          Kafka Queue
                               ↓
                            Workers
                               ↓
                               DB
                               ↓
                    Notification Service
```

###### 🟦 Client → API Gateway

- Handles:
  - Authentication
  - Rate limiting
- No business logic here
- API Gateway → Payment Service

###### 🟦 Payment Service → Redis (Idempotency Check)

- This is the key point
- Payment Service checks **Redis**:
  - “Have I already processed this request?”

```json
{
  "idempotency_key": "txn_123"
}
```

```js
if (redis.exists(key)) {
   return previous_response;
} else {
   redis.set(key, "processing");
   continue;
}
```

###### 🟦 Payment Service → Kafka

- Instead of hitting DB directly:
  - Push event to Kafka
  - **Example:** `"PROCESS_PAYMENT"`
- This makes system:
  - Asynchronous
  - Scalable

###### 🟦 Workers(Consumer services (backend apps)) → DB

- Workers consume Kafka messages
- Perform:
  - Debit from sender account
  - Credit to receiver account
  - If any step fails → trigger Refund Sender (undo debit)
- **Saga** = Managing the full transaction flow (debit + credit)
  with compensating actions (refund) to ensure consistency

###### 🟦 Notification Service

- Runs async
- Sends:
  - SMS / Email
  - Success/Failure
