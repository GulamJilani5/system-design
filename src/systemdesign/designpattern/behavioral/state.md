# 🔄 3. State Pattern
**Purpose:**
- Allows an object to change its behavior when its internal state changes. It appears as if the object changed its class.

### Real-world Analogy:
A traffic light changes behavior based on its current state (red, yellow, green).

### Structure:
**Context**: Maintains an instance of a state.  
**State**: Interface for state behavior.  
**ConcreteState**: Implements behavior for a particular state.  

### **Example:**

    interface State {
    void handle();
    }
    
    class RedLight implements State {
    public void handle() {
    System.out.println("Red Light - Stop");
    }
    }
    
    class GreenLight implements State {
    public void handle() {
    System.out.println("Green Light - Go");
    }
    }
    
    class TrafficLight {
    private State state;
    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handle();
    }
}
