# 🌳 3. Composite Pattern

### ✅ Definition
- Composite pattern lets clients treat individual objects and compositions of objects uniformly.

### 🔧 Use Cases
- Hierarchical structures like menus, file systems, organizations.  
- Tree structures.  

### 📄 Example

    // Component
    interface Employee {
    void showEmployeeDetails();
    }
    
    // Leaf
    class Developer implements Employee {
    private String name;
    Developer(String name) { this.name = name; }
    
        public void showEmployeeDetails() {
            System.out.println("Developer: " + name);
        }
    }
    
    // Composite
    class Manager implements Employee {
    private List<Employee> employees = new ArrayList<>();
    
        public void add(Employee e) {
            employees.add(e);
        }
    
        public void showEmployeeDetails() {
            for (Employee e : employees) {
                e.showEmployeeDetails();
            }
        }
    }
