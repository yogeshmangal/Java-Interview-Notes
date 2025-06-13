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

### (i) **New**
- The thread is created but not yet started.
- This happens when you create a thread object using `new Thread()`.

```java
Thread t1 = new Thread(); // New state
```

---

### (ii) **Runnable**
- After calling `start()`, the thread moves to the **Runnable** state.
- The thread is ready to run but is waiting for CPU scheduling.

```java
t1.start(); // Moves to Runnable state
```

---

### (iii) **Running**
- The thread scheduler picks the thread from the runnable pool and gives it CPU.
- Now the thread is actively executing its task.

> Note: The exact time a thread spends in this state is decided by the **JVM thread scheduler**, which depends on OS-level algorithms.

---

### (iv) **Blocked / Waiting / Timed Waiting (Non-Runnable)**
- The thread is **not eligible** for running due to one of the following:
  - Waiting for a monitor lock (Blocked)
  - Waiting indefinitely (`wait()`)
  - Waiting for a specific time (`sleep()` / `join(timeout)`)

---

### (v) **Terminated (Dead)**
- Once the `run()` method finishes execution or the thread is stopped, it enters the **terminated** state.

---

### Summary Diagram (Textual)
- Refer to the following diagram for visual understanding of the thread lifecycle:  
[Thread-Lifecycle.png](https://github.com/yogeshmangal/Java-Interview-Notes/blob/main/Multithreading/Thread-Lifecycle.png)

---

> **Pro Tip:**  
> You cannot restart a dead thread. Once terminated, a thread object cannot be started again - doing so results in `IllegalThreadStateException`.

---

## 5. How to Create Threads in Java?
- In Java, there are **two primary ways** to create threads:

### a) By Extending the `Thread` Class
- You can create a new thread by extending the built-in `Thread` class and overriding its `run()` method.

#### Example:
```java
class Test extends Thread {
    @Override
    public void run() {
        // Task for the thread
        System.out.println("Thread is running...");
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        t.start(); // Starts the thread
    }
}
```

#### Notes:
- The `Thread` class internally implements the `Runnable` interface.
- A thread can only be started **once** using `.start()`. Calling `start()` again on the same thread object will throw:

```java
IllegalThreadStateException
```

---

### b) By Implementing the `Runnable` Interface
- This is the best approach to create Thread as it promotes better design and allows your class to extend another class (Java does not support multiple inheritance).

#### Example:
```java
class Test implements Runnable {
    @Override
    public void run() {
        // Task for the thread
        System.out.println("Thread is running...");
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        Thread th = new Thread(t);
        th.start(); // Starts the thread
    }
}
```

#### Advantages of Using `Runnable`:
- Better separation of task (logic) from thread management.
- More flexible, especially when your class already extends another class.
- Encourages reusability and avoids tight coupling.

---

> **Pro Tip:**  
> Always prefer implementing `Runnable` over extending `Thread` unless you need to override other `Thread` methods.

---

## 6. Different Cases of Executing Threads
- In Java, there are **four common execution cases** while working with threads:

### a) Performing Single Task from a Single Thread

Example:
```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Thread Task");
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        t.start();
    }
}
```

---

### b) Performing Single Task from Multiple Threads

Example:
```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Thread Task");
    }
}

public class Program {
    public static void main(String[] args) {
        Test t1 = new Test();
        t1.start();

        Test t2 = new Test();
        t2.start();
    }
}
```

**Note:**  
- Even though we are creating only two threads (t1 and t2), Java internally also has the **main thread**.  
- So a total of **three threads** will be running: `main`, `t1`, and `t2`.

---

### c) Performing Multiple Tasks from a Single Thread

- This case is Not Possible:
- A single thread executes only one task at a time.
- To perform multiple tasks concurrently, multiple threads are required.

---

### d) Performing Multiple Tasks from Multiple Threads

Example:
```java
class MyThread1 extends Thread {
    @Override
    public void run() {
        System.out.println("Task1");
    }
}

class MyThread2 extends Thread {
    @Override
    public void run() {
        System.out.println("Task2");
    }
}

public class Program {
    public static void main(String[] args) {
        MyThread1 t1 = new MyThread1();
        t1.start();

        MyThread2 t2 = new MyThread2();
        t2.start();
    }
}
```

**Note:**  
- The **order of execution** between `t1` and `t2` is **not guaranteed**.  
- The **JVM thread scheduler** decides which thread to run first based on underlying OS algorithms.

---

## 7. Thread Class Methods
- The `Thread` class provides several methods to manage threads in Java:

---

### a) Basic Methods:
- `run()` - Code that the thread executes.
- `start()` - Starts the execution of the thread.
- `currentThread()` - Returns a reference to the currently executing thread.
- `isAlive()` - Checks if the thread is alive (i.e., started and not yet terminated).

### b) Naming Related Methods:
- `getName()` - Gets the name of the thread.
- `setName(String name)` - Sets the name of the thread.

### c) Daemon Thread Methods:
- `isDaemon()` - Checks if the thread is a daemon thread.
- `setDaemon(boolean b)` - Marks the thread as a daemon thread.

### d) Priority Related Methods:
- `getPriority()` - Gets the priority of the thread.
- `setPriority(int priority)` - Sets the priority of the thread.

### e) Preventing Thread Execution Methods:
- `sleep(long millis)` - Pauses execution for specified milliseconds.
- `yield()` - Causes the currently executing thread to temporarily pause and allow other threads to execute.
- `join()` - Waits for the thread to die.

### f) Thread Interruption Methods:
- `interrupt()` - Interrupts the thread.
- `isInterrupted()` - Checks if the thread has been interrupted.
- `interrupted()` - Static method that checks the current thread's interrupt status and clears it.

### g) Inter-Thread Communication (from `Object` class, not `Thread` class):
- `wait()`
- `notify()`
- `notifyAll()`

---

## Examples

### Basic and Naming Methods (`run()`, `start()`, `currentThread()`, `isAlive()`, `getName()`, `setName()`):

```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Thread name is: " + Thread.currentThread().getName());
    }
}

public class Program {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName());

        Test t1 = new Test();
        t1.start();

        Test t2 = new Test();
        t2.start();

        t1.setName("T1_Thread");
        System.out.println("t1 is Alive?: " + t1.isAlive());
    }
}
```

**Sample Output:**
```
main
Thread name is: T1_Thread
Thread name is: Thread-1
t1 is Alive?: true
```

---

### Daemon Threads (`isDaemon()`, `setDaemon(boolean b)`):
- **Daemon Threads** are the threads which run in the background of another thread. It provides services to other threads.

**Examples of Daemon Threads:**
- Garbage Collector
- Finalizer
- Attach Listeners
- Signal Dispatchers
- Spelling Checkers in MS Word

```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Child Thread");
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        t.setDaemon(true);
        t.start();
    }
}
```

**Note:**  
- You cannot mark the main thread as a daemon because it is created and started by the JVM.

---

### Priority Threads (`setPriority(int priority)`, `getPriority()`)

- Each thread has its own **priority** which can influence how the JVM schedules threads.
- Priorities are integer values from **1 to 10**:

| Priority Constant | Value |
|-------------------|-------|
| `Thread.MIN_PRIORITY` | 1 |
| `Thread.NORM_PRIORITY` | 5 (default) |
| `Thread.MAX_PRIORITY` | 10 |

#### Notes:
- Priorities are **inherited from parent threads**.
- By default, the **main thread** has priority `5`.
- Thread priorities are **hints** to the JVM scheduler. On some platforms (e.g. Windows), priorities may have little or no effect.

#### Example:
```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Child Thread");
    }
}

public class Program {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getPriority());

        Test t = new Test();
        t.setPriority(10);
        t.start();

        System.out.println(t.getPriority());
    }
}
```

**Sample Output:**
```
5
10
Child Thread
```

---
