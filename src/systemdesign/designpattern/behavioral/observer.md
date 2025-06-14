
# 🛰️ 1. Observer Pattern

### **Purpose:**
Used when one object (subject) needs to notify multiple other objects (observers) about state changes **without tightly coupling them**.

### **Real-world Analogy:**
A YouTube channel (subject) notifies all its subscribers (observers) when a new video is uploaded.

### **Structure:**
- **Subject**: Maintains a list of observers and notifies them on changes.
- **Observer**: Interface for objects that should be notified.
- **ConcreteSubject**: Implements the logic and notifies observers.
- **ConcreteObserver**: Implements response to notifications.

### **Example (Java-like):**
```java
interface Observer {
    void update(String message);
}

class Subscriber implements Observer {
    public void update(String message) {
        System.out.println("Received update: " + message);
    }
}

class Channel {
    List<Observer> observers = new ArrayList<>();

    void subscribe(Observer o) {
        observers.add(o);
    }

    void notifyObservers(String message) {
        for (Observer o : observers) {
            o.update(message);
        }
    }
}
