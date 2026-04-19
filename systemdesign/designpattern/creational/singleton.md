🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Singleton - Only one object allowed

- Only one instance of a class exists
- Provides a global access point to that instance

## ➡️ Different Ways to Create Singleton

- Singleton in Core Java (How it works internally)

#### 🟦 1. Eager Initialization

- Instance is created at class loading.
- Not suitable if instance creation is heavy and might not be used, not used much.

```java
      public class Singleton {
      private static final Singleton instance = new Singleton();

      private Singleton() {}

      public static Singleton getInstance() {
      return instance;
      }
      }
```

#### 🟦 2. Lazy Initialization(Not Thread-safe)

- Instance is created only when needed, mostly used.
- Not thread-safe by default.

```java
      public class Singleton {
      private static Singleton instance;

      private Singleton() {}

      public static Singleton getInstance() {
      if (instance == null) {
      instance = new Singleton();
      }
      return instance;
      }
      }
```

#### 🟦 3. Synchronized (Thread-safe Singleton )

- Thread-safe.
- Performance overhead due to synchronization.

```java
        public class Singleton {
          private static Singleton instance;

        private Singleton() {}

        public static synchronized Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
       }
```

#### 🟦 4. Double-Checked Locking (Mostly used in practice)

- Thread-safe.
- High performance — synchronization happens only on first initialization.
- Requires volatile to avoid memory consistency errors.

```java
      public class Singleton {

      private static volatile Singleton instance;

        private Singleton() {}

        public static Singleton getInstance() {
            if (instance == null) { // First check (no locking)
                synchronized (Singleton.class) {
                    if (instance == null) { // Second check (with locking)
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }

  }
```

## ➡️ Singleton in Spring Boot

- We DON'T write Singleton code in Spring
- Spring handles it internally using IoC Container
- By default, Spring Beans are Singleton
- Spring creates only one object per container
- What happens internally:
  - Spring starts
  - It scans classes with: `@Component`, `@Service`, `@Repository` & `@Controller`
  - It creates ONE instance per bean
  - Stores it in **ApplicationContext** (IoC Container)
- Singleton in Spring = per container
- Not JVM-wide singleton
- 2 Spring containers → 2 instances

```java
@Service
public class PaymentService {
}
```

```java
@Controller
public class PaymentController {
    @Autowired
    private PaymentService paymentService;
}
```

- Even if:

```java
@Autowired PaymentService p1;
@Autowired PaymentService p2;
```

- Both point to same object

#### 🟦 Internal Concept

- Spring uses:
  - Singleton Bean Scope (default)
  - Stored in BeanFactory cache

```java
Map<String, Object> singletonObjects
```

#### 🟦 Why Spring Uses Singleton by Default

- Performance
  - Object creation is expensive
  - Reuse same instance
- Memory Efficient
  - No duplicate objects
- Centralized Management
  - One place to manage lifecycle

## ➡️ Use Case - Real Project

#### 🟦 DB connection

- Without Singleton, Each request creates DB connection → expensive
- Database Connection Pool
- Only one connection manager should exist → avoids duplication & resource waste
- One connection manager (e.g., HikariCP)

```java
@Bean
public DataSource dataSource() {
    return new HikariDataSource();
}
```

#### 🟦 Payment System

```java
@Service
public class PaymentService {
    public void process() {
        System.out.println("Processing payment...");
    }
}
```

- Why Singleton?
  - Stateless service
  - No need to create multiple objects
  - Better performance
- Use when:
  - Object is stateless
  - Object is shared
  - Expensive to create
- Avoid when:
  - Object has state (user-specific data)
