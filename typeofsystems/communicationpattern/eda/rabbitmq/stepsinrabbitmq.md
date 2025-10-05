🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Use RabbitMQ using Spring Cloud Function and Spring Cloud Stream(as per the Udemy Eazy bytes Reddy)

## ➡️ Producer - Account Microservice

### 🟦 **pom.xml**

### 🟦 **pom.xml**

```java
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
    </dependency>
</dependencies>

```

### 🟦 **application.yml**

```java

```

## ➡️ RabbitMQ(Message Queue)

- Make sure **RabbitMQ** running locally.

```java
 docker run -d --hostname rabbitmq --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

## ➡️ Consumer - Message Microservice

### 🟦 **pom.xml**

```java
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
    </dependency>
</dependencies>

```

### 🟦 **application.yml**

- **Binding Naming Convention**
  - Input Binding → `<functionName>-in-<index>`
  - Output Binding → `<functionName>-out-<index>`

```java
spring:
  application:
    name: message

  cloud:
    function:
      definition: email|sms
    stream:
      bindings:
        emailsms-in-0:
          destination: send-communication
          group: ${spring.application.name}
        emailsms-out-0:
          destination: communication-sent

  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
    connection-timeout: 10s

```
