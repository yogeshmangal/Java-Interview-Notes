
# Java Core Concepts

## 1. What is JDK ? (Java Development Kit):
>> JDK contains tools that are needed to develop Java Applications. These tools are JRE, Compiler, Debuggers, etc.
>> In summary JDK = JRE + Development Tools

## 2. What is JRE ? (Java Runtime Environment):
>> JRE is a software package that contains JVM, Java class libraries and all the required components to run the Java applications.
>> In summary JRE = JVM + Libraries to run the Java application.

## 3. What is JVM ? (Java Virtual Machine):
>> JVM is a platform dependent Software Engine that interprets and executes Java Bytecode. It acts as intermediary between Java Programs and the underlying hardware, allowing Java code to run
   on any device or operating system that has a compatible JVM installed.
>> The JVM handle tasks such as Garbage Collection, Memory Management and Platform Independence.

Note: Java uses both Compiler and Interpreter:

Java -> Compiler -> Bytecode
Bytecode -> Interpreter(JVM) -> MachineCode

## 4. Inheritance in Java?
>> Java supports 3 types of Inheritance:

a) Single Inheritance  
b) Multilevel Inheritance  
c) Hierarichal Inheritance

### a) Single Inheritance:
eg: 
class Vehicle  
class Car extends Vehicle

### b) Multilevel Inheritance:
eg:
class Animal  
class Dog extends Animal  
class BabyDog extends Dog

### c) Hierarichal Inheritance:
eg:
class Person  
class Student extends Person  
class Teacher extends Person

## 5. What is Object Oriented Programming? (OOPS):
>> It is a methodology or paradigm to design a program using classes and objects. It provides some concepts:
a) Class  
b) Object  
c) Inheritance  
d) Polymorphism  
e) Abstraction  
f) Encapsulation

Real Life Examples:
a) Class - Blueprint of a Car.  
b) Object - A specific car like Maruti Swift.  
c) Inheritance - A SportsCar inherits features of a basic Car.  
d) Polymorphism - A remote control can operate TV, AC, etc. (different behaviors with same interface).  
e) Abstraction - Driving a car without knowing how engine works internally.  
f) Encapsulation - ATM machine hides the inner code but exposes the operations like withdraw, deposit.

## 6. What is String Pool?
>> String Pool is a special area in the Java Heap Memory where String literals are stored. When you create a string using a string literal (eg: "hello"), java checks if a string with the same value
   already exists in the Sring Pool. If it does, the new String reference is simply assigned to the existing string object in the pool, rather than creating a new object. This behavior is known as 
   String interning.
>> This mechanism helps in saving memory by avoiding the creation of duplicate String objects with the same value.
>> But the Strings that are created using the 'new' keyword are not stored in the String Pool and will always create a new object in the Heap memory even if an equivalent String exists in the String Pool.

eg:
String s1 = "hello"  //"hello" is added to String Pool  
String s2 = "hello" // Reuses the same "hello" string from the String Pool  
String s3 = new Sring("hello") // Creates a new String object in the Heap memory.

## 7. Why Strings are Immutable?
>> Becuase Strings in the String Pool are shared among multiple references and any attempt to modify their contents could affect other parts of the code that references to the same String.
>> The other reasons are: Thread Safety, Caching, Security, Performance, etc.

## 8. Stack Memory and Heap Memory:

### a) Stack Memory:
>> Stack Memory is used for static memory allocation and deallocation, where memory is allocated and deallocated in the LIFO manner. 
>> In Java, the stack memory primarily holds primitive data types, method calls and references to the objects in heap memory.
>> Each thread in Java program has its own stack, which stores local variables, method parameters and method calls.
>> The Stack memory is limited in size and is generally smaller than Heap Memory.
>> When a method is called, a new Stack frame is created on the stack to store its local variables and parameters. When the method returns, its stack frame is removed from the Stack.

### b) Heap Memory:
>> Heap Memory is used for Dynamic memory allocation, where memory is allocated and deallocated as needed during program execution.
>> In Java, Objects, Arrays, and Instance variables are stored in the Heap Memory.
>> The Heap Memory is shared among all threads in a Java program and is managed by the garbage collector.
>> Objects created in the Heap Memory persist until they are no longer referenced and subsequently garbage collected.
>> Unlike Stack Memory, Heap Memory does not have a fixed size and can grow or shrink dynamically based on the memory requirements of the Program.
