🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕
⏺️ PRODUCER SIDE STORY

### ➡️ Producer Configuration

- Here we set up how the producer connects to Kafka — **e.g.**, `bootstrap-servers`, `serializers`, `retries`, etc.
- `application.yml`

```java
spring:
  cloud:
    stream:
      defaultBinder: kafka
      bindings:
        outputEvent-out-0:
          destination: my-topic
          content-type: application/json
      kafka:
        binder:
          brokers: localhost:9092
        bindings:
          outputEvent-out-0:
            producer:
              configuration:
                retries: 3
                acks: all

```

- **What’s happening here**
  - destination → Topic name
  - acks: all → Wait for leader + replicas acknowledgment
  - retries → Retries in case of failure
- This is equivalent to setting producer configs like `bootstrap.servers`, `key.serializer`, `value.serializer` manually.

### ➡️ Topic Selection

- You decide which topic to send to using the destination in binding OR programmatically route.

```java
spring:
  cloud:
    stream:
      bindings:
        outputEvent-out-0:
          destination: order-topic
```

- You can also choose topics dynamically using Spring Cloud Stream routing:

```java
@Bean
public Function<Message<String>, Message<String>> router() {
    return msg -> MessageBuilder.withPayload(msg.getPayload())
            .setHeader("spring.cloud.stream.sendto.destination", "dynamic-topic")
            .build();
}

```

### ➡️ Message Production

- Here you send the actual event/message to Kafka.

```java
@Bean
public Supplier<OrderCreatedEvent> outputEvent() {
    return () -> {
        OrderCreatedEvent event = new OrderCreatedEvent(UUID.randomUUID().toString(), "Laptop");
        return event;
    };
}

```

- This will automatically produce messages to my-topic because of the binding outputEvent-out-0.

### ➡️ Partition Assignment

- Kafka decides which partition a message goes to:
  - By default: Round-robin
  - Or based on key (deterministic)
  - Or custom partitioner

```java

spring:
  cloud:
    stream:
      bindings:
        outputEvent-out-0:
          producer:
            partitionKeyExpression: headers['partitionKey']
            partitionCount: 3

```

```java
Message<OrderCreatedEvent> message = MessageBuilder
        .withPayload(event)
        .setHeader("partitionKey", event.getOrderId())
        .build();

streamBridge.send("outputEvent-out-0", message);

```

- This ensures the same key always goes to the same partition.

### ➡️ Message Routing and Partition Assignment

- Once partition is decided, the message is routed to the correct broker that hosts the leader of that partition.
- Spring handles this automatically under the hood. No manual coding needed.
- The producer checks the partition leader, routes message there.

### ➡️

- Kafka cluster will replicate the message from partition leader to follower replicas.
- You control replication factor at topic creation, not from Spring code:

```java

```

### ➡️ Acknowledgement and Error Handling

- Once message reaches broker:

  - acks=all waits for all replicas → success callback
  - acks=1 waits for leader only
  - acks=0 → fire and forget

- You can handle errors via ProducerListener or error channels:

```java
spring:
  cloud:
    stream:
      kafka:
        bindings:
          outputEvent-out-0:
            producer:
              configuration:
                acks: all

```

```java
@Bean
public ApplicationListener<ProducerListener.Failure> producerFailureListener() {
    return failure -> System.err.println("❌ Failed to send message: " + failure.getRecord());
}

```

### ➡️

```java

```
