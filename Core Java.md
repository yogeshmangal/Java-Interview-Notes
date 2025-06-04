# Java Core Concepts

## 1. What is JDK? (Java Development Kit)
- JDK contains tools that are needed to develop Java applications. These tools are Compiler, JRE, Debugger, etc.
- **In summary:** `JDK = JRE + Development Tools`

## 2. What is JRE? (Java Runtime Environment)
- JRE is a software package that contains the JVM, Java class libraries, and all the required components to run Java applications.
- **In summary:** `JRE = JVM + Libraries to run Java applications`

## 3. What is JVM? (Java Virtual Machine)
- The JVM is a platform-dependent software engine that interprets and executes Java bytecode. It serves as an intermediary between Java programs and the underlying hardware.
- It enables Java code to run on any device or operating system that has a compatible JVM installed.
- **JVM handles tasks like:** garbage collection, memory management, and platform independence.

### Java uses both Compiler and Interpreter:

```
Java Code -> Compiler -> Bytecode
Bytecode -> Interpreter (JVM) -> Machine Code
```

## 4. Inheritance in Java
Java supports three types of inheritance:

- **Single Inheritance**
- **Multilevel Inheritance**
- **Hierarchical Inheritance**

### a) Single Inheritance
```java
class Vehicle {}
class Car extends Vehicle {}
```

### b) Multilevel Inheritance
```java
class Animal {}
class Dog extends Animal {}
class BabyDog extends Dog {}
```

### c) Hierarchical Inheritance
```java
class Person {}
class Student extends Person {}
class Teacher extends Person {}
```

## 5. What is Object-Oriented Programming (OOP)?
Object-Oriented Programming is a methodology to design a program using classes and objects. It provides the following core concepts:

- **Class**
- **Object**
- **Inheritance**
- **Polymorphism**
- **Abstraction**
- **Encapsulation**

### Real-Life Examples:
- **Class**: Blueprint of a car
- **Object**: A specific car like Maruti Swift
- **Inheritance**: A sports car inherits features from a general car
- **Polymorphism**: A remote can operate different devices (TV, AC) using the same buttons
- **Abstraction**: Driving a car without knowing the internal mechanics
- **Encapsulation**: An ATM hides internal operations but provides buttons for users

## 6. What is the String Pool?
- The String Pool is a special area in the Java heap memory where string literals are stored.
- When a string is created using a literal (e.g., `"hello"`), Java checks if the value already exists in the pool. If it does, the new String reference is simply assigned to the existing string object in the pool, rather than creating a new Object.
- This process is called **String Interning**.

```java
String s1 = "hello";     // "hello" is added to the String Pool
String s2 = "hello";     // Reuses the same "hello" from the pool
String s3 = new String("hello"); // Creates a new object in the heap
```

- Strings created using the `new` keyword are not added to the pool and always result in a new object.

## 7. Why Are Strings Immutable?
- Strings are immutable because Strings in the String Pool are shared among multiple references and any attempt to modify their contents could affect the other parts of the code that reference to the same string.
- Other reasons include thread safety, caching, security, and performance benefits.

## 8. Stack Memory vs Heap Memory

### Stack Memory
- Stack Memory is used for **static memory allocation and deallocation**, where memory is allocated and deallocated in the LIFO manner. 
- In Java, the stack memory primarily holds primitive data types, method calls and references to the objects in heap memory.
- Each thread in Java program has its own stack, which stores local variables, method parameters and method calls.
- The Stack memory is limited in size and is generally smaller than Heap Memory.
- When a method is called, a new Stack frame is created on the stack to store its local variables and parameters. When the method returns, its stack frame is removed from the Stack.

### Heap Memory
- Heap Memory is used for **Dynamic memory allocation and deallocation**, where memory is allocated and deallocated as needed during program execution.
- In Java, Objects, Arrays, and Instance variables are stored in the Heap Memory.
- The Heap Memory is shared among all threads in a Java program and is managed by the **Garbage collector.**
- Objects created in the Heap Memory persist until they are no longer referenced and subsequently garbage collected.
- Unlike Stack Memory, Heap Memory does not have a fixed size and can grow or shrink dynamically based on the memory requirements of the Program.

## 9. What is Class Loader in Java?
- In Java, Class Loader is a crucial component of JRE that is responsible for loading Java classes and interfaces into memory at runtime.
- **For Example:** To get input from the console, we require the Scanner class and the Scanner class is loaded by the Class Loader.

## 10. Abstract Class
- In Java, an Abstract class is a class that cannot be instantiated on its own and may contain the mix of abstract and concrete methods, serving as a blueprint for subclasses.

## 11. Interface
- An Interface in Java is a contract that defines a set of abstract methods that must be implemented by classes that implements the interface.

## 12. Difference between Abstract class and Interface:

| Feature                         | Abstract Class                                 | Interface                                         |
|---------------------------------|-------------------------------------------------|--------------------------------------------------|
| **Keyword Used**               | `abstract class`                                | `interface`                                       |
| **Methods**                    | Can have abstract and non-abstract methods      | All methods are abstract by default (until Java 8) <br> Can have `default`, `static`, and `private` methods since Java 8/9 |
| **Access Modifiers**           | Can use any access modifier (public, protected, etc.) | All methods are implicitly `public abstract` (unless `default`, `static`, or `private`) |
| **Constructor**                | Yes                                             | No                                                |
| **Instance Variables**         | Can have instance variables                     | Only `public static final` (constants) allowed    |
| **Multiple Inheritance**       | Not supported (only one abstract class can be extended) | Supported (a class can implement multiple interfaces) |
| **Use of `abstract` Keyword**  | Must be explicitly added to class or method     | Not required for methods (they're abstract by default) |
| **Performance**                | Slightly better (due to direct inheritance)     | Slightly slower (extra indirection)               |

---

## 13. Difference between HashMap and HashTable:

| Feature                  | HashMap                                  | Hashtable                               |
|--------------------------|-------------------------------------------|------------------------------------------|
| **Thread Safety**        | Not synchronized — not thread-safe       | Synchronized — thread-safe               |
| **Performance**          | Faster in single-threaded environments    | Slower due to synchronization            |
| **Null Keys/Values**     | Allows **one null key** and **multiple null values** | Does **not allow** null keys or values   |
| **Use in Multithreading**| Needs external synchronization (e.g., `Collections.synchronizedMap()`) | Thread-safe by default       |

---

## 14. Can we use `this()` and `super()` together in the Same Constructor?
- No, it's **not possible** to use both `this()` and `super()` constructor calls in the same constructor block.

In Java:
- `this()` is used to call **another constructor in the same class**.
- `super()` is used to call a **constructor of the parent class**.

⚠️ **Important Rule:**  
Only **one constructor call (`this()` or `super()`)** is allowed, and it **must be the first statement** in the constructor body.  
Trying to use both results in a **compile-time error**.

---

## 🧪 Example:

```java
class Base {
    Base() {
        System.out.println("Hello from Base");
    }
}

class Main extends Base {
    Main() {
        System.out.println("Main class constructor");
    }

    Main(int x) {
        this();      // ✅ Calls another constructor in the same class
        super();     // ❌ Compile-time error: constructor call must be the first statement
    }
}

public class Program {
    public static void main(String[] args) {
        Main obj = new Main(5);
    }
}
```
---

## 15. Java works as `pass by value` or `pass by reference`?
- Java **always works as `pass by value`**. There is **no concept of `pass by reference`** in Java.
- However, **in the case of objects**, when passed to a method, **a copy of the reference (not the actual object)** is passed.
- This reference still points to the **same object in memory**, so if the object's internal state is changed inside the method, the changes are **visible outside** the method.

### 🔍 In Layman's Terms:
> When you pass an object to a method in Java, you're passing a **copy of the reference** to that object—not the object itself.  
> Both the original and copied references point to the **same memory location**, which is why changes made to the object inside the method are reflected outside.  
> But you **cannot reassign the original reference** from inside the method, as only a copy was passed.

---

### ✅ Code Example 1: Modifying object state

```java
class Person {
    String name;
}

public class Test {
    public static void changeName(Person p) {
        p.name = "John";
    }

    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice";

        changeName(person);

        System.out.println(person.name); // Output: John
    }
}
```
**Explanation:**  
Even though `person` was passed by value (copy of reference), the internal state (`name`) was modified because both references point to the same object.

---

### ❌ Code Example 2: Trying to reassign object

```java
class Person {
    String name;
}

public class Test {
    public static void reassign(Person p) {
        p = new Person();
        p.name = "Bob";
    }

    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice";

        reassign(person);

        System.out.println(person.name); // Output: Alice
    }
}
```
**Explanation:**  
Inside `reassign()`, the reference `p` is reassigned to a new object, but this change does **not affect** the original `person` in `main()`, because only a **copy** of the reference was passed.

---

### ✅ Final Conclusion:
> Java is strictly **pass-by-value**.  
> For objects, the **value passed is a reference**, so Java **appears** to have pass-by-reference behavior—but it's still just passing a **copy of the reference**.

---

## 16. Relationships in Java

- There are various types of relationships that exist between classes. Here are 3 of them:

### a) Inheritance (IS-A)
- Represents a relationship where one class (subclass or child class) derives properties and behavior from another class (superclass or parent class).

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound(); // Inherited from Animal
        dog.bark();
    }
}
```

---

### b) Composition (HAS-A)
- Indicates that a class contains objects of another class as parts and the parts **cannot exist independently** of the whole.
- Represents a **strong ownership** relationship.

**Example:** A School and a Classroom. A classroom is a part of the school and cannot exist independently of the school.

```java
class Classroom {
    void conductClass() {
        System.out.println("Class is in session");
    }
}

