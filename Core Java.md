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
- An 