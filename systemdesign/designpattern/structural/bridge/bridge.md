# 🧱 2. Bridge Pattern

### ✅ Definition
- Bridge pattern decouples abstraction from implementation so that both can vary independently.

### 🔧 Use Cases
- UI toolkits (different OS implementations).  
- Supporting multiple platforms/devices.  
- When both abstraction and implementation may change over time.  

### 📄 Example

    // Implementor
    interface Color {
    void applyColor();
    }

    // Concrete Implementor
    class RedColor implements Color {
    public void applyColor() {
    System.out.println("Applying red color");
    }
    }

    // Abstraction
    abstract class Shape {
    protected Color color;
    Shape(Color c) { this.color = c; }
    abstract void draw();
    }

    // Refined Abstraction
    class Circle extends Shape {
    Circle(Color c) { super(c); }

    void draw() {
        System.out.print("Circle filled with color: ");
        color.applyColor();
    }
    }


