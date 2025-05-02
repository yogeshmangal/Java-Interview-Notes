# SOLID Principles in Java

The **SOLID** principles are five design principles intended to make software designs more understandable, flexible, and maintainable.

---

## 1. **S — Single Responsibility Principle (SRP)**

**Definition:** A class should have only one reason to change, meaning it should only have one job or responsibility.

```java
class Invoice {
    public void calculateTotal() {
        // Logic to calculate total
    }
}

class InvoicePrinter {
    public void printInvoice(Invoice invoice) {
        // Logic to print invoice
    }
}
```

---

## 2. **O — Open/Closed Principle (OCP)**

**Definition:** Software entities should be open for extension but closed for modification.

```java
interface Discount {
    double apply(double price);
}

class NewYearDiscount implements Discount {
    public double apply(double price) {
        return price * 0.9;
    }
}

class InvoiceService {
    public double getFinalPrice(double price, Discount discount) {
        return discount.apply(price);
    }
}
```

---

## 3. **L — Liskov Substitution Principle (LSP)**

**Definition:** Subtypes must be substitutable for their base types without altering the correctness of the program.

```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Sparrow extends Bird {}

class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly");
    }
}
```

*Violation of LSP because:*

```java
Bird bird = new Ostrich(); // ❌ This will break at runtime if fly() is called
bird.fly(); // Throws UnsupportedOperationException
```
---

## 4. **I — Interface Segregation Principle (ISP)**

**Definition:** Clients should not be forced to depend on interfaces they do not use.

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

class MultiFunctionPrinter implements Printer, Scanner {
    public void print() {}
    public void scan() {}
}

class SimplePrinter implements Printer {
    public void print() {}
}
```

---

## 5. **D — Dependency Inversion Principle (DIP)**

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.

```java
interface MessageSender {
    void send(String message);
}

class EmailSender implements MessageSender {
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

class NotificationService {
    private final MessageSender sender;

    public NotificationService(MessageSender sender) {
        this.sender = sender;
    }

    public void notify(String msg) {
        sender.send(msg);
    }
}
```
