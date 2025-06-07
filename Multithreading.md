# Java Multithreading

## 1. What is Multithreading?
- **Multithreading** in Java is the process of executing two or more threads concurrently.
- It allows a program to perform multiple tasks at the same time, improving performance and resource utilization, especially on multi-core systems.

---

## 2. What is Thread?
- In Java, a **thread** is the smallest unit of execution within a process.
- Multiple threads can run concurrently within the same process and share the same memory and resources.
- Threads operate independently but can communicate and coordinate with each other.   
- **For Eg:** VLC Media Player is a process, and within it, multiple small tasks like Video, Music, Timer, Progress Bar, etc., are executed. These small tasks are called threads.

---

## 3. Difference between Process and Thread

| Aspect                      | Process                                           | Thread                                          |
|----------------------------|---------------------------------------------------|--------------------------------------------------|
| Definition                 | A program in execution.                          | A lightweight sub-unit of a process.            |
| Weight                     | Heavyweight (contains multiple threads).          | Lightweight (part of a process).                |
| Context Switching          | Slower due to more overhead.                      | Faster due to less overhead.                    |
| Communication              | Inter-process communication is slower and complex.| Communication between threads is faster and easier. |
| Memory                     | Each process has its own address space.           | Threads share the same address space.           |
| Synchronization            | Usually not required (isolated memory).           | May require synchronization to avoid conflicts. |

---

> 🧠 **Note:**  
> - Since threads share the same memory, synchronization is crucial to avoid **race conditions**.
> - Processes are more isolated and secure, whereas threads are more efficient in terms of resource usage.

---