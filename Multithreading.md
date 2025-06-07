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

## 4. Thread Lifecycle
- A thread in Java goes through the following **five main stages** during its lifecycle:

### (i). **New**
- The thread is created but not yet started.
- This happens when you create a thread object using `new Thread()`.

```java
Thread t1 = new Thread(); // New state
```

---

### (ii). **Runnable**
- After calling `start()`, the thread moves to the **Runnable** state.
- The thread is ready to run but is waiting for CPU scheduling.

```java
t1.start(); // Moves to Runnable state
```

---

### (iii). **Running**
- The thread scheduler picks the thread from the runnable pool and gives it CPU.
- Now the thread is actively executing its task.

> Note: The exact time a thread spends in this state is decided by the **JVM thread scheduler**, which depends on OS-level algorithms.

---

### (iv). **Blocked / Waiting / Timed Waiting (Non-Runnable)**
- The thread is **not eligible** for running due to one of the following:
  - Waiting for a monitor lock (Blocked)
  - Waiting indefinitely (`wait()`)
  - Waiting for a specific time (`sleep()` / `join(timeout)`)

---

### (v). **Terminated (Dead)**
- Once the `run()` method finishes execution or the thread is stopped, it enters the **terminated** state.

---

### ?? Summary Diagram (Textual)
```
Refer Diagram(1).png
```

---

> ?? **Pro Tip:**  
> You cannot restart a dead thread. Once terminated, a thread object cannot be started again - doing so results in `IllegalThreadStateException`.

---