# OOPS Concepts

**Object-Oriented Programming(OOPS)** is a methodology to design a program using classes and objects. It provides the following core concepts:

## 1. Class

A **class** is a blueprint or template that defines properties
(attributes) and behaviors (methods).

**Real-life Example:**\
A "Car" class defines attributes like `color`, `model`, `speed` and
methods like `start()`, `stop()`.

---

## 2. Object

An **object** is an instance of a class. It represents a real-world
entity.

**Real-life Example:**\
Your car (red Honda Civic) is an **object** of the "Car" class.

---

## 3. Inheritance

**Inheritance** allows one class to acquire properties and methods of
another class.

**Real-life Example:**\
A "SportsCar" class inherits from "Car" class and gets all features of
Car, plus extra features like `turboBoost()`.

---

## 4. Polymorphism

**Polymorphism** means "many forms" -- the ability to use a single
interface with different implementations. Polymorphism has 2 types - Method Overloading and Method Overriding.

**Real-life Example:**\
The `start()` method in "Car" and "Bike" classes may work differently,
but both are called `start()`.

---

## 5. Abstraction

**Abstraction** hides implementation details and shows only the
essential features. It is implemented using Interface and Abstract class.

**Real-life Example:**\
When you drive a car, you just press the accelerator. You don't need to
know how the engine internally increases speed.

---

## 6. Encapsulation

**Encapsulation** means bundling data and methods inside a class and
restricting direct access (We cannot modify the variables directly, we have to use getters() and setters() methods.)

**Real-life Example:**\
Car's `speed` cannot be directly changed by outside code; it can be
changed only through `accelerate()` or `brake()` methods.

---
