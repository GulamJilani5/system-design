# 🧠 2. Strategy Pattern

### Purpose:
- Defines a family of algorithms, encapsulates each one, and makes them interchangeable. It allows the algorithm to vary independently from clients that use it.

### Real-world Analogy:
- A navigation app lets you choose between driving, cycling, or walking routes (strategies).

### Structure:
**Context**: Uses a strategy.  
**Strategy**: Interface for different algorithms.  
**ConcreteStrategy**: Implements the algorithm.

### **Example**:
    interface PaymentStrategy {
         void pay(int amount);
    }
    
    class CreditCardPayment implements PaymentStrategy {
         public void pay(int amount) {
         System.out.println("Paid " + amount + " using Credit Card");
        }
    }
    
    class PayPalPayment implements PaymentStrategy {
            public void pay(int amount) {
            System.out.println("Paid " + amount + " using PayPal");
         }
    }

    class ShoppingCart {
        PaymentStrategy strategy;

        void setPaymentStrategy(PaymentStrategy strategy) {
            this.strategy = strategy;
    }

    void checkout(int amount) {
           strategy.pay(amount);
        }
    }
