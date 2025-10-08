🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

⏺️ CONSUMER SIDE STORY

### ➡️ Consumer Group and Topic Subscription

- A consumer must join a consumer group and subscribe to topics.

### ➡️

```java
spring:
  cloud:
    stream:
      bindings:
        inputEvent-in-0:
          destination: my-topic
          group: my-consumer-group
          content-type: application/json

```

- **group** → All consumers with same group share the work (partition assignment)
- If group changes → new offsets start fresh (by default latest)

### ➡️ Partition Assignment

- Kafka automatically assigns partitions to consumers in the group.

```java
spring:
  cloud:
    stream:
      bindings:
        inputEvent-in-0:
          consumer:
            concurrency: 3

```

- This will spin up multiple consumer threads to handle multiple partitions in parallel.

### ➡️ Offset Management

- Kafka uses offsets to track what messages have been processed.
- Spring Cloud Stream handles this automatically (by default commits after successful message processing).

```java
spring:
  cloud:
    stream:
      kafka:
        bindings:
          inputEvent-in-0:
            consumer:
              enableDlq: true
              autoCommitOffset: true

```

- If you want manual commits, you can use Acknowledgment in a listener.

### ➡️ Fetch Request

```java

```

### ➡️ Message Fetching

```java

```

### ➡️ Message Processing

```java

```

### ➡️ Committing the Offset

- Steps **Fetch Request** **Message Fetching** **Message Processing** **Committing the Offset** happen continuously inside the consumer polling loop.
- **Spring abstracts this, but conceptually:**
  - **Fetch Request:** Consumer asks broker for data for its assigned partitions.
  - **Message Fetching:** Broker sends a batch of records.
  - **Message Processing:** Your function consumes & processes.
  - **Commit Offset:** After successful processing, Spring commits offset (automatically or manually).

```java
@Bean
public Consumer<OrderCreatedEvent> inputEvent() {
    return event -> {
        System.out.println("✅ Received Order: " + event);
        // process and save to DB...
    };
}

```

- Spring handles the poll loop under the hood — you don't manually call poll() like in the raw Kafka API.

### ➡️ Polling Loop

- Kafka consumer runs an infinite poll loop to keep fetching, processing, and committing messages.
- In **Spring Cloud Stream**, this is invisible. You just declare a Consumer bean;
- Spring creates the listener container and manages the loop.
