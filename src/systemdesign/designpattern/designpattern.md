🔵🟢🔴➡️⭕🟠🟦🟣🟥🟧✔️
☑️ • ‣ → ⁕

# 🔨 Creational Design Patterns
**Purpose:** Deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

| Pattern              | Description                                                                                                         | Example Use Case                                              |
| -------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Singleton**        | Ensures a class has only one instance and provides a global point of access to it.                                  | Configuration manager, logger                                 |
| **Factory Method**   | Provides an interface for creating objects but allows subclasses to alter the type of objects that will be created. | Document creator for different formats (PDF, DOCX)            |
| **Abstract Factory** | Produces families of related or dependent objects without specifying their concrete classes.                        | UI components for multiple operating systems                  |
| **Builder**          | Constructs a complex object step by step.                                                                           | Creating a complex object like a car with multiple components |
| **Prototype**        | Creates new objects by copying an existing object (prototype).                                                      | Cloning objects where instantiation is costly                 |


# 🧱 Structural Design Patterns
**Purpose:** Concerned with how classes and objects are composed to form larger structures.

| Pattern       | Description                                                                        | Example Use Case                               |
| ------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Adapter**   | Allows incompatible interfaces to work together.                                   | Connecting new software to legacy systems      |
| **Bridge**    | Separates abstraction from its implementation so that both can vary independently. | UI abstraction vs rendering implementation     |
| **Composite** | Composes objects into tree structures to represent part-whole hierarchies.         | File system structure (folders and files)      |
| **Proxy**     | Provides a surrogate or placeholder for another object.                            | Virtual proxy for loading images lazily        |
| **Decorator** | Adds responsibilities to objects dynamically.                                      | Adding scrolling or border to a UI component   |
| **Facade**    | Provides a simplified interface to a larger body of code.                          | API gateway, simplified interface to a library |
| **Flyweight** | Reduces the cost of creating and manipulating a large number of similar objects.   | Text editors sharing character glyphs          |


# 🔁 Behavioral Design Patterns
**Purpose:** Deal with communication between objects and how responsibilities are assigned.

| Pattern                     | Description                                                                                              | Example Use Case                           |
| --------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| **Observer**                | Defines a one-to-many dependency so that when one object changes state, all its dependents are notified. | Event handling, UI listeners               |
| **Strategy**                | Defines a family of algorithms, encapsulates each one, and makes them interchangeable.                   | Sorting strategies (bubble, quick, merge)  |
| **Command**                 | Encapsulates a request as an object.                                                                     | Undo/redo functionality in editors         |
| **Chain of Responsibility** | Passes a request along a chain of handlers.                                                              | Request validation or processing pipelines |
| **Mediator**                | Defines an object that encapsulates how a set of objects interact.                                       | Chatroom manager                           |
| **Memento**                 | Captures and restores an object’s internal state.                                                        | Save and restore game state                |
| **State**                   | Allows an object to alter its behavior when its internal state changes.                                  | Traffic light controller                   |
| **Template Method**         | Defines the program skeleton of an algorithm, deferring steps to subclasses.                             | Algorithm with customizable steps          |
| **Visitor**                 | Lets you define a new operation without changing the classes of the elements.                            | Compilers, object structure traversal      |
| **Interpreter**             | Implements a language interpreter for a grammar.                                                         | Parsing expressions or scripting languages |
| **Iterator**                | Provides a way to access elements of a collection sequentially.                                          | Iterating over a list or array             |
