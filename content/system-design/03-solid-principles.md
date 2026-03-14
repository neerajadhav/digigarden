---
title: SOLID Principles
tags:
  - Principles
---

**SOLID** is a set of **5 design principles** that help you write:

- clean code
    
- flexible code
    
- maintainable code
    
- scalable code
    

They are **guidelines**, not rules.  
Following them makes your OOP designs **robust and easy to change**.

```
S – Single Responsibility Principle
O – Open/Closed Principle
L – Liskov Substitution Principle
I – Interface Segregation Principle
D – Dependency Inversion Principle
```


## 1. Single Responsibility Principle (SRP)

### Definition

> **A class should have only one reason to change.**

In simple words:

- One class = **one job**
    
- A class should do **only one thing**
    


### ❌ Problem (Violating SRP)

```java
class Student {
    String name;
    int marks;

    void calculateGrade() {
        // grading logic
    }

    void saveToDatabase() {
        // database code
    }

    void printReport() {
        // printing logic
    }
}
```

### Why this is bad?

This class has **multiple responsibilities**:

- business logic (grade)
    
- database logic
    
- presentation logic
    

If **database changes**, this class changes  
If **report format changes**, this class changes

❌ Too many reasons to change.


### ✅ Solution (Following SRP)

```java
class Student {
    String name;
    int marks;
}

class GradeCalculator {
    void calculateGrade(Student student) {
        // grading logic
    }
}

class StudentRepository {
    void save(Student student) {
        // database logic
    }
}

class StudentReport {
    void print(Student student) {
        // printing logic
    }
}
```

### Benefits

- Easier to maintain
    
- Easier to test
    
- Changes don’t break unrelated code
    


## 2. Open/Closed Principle (OCP)

### Definition

> **Software entities should be open for extension but closed for modification.**

Meaning:

- You should **add new behavior**
    
- Without **changing existing code**
    


### ❌ Problem (Violating OCP)

```java
class DiscountCalculator {
    double calculateDiscount(String customerType, double amount) {
        if (customerType.equals("Regular")) {
            return amount * 0.1;
        } else if (customerType.equals("Premium")) {
            return amount * 0.2;
        }
        return 0;
    }
}
```

### Why this is bad?

- Adding a new customer type → **modify the class**
    
- Risk of breaking existing logic
    


### ✅ Solution (Using Polymorphism)

```java
interface Discount {
    double calculate(double amount);
}

class RegularCustomerDiscount implements Discount {
    public double calculate(double amount) {
        return amount * 0.1;
    }
}

class PremiumCustomerDiscount implements Discount {
    public double calculate(double amount) {
        return amount * 0.2;
    }
}
```

```java
class DiscountCalculator {
    double calculateDiscount(Discount discount, double amount) {
        return discount.calculate(amount);
    }
}
```

### Benefits

- New discount types → **new classes only**
    
- Existing code remains untouched
    


## 3. Liskov Substitution Principle (LSP)

### Definition

> **Objects of a superclass should be replaceable with objects of its subclass without breaking the program.**

Simple meaning:

- Child class should behave like parent
    
- No unexpected behavior
    


### ❌ Classic Violation Example

```java
class Bird {
    void fly() {
        System.out.println("Bird flying");
    }
}

class Ostrich extends Bird {
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```

### Why this violates LSP?

- Ostrich **is a Bird**
    
- But Ostrich **cannot fly**
    
- Replacing Bird with Ostrich breaks code
    


### ✅ Solution (Proper Abstraction)

```java
interface Bird {
}

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() {
        System.out.println("Sparrow flying");
    }
}

class Ostrich implements Bird {
    // no fly method
}
```

### Benefits

- Correct inheritance
    
- No runtime surprises
    
- Safer polymorphism
    


## 4. Interface Segregation Principle (ISP)

### Definition

> **Clients should not be forced to depend on interfaces they do not use.**

Meaning:

- Prefer **many small interfaces**
    
- Avoid **fat interfaces**
    


### ❌ Problem (Violating ISP)

```java
interface Machine {
    void print();
    void scan();
    void fax();
}

class OldPrinter implements Machine {
    public void print() {
        System.out.println("Printing");
    }

    public void scan() {
        // not supported
    }

    public void fax() {
        // not supported
    }
}
```

### Why this is bad?

- OldPrinter is forced to implement methods it doesn’t need
    
- Leads to empty or exception code
    


### ✅ Solution (Split Interfaces)

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface Fax {
    void fax();
}
```

```java
class OldPrinter implements Printer {
    public void print() {
        System.out.println("Printing");
    }
}

class MultiFunctionPrinter implements Printer, Scanner, Fax {
    public void print() {}
    public void scan() {}
    public void fax() {}
}
```

### Benefits

- Cleaner code
    
- No unused methods
    
- Better flexibility
    


## 5. Dependency Inversion Principle (DIP)

### Definition

> **High-level modules should not depend on low-level modules. Both should depend on abstractions.**

In short:

- Depend on **interfaces**, not **concrete classes**
    


### ❌ Problem (Tight Coupling)

```java
class MySQLDatabase {
    void connect() {
        System.out.println("Connected to MySQL");
    }
}

class UserService {
    MySQLDatabase db = new MySQLDatabase();

    void saveUser() {
        db.connect();
    }
}
```

### Why this is bad?

- UserService is tightly coupled to MySQL
    
- Switching to PostgreSQL requires code changes
    


### ✅ Solution (Using Abstraction)

```java
interface Database {
    void connect();
}

class MySQLDatabase implements Database {
    public void connect() {
        System.out.println("Connected to MySQL");
    }
}

class PostgreSQLDatabase implements Database {
    public void connect() {
        System.out.println("Connected to PostgreSQL");
    }
}
```

```java
class UserService {
    private Database database;

    UserService(Database database) {
        this.database = database;
    }

    void saveUser() {
        database.connect();
    }
}
```

### Benefits

- Loose coupling
    
- Easy to swap implementations
    
- Better testability (mocking)
    


## One-Line Interview Summary

- **SRP**: One class, one responsibility
    
- **OCP**: Extend behavior without modifying code
    
- **LSP**: Child should safely replace parent
    
- **ISP**: Many small interfaces are better
    
- **DIP**: Depend on abstractions, not implementations
    
