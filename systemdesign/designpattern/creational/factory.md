🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️⏺️ ☑️ • ‣ → ⁕

# ⏺️ Factory Method

Sometimes we don't want users to call `new` directly, we give them a method that decides how objects are created.

````java
class Shape{
    private Shape(){}

    public static Shape createCircle(){
        return new Shape();
    }
}
```

```java
public class Demo{
    public static void main(String[ args]){
        Shape circle = Shape.createCircle();
    }
}

````