class School {
    private Classroom classroom = new Classroom(); // Strongly owned

    void startDay() {
        classroom.conductClass();
    }
}
```

---

### c) Aggregation (HAS-A)
- Similar to composition, but the parts **can exist independently** of the whole.
- Represents a **weaker form of ownership** compared to composition.

**Example:** A School and a Teacher. Teachers are associated with the school but can exist independently.

```java
class Teacher {
    void teach() {
        System.out.println("Teacher is teaching");
    }
}

class School {
    private Teacher teacher; // Can be shared, not tightly owned

    School(Teacher teacher) {
        this.teacher = teacher;
    }

    void conductClass() {
        teacher.teach();
    }
}

public class Main {
    public static void main(String[] args) {
        Teacher t = new Teacher();
        School s1 = new School(t);
        School s2 = new School(t); // Same teacher used in different schools
        s1.conductClass();
        s2.conductClass();
    }
}
```
---

## 17. How to Make Database Queries Fast?
- There are several key techniques to improve the performance of database queries:

### a) Indexing
- Properly index the columns that are frequently used in `WHERE` clauses, `JOIN` conditions, and `ORDER BY` clauses.
- Internally, indexing works by creating data structures (like B-Trees or Hash Maps) that allow for efficient retrieval of records.

```sql
-- Example: Creating an index on 'email' column
CREATE INDEX idx_users_email ON users(email);
```

---

### b) Query Optimization
- Avoid `SELECT *`; instead, specify only the columns you need.
- Use `WHERE` clauses to filter data as early as possible.
- Use `LIMIT` to restrict the number of rows returned.

---

### c) Database Design
- Normalize tables to reduce data redundancy.
- Use appropriate data types for columns (e.g., use `INT` instead of `VARCHAR` for numeric data).

---

### d) Efficient Joins
- Prefer `INNER JOIN` over `OUTER JOIN` when possible.
- Ensure that columns used in `JOIN` conditions are indexed.

---

### e) Caching
- Cache frequently accessed data (e.g., using Redis or in-memory cache).
- Reduces repeated hits to the database for the same query.

---

### f) Connection Pooling
- Reuse database connections using a connection pool (e.g., HikariCP in Java).
- Reduces the overhead of establishing new connections repeatedly.

```java
// HikariCP example
HikariDataSource ds = new HikariDataSource();
ds.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
ds.setUsername("root");
ds.setPassword("password");
```
---

## 18. What is Connection Pooling?
- Connection pooling is a technique used to **reuse database connections** instead of creating and destroying them repeatedly. It maintains a **pool of open connections**, which can be reused by applications to communicate with the database.

### 🔹 Why is it needed?
Establishing a database connection is a **costly and time-consuming operation**. Without pooling:
- Every request opens a new connection → high overhead.
- More chances of resource exhaustion.
- Increased latency in high-traffic applications.

With pooling:
- Connections are reused → faster query execution.
- Reduces database load and improves application performance.
- Efficient use of resources.

---

### 🔹 How it works:
1. A connection pool is initialized with a **predefined number of open connections**.
2. When the application needs to interact with the DB:
   - It **borrows a connection** from the pool.
3. Once done:
   - The connection is **returned to the pool** instead of being closed.
4. If no connections are available:
   - The application waits or creates new connections based on configuration.

---

### 🔹 Example: HikariCP in Java (Spring Boot)

```java
// application.properties

spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
```
> HikariCP is the default and high-performance connection pool used in Spring Boot.
> It maintains active and idle connections, improving responsiveness under load.