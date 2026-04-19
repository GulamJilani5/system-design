⏺️ ➡️ 🟦 🔵 🟢🔴⭕🟠🟣🟥🟧✔️ ☑️ 🔹 • ‣ → ⁕

# ⏺️ Observer Pattern

- Used when one object (subject or observable) needs to notify multiple other objects (observers) about state changes **without tightly coupling them**.

### ➡️ Real-world Analogy

- A YouTube channel (subject) notifies all its subscribers (observers) when a new video is uploaded.

- **Structure:**
  - **Subject**: Maintains a list of observers and notifies them on changes.
  - **Observer**: Interface for objects that should be notified.
  - **ConcreteSubject**: Implements the logic and notifies observers.
  - **ConcreteObserver**: Implements response to notifications.

##### 🔵 Observer Interface

```java
interface Observer {
    void update(String message);
}
```

##### 🔵 Subject

```java
import java.util.*;

public class NotificationService {
    private List<Observer> observers = new ArrayList<>();

    public void subscribe(Observer observer) {
        observers.add(observer);
    }

    public void notifyAllUsers(String msg) {
        for (Observer obs : observers) {
            obs.update(msg);
        }
    }
}
```

##### 🔵 Concrete Observers

```java
public class EmailService implements Observer {
    public void update(String message) {
        System.out.println("Email sent: " + message);
    }
}

public class SMSService implements Observer {
    public void update(String message) {
        System.out.println("SMS sent: " + message);
    }
}
```

##### 🔵 Usage

```java
NotificationService service = new NotificationService();

service.subscribe(new EmailService());
service.subscribe(new SMSService());

service.notifyAllUsers("Order placed!");
```
