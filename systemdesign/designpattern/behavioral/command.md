⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Command Pattern

- Command Pattern encapsulates a request as an object, allowing you to parameterize clients with different requests and decouple sender from receiver.

### ➡️ Key Components

##### 🟦 Command

- `SmsQuery`, `EmailQuery`, `PortalQuery`

```java
public class SmsQuery {
    private String message;
    private String phoneNumber;

    public static SmsQuery of(String message, String phoneNumber) {
        SmsQuery query = new SmsQuery();
        query.setMessage(message);
        query.setPhoneNumber(phoneNumber);
        return query;
    }
}
```

##### 🟦 Invoker

- `sendSmsWrapper.execute()`

```java
return sendSmsWrapper.execute(SmsQuery.of(smsBody, phoneNumber));
```

##### 🟦 Receiver

- Actual service sending SMS/Email

##### 🟦 Client

- Our service method

### ➡️ Flow (Very Important for Interview)

- Client(A Service which send the SMS/Email) creates command object
  - `SmsQuery.of(...)`
- Passes it to invoker
  - `sendSmsWrapper.execute(query)`
- Invoker calls receiver internally
  - SMS/Email service sends message

### ➡️ Why Use Command Pattern?

- Decouples who sends request from who executes
- Makes system extensible
- Supports queue, logging, retry

### ➡️ Real-world Use Cases

- Messaging systems (SMS / Email / Portal Message) In Our CASE
- Job queues (Kafka, RabbitMQ)
- Undo/Redo operations
- API request objects

# ⏺️ COMBINED UNDERSTANDING Of Command & Strategy

- Command Pattern

```java
SmsQuery.of(...)
EmailQuery.of(...)
PortalQuery.of(...)
```

- Strategy Pattern

```java
sendSmsWrapper.execute(...)
sendEmailWrapper.execute(...)
sendPortalWrapper.execute(...)
```

- Query (Command) = "WHAT to do"
- Wrapper (Strategy) = "HOW to do"

### ➡️Interview Style

- In our project, we used a combination of Command and Strategy patterns.
- We created **Query** objects like `SmsQuery`, `EmailQuery` & `PortalQuery` which encapsulate the request — this follows **Command Pattern**.
- Then we used a common **interface** `ServiceWrapper` with different implementations like `sendSmsWrapper` and `sendEmailWrapper` & `sendPortalWrapper` — this follows **Strategy Pattern**.
- At runtime, we pass different query objects to different wrappers, which execute the logic accordingly.
- This design helped us keep the system extensible and avoid conditional logic.
