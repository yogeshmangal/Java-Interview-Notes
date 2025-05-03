# Java Core Concepts

## 1. What is JDK? (Java Development Kit)
- The JDK contains tools needed to develop Java applications, including the JRE, compiler, debugger, and other development tools.
- **In summary:** `JDK = JRE + Development Tools`

## 2. What is JRE? (Java Runtime Environment)
- The JRE is a software package that includes the JVM, Java class libraries, and all necessary components to run Java applications.
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
- When a string is created using a literal (e.g., `"hello"`), Java checks if the value already exists in the pool. If it does, the new reference points to the existing string, avoiding duplicate objects.
- This process is called **String Interning**.

```java
String s1 = "hello";     // "hello" is added to the String Pool
String s2 = "hello";     // Reuses the same "hello" from the pool
String s3 = new String("hello"); // Creates a new object in the heap
```

- Strings created using the `new` keyword are not added to the pool and always result in a new object.

## 7. Why Are Strings Immutable?
- Strings are immutable because they are shared in the string pool. Changing a shared string would affect all references to it.
- Other reasons include thread safety, caching, security, and performance benefits.

## 8. Stack Memory vs Heap Memory

### Stack Memory
- Used for **static memory allocation** (e.g., method calls, primitive types).
- Follows **LIFO (Last In, First Out)** structure.
- Each thread has its own stack.
- Contains local variables, method parameters, and references to heap objects.
- Limited in size and faster access.

### Heap Memory
- Used for **dynamic memory allocation** (e.g., objects, arrays).
- Shared among all threads.
- Managed by the **Garbage Collector**.
- Objects remain until they are no longer referenced and are garbage collected.
- Can grow/shrink at runtime based on memory needs.
