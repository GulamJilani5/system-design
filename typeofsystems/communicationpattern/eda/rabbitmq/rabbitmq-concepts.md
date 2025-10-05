🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Two ways to use RabbitMQ.

## ➡️ Spring Boot + Spring AMQP

- Spring AMQP directly (i.e., RabbitTemplate + @RabbitListener) — which is the “manual” way to use RabbitMQ.
- Use Spring AMQP if you want low-level control, simpler apps, or learning fundamentals.

#### 🟦 Steps

##### 🔵 STEP 1: Setup RabbitMQ

- Make sure RabbitMQ running locally.

```java
docker run -d --hostname rabbitmq --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

##### 🔵 STEP 2: Producer — AccountsService

- service will publish messages to a queue when a new account is created.

- **pom.xml**

```java
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
    </dependency>
```

- **application.yml**

```java
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```

- **RabbitMQConfig.java**

```java
package com.example.accounts.config;

import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitMQConfig {
    public static final String ACCOUNT_QUEUE = "account-created-queue";

    @Bean
    public Queue queue() {
        return new Queue(ACCOUNT_QUEUE, false);
    }
}

```

- **AccountController.java**

```java
package com.example.accounts.controller;

import com.example.accounts.config.RabbitMQConfig;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/accounts")
public class AccountController {

    private final RabbitTemplate rabbitTemplate;

    public AccountController(RabbitTemplate rabbitTemplate) {
        this.rabbitTemplate = rabbitTemplate;
    }

    @PostMapping("/create")
    public String createAccount(@RequestParam String email) {
        // Logic to create account in DB would go here...

        // Publish event to RabbitMQ
        rabbitTemplate.convertAndSend(RabbitMQConfig.ACCOUNT_QUEUE, email);
        return "Account created. Notification event sent for email: " + email;
    }
}

```

##### 🔵 STEP 3: Consumer — NotificationService

- **pom.xml**

```java
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-amqp</artifactId>
    </dependency>
```

- **application.yml**

```java
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

```

- **NotificationListener.java**

```java
package com.example.notifications.listener;

import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
public class NotificationListener {

    @RabbitListener(queues = "account-created-queue")
    public void handleAccountCreated(String email) {
        System.out.println("📧 Sending welcome email to: " + email);
        // Logic to send email or SMS goes here...
    }
}

```

## ➡️ Spring Cloud Stream + Spring Cloud Function.

- cloud-native microservices approach(real microservice event-driven systems).
- These libraries abstract away low-level messaging details and make your producer/consumer code more declarative and portable (you can switch brokers like RabbitMQ ⇄ Kafka without changing business logic).

#### 🟦 Steps

##### 🔵 STEP 1: Setup RabbitMQ

- Make sure RabbitMQ running locally.

```java
docker run -d --hostname rabbitmq --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

##### 🔵 STEP 2: Producer — AccountsService

- **pom.xml**

```java
  <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
    </dependency>
```

- **application.yml**

```java
spring:
  cloud:
    stream:
      bindings:
        accountCreatedSupplier-out-0:
          destination: account-created
      rabbit:
        bindings:
          accountCreatedSupplier-out-0:
            producer:
              routing-key-expression: '''account-created'''

```

- **AccountEventSupplier.java**

```java
package com.example.accounts;

import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;
import java.util.function.Supplier;

@Component
public class AccountEventSupplier {

    private final BlockingQueue<String> eventQueue = new LinkedBlockingQueue<>();

    @Bean
    public Supplier<String> accountCreatedSupplier() {
        return eventQueue::poll;
    }

    public void publish(String email) {
        eventQueue.offer(email);
    }
}

```

- **AccountController.java**

```java
package com.example.accounts;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/accounts")
public class AccountController {

    private final AccountEventSupplier supplier;

    public AccountController(AccountEventSupplier supplier) {
        this.supplier = supplier;
    }

    @PostMapping("/create")
    public String createAccount(@RequestParam String email) {
        // Business logic to create account...
        supplier.publish(email); // Push event to stream
        return "Account created for " + email;
    }
}

```

##### 🔵 STEP 3: Consumer — NotificationService

- **pom.xml**

```java
<dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
    </dependency>
```

- **application.yml**

```java
spring:
  cloud:
    stream:
      bindings:
        accountCreatedConsumer-in-0:
          destination: account-created
          group: notification-group

```

- **NotificationConsumer.java**

```java
package com.example.notifications;

import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.function.Consumer;

@Component
public class NotificationConsumer {

    @Bean
    public Consumer<String> accountCreatedConsumer() {
        return email -> {
            System.out.println("📧 Sending welcome email to: " + email);
            // Send email logic...
        };
    }
}

```

## ➡️ Spring AMQP (manual) vs Spring Cloud Stream + Function

| Feature                      | Spring AMQP (manual)                                   | Spring Cloud Stream + Function                               |
| ---------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| **Level of abstraction**     | Low — you manage queues, templates, listeners yourself | High — you focus on functions (Supplier, Function, Consumer) |
| **Code to send messages**    | `RabbitTemplate.convertAndSend()`                      | Bind to `Supplier` / `Function` automatically                |
| **Broker-specific code**     | Yes (RabbitMQ specific)                                | No (can switch to Kafka etc. easily)                         |
| **Best for**                 | Simple apps, fine-grained control                      | Cloud-native, event-driven microservices                     |
| **Configuration**            | Programmatic + YAML                                    | Mostly YAML (bindings)                                       |
| **Scalability & decoupling** | Manual                                                 | Easier (through binding abstractions)                        |
