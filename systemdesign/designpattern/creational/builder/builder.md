⏺️ ➡️ 🟦 🔵 🟢 🔴 ⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Builder Class

##### 🟦 Why keep Builder inside User?

- Because **Builder** is tightly related to **User** only.
- It does not make sense independently.
- **Builder** is a helper for creating that(here its **User**) object.

##### 🟦 Why Builder class is static

- If Builder were NOT static:

```java
class User {

    class Builder {

    }
}
```

- then Java would require:

```java
User user = new User();
User.Builder builder = user.new Builder();
```

- This makes no sense because:
  - we are trying to create **User**
  - but **User** object already needed first
- Circular problem 🔴
- **Therefore:** We should make builder class as static and it allows

```java
new User.Builder()
```

- Without existing **User** object.

##### 🟦 Can Builder Be Separate Class?

- YES, But usually not preferred because:
  - Builder only belongs to User
  - nesting keeps code cleaner
  - easier maintenance

### ➡️ Understanding Builder with an example

##### 🟦 User Class

```java
class User {

    String name;
    int age;
    String city;

    User(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
}
```

- ###### 🔵 Object creation:

```java
User user = new User("Gulam", 25, "Pune");
```

- Works fine
- But if:

```java
User("Gulam", 25, "Pune", true, "Java", 50000, ...)
```

- difficult to read
- parameter order confusion
- many optional fields issue

##### 🟦 In order to resolve this issue we should use **Builder** class

- **Builder** belongs to **User**
- **Builder** stores temporary values

```java
class User {

    // =========================
    // Actual User fields
    // =========================
    private String name;
    private int age;
    private String city;

    // =========================
    // Constructor
    // =========================
    private User(Builder builder) {

        // Copy values from Builder -> User object
        this.name = builder.name;
        this.age = builder.age;
        this.city = builder.city;
    }

    // =========================
    // toString
    // =========================
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", city='" + city + '\'' +
                '}';
    }

    // =========================
    // Builder Class - internal static class Belongs to User
    // =========================
    public static class Builder {

        // Temporary fields
        private String name;
        private int age;
        private String city;

        // =========================
        // Builder Methods
        // =========================

        public Builder name(String name) {

            // store value inside builder
            this.name = name;

            // return same builder object
            return this;
        }

        public Builder age(int age) {

            this.age = age;

            return this;
        }

        public Builder city(String city) {

            this.city = city;

            return this;
        }

        // =========================
        // build() Method
        // =========================
        public User build() {

            // create final User object
            return new User(this);
        }
    }
}
```

##### 🟦 A Class which wanted to use the Builder class

###### 🔵 Case 1 Without builder() helper method

- This is pure/manual builder pattern implementation.
- Directly creates Builder object using

```java
new User.Builder()
```

```java
public class UserService {

    public static void createUser() {

        User user = new User.Builder()
                .name("Gulam")
                .age(25)
                .city("Pune")
                .build();

        System.out.println(user);
    }
}
```

- ###### 🔵 OUTPUT

```java
 User{name='Gulam', age=25, city='Pune'}
```

###### 🔵 Case 2 With builder() helper method (Modern Style)

- Add this method inside User class 🔴

```java
class User {

    ...
    ...

    // Helper method
    public static Builder builder() {
        return new Builder();
    }

    public static class Builder {
     ....
     ....
    }
}
```

- Using `User.builder()` method rather than `User.Builder()`

```java
User user = User.builder()
        .name("Gulam")
        .age(25)
        .city("Pune")
        .build();
```

- `User.builder()` internally calls: `return new Builder();`

- ###### 🔵 OUTPUT

```java
 User{name='Gulam', age=25, city='Pune'}
```

##### 🟦 NOW LET’S UNDERSTAND FLOW STEP-BY-STEP

###### 🔵 STEP 1

- Creates Builder object.

```java
new User.Builder()
```

- OR

```java
User.builder()
```

- Memory:

```java
Builder {
    name = null
    age = 0
    city = null
}
```

###### 🔵 STEP 2

- `.name("Gulam")` calls below:

```java
public Builder name(String name) {
    this.name = name;
    return this;
}
```

- Now builder memory becomes:

```java
Builder {
    name = "Gulam"
    age = 0
    city = null
}
```

###### 🔵 STEP 3

- `.age(30)`
- Now builder memory becomes:

```java
Builder {
    name = "Gulam"
    age = 25
    city = null
}
```

###### 🔵 STEP 4

- `.city("Pune")`
- Builder memory:

```java
Builder {
    name = "Gulam"
    age = 25
    city = "Pune"
}
```

###### 🔵 STEP 5

- `.build()` calls:

```java
return new User(this);
```

- Here VERY IMPORTANT is `this` which means: current Builder object.
- So internally:

```java
new User(builderObject)
```

###### 🔵 STEP 6

- Constructor executes:

```java
private User(Builder builder) {
    this.name = builder.name;
    this.age = builder.age;
    this.city = builder.city;
}
```

- Copies data from builder → final object.

```java
User {
    name = "Gulam"
    age = 25
    city = "Pune"
}
```
