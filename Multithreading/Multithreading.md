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
- `interrupted()` - Static method that checks the current thread's interrupt status and clears it.
- `isInterrupted()` - Checks if the thread has been interrupted.

### g) Inter-Thread Communication (from `Object` class, not `Thread` class):
- `wait()`
- `notify()`
- `notifyAll()`

---

## Examples

### i) Basic and Naming Methods (`run()`, `start()`, `currentThread()`, `isAlive()`, `getName()`, `setName()`):

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

### ii) Daemon Threads (`isDaemon()`, `setDaemon(boolean b)`):
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

### iii) Priority Threads (`setPriority(int priority)`, `getPriority()`)

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

### iv) Preventing Thread Execution Methods (`sleep(long millis)`, `yield()`, `join()`)

These methods help manage the execution timing and coordination of threads.

---

#### `sleep(long millis)`

- Causes the current thread to pause execution for the specified duration (in milliseconds).
- It throws `InterruptedException` if the thread is interrupted while sleeping.

```java
class Test extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            try {
                System.out.println(i);
                Thread.sleep(1000); // Sleeps for 1 second
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
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

#### `yield()`

- Stop the currently executing thread and give a chance for other waiting threads of the same priority to execute.
- It's a hint to the scheduler and doesn't guarantee immediate yielding.

```java
class Test extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + " 0" + i);
        }
    }
}

public class Program {
    public static void main(String[] args) throws InterruptedException {
        Test t = new Test();
        t.start();

        Thread.yield(); // Suggests yielding CPU to another thread

        for (int i = 1; i <= 5; i++) {
            System.out.println("Main Thread " + i);
        }
    }
}
```

---

#### `join()`

- Used when one thread wants to wait for another thread to finish before continuing.
- It blocks the calling thread until the target thread has completed execution.

```java
class Test extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            try {
                System.out.println("Child Thread " + i);
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

public class Program {
    public static void main(String[] args) throws InterruptedException {
        Test t = new Test();
        t.start();
        t.join(); // Main thread waits for t to finish

        try {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Main Thread " + i);
                Thread.sleep(1000);
            }
        } catch (Exception ex) {
        }
    }
}
```

**Sample Output:**
```
Child Thread 1
Child Thread 2
Child Thread 3
Child Thread 4
Child Thread 5
Main Thread 1
Main Thread 2
Main Thread 3
Main Thread 4
Main Thread 5
```

**Note:** Using `join()` ensures that the **main thread waits** for the **child thread** to complete its task before continuing.

---

### v) Thread Interruption Methods: (`interrupt()`, `interrupted()`, `isInterrupted()`)

#### `interrupt()`

- It is used to interrupt an executing thread.
- interrupt() method will only works when the thread is in sleeping or waiting state.
- If a thread is not in sleeping or waiting state then calling an interrupt() method will perform normal behavior.
- When we use an interrupt() method, it throws an InterruptedException.

```java
class Test extends Thread {
    @Override
    public void run() {
        try {
            for (int i = 1; i <= 5; i++) {
                System.out.println(i);
                Thread.sleep(1000);
            }
        } catch (Exception ex) {
            System.out.println("Thread interrupted");
            ex.printStackTrace();
        }
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        t.start();
        t.interrupt();
    }
}
```
**Sample Output:**
```
1
Thread interrupted
java.lang.InterruptedException: sleep interrupted
	at java.base/java.lang.Thread.sleep0(Native Method)
	at java.base/java.lang.Thread.sleep(Thread.java:484)
	at Test.run(Program.java:7)
```

---

#### `interrupted()` and `isInterrupted()`

- Both methods i.e. interrupted() and isInterrupted() are used to check whether a thread is interrupted or not.
- `interrupted()` method will return true if a thread is interrupted and after that it clears the interrupt status from true to false.
- `isInterrupted()` method will return true if a thread is interrupted but it does not clear the interrupt status.  

**Note:** `interrupted()` method will change the result if called twice because it will change the status from true to false.

```java
class Test extends Thread {
    @Override
    public void run() {
        System.out.println("Is Thread Interrupted: " + Thread.interrupted());
        System.out.println("Is Thread Interrupted: " + Thread.currentThread().isInterrupted());
        try {
            for (int i = 1; i <= 5; i++) {
                System.out.println(i);
                Thread.sleep(1000);
            }
        } catch (Exception ex) {
            System.out.println("Thread interrupted");
            ex.printStackTrace();
        }
    }
}

public class Program {
    public static void main(String[] args) {
        Test t = new Test();
        t.start();
        t.interrupt();
    }
}
```
**Sample Output:**
```
Is Thread Interrupted: true
Is Thread Interrupted: false
1
2
3
4
5
```

---

### vi) Inter-Thread Communication Methods: (`wait()`, `notify()`, `notifyAll()`)

- Inter-Thread communication is a mechanism in which a thread releases the lock and enter into the paused state and another thread acquires the same lock and continue to executed. It is implemented by the 3 methods of object class:  
**wait(), notify(), notifyAll()**

---

#### `wait()`

- If any thread calls the wait() method, it causes the current thread to release the lock and wait until another thread invokes the notify() or notifyAll() method for this object, or a specified amount of time has elapsed.

---

#### `notify()`

- This method is used to wake up a single thread and releases the object lock.

---

#### `notifyAll()`

- This method is used to wake up all the threads that are in waiting state.

--- 

**Note:** To call wait(), notify() or notifyAll() method on any object, thread should own the lock of that object i.e. the thread should be inside synchronized area.

---

## 8. Runnable vs Callable

### Runnable - Basics
- Runnable is an interface with a single method: `void run()`
- Used to define tasks that can be executed by a thread.
- Does **not return** any value.
- Cannot throw **checked exceptions**.

#### Example:
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable is running...");
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable task = new MyRunnable();
        Thread thread = new Thread(task);
        thread.start();
    }
}
```

---

### Callable - Basics
- Callable is a generic interface: `V call() throws Exception`
- Used to define tasks that return a result or throw checked exceptions.
- Cannot be used directly with `Thread`. It is used with `ExecutorService`.

#### Example:
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

class MyCallable implements Callable<String> {
    public String call() throws Exception {
        return "Callable finished!";
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        MyCallable task = new MyCallable();
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<String> future = executor.submit(task);
        String result = future.get();  // Blocks until result is available
        System.out.println(result);
        executor.shutdown();
    }
}
```

---

### Runnable vs Callable Comparison

| Feature                  | Runnable            | Callable             |
|--------------------------|---------------------|----------------------|
| Method                   | `run()`             | `call()`             |
| Returns a value?         | No               | Yes               |
| Can throw checked exceptions? | No       | Yes               |
| Used with                | `Thread`            | `ExecutorService` + `Future` |

---

### Real Life Analogy

- **Runnable**: Like asking a friend to do something without expecting anything back.
- **Callable**: Like ordering on Swiggy - you track the order and get the result (food).

---

### When to Use What?

| Scenario                              | Use         |
|---------------------------------------|-------------|
| Simple task in background             | Runnable    |
| Need result from task                 | Callable    |
| Task may throw checked exception      | Callable    |
| Want to wait, cancel, or track result | Callable + Future |

---

## 9. What is Synchronization and its advantages and disadvantages?

- Synchronization is the process by which we can control the accesibility of multiple threads to a particular shared resource.
- Problem which can other without synchronization is Data Inconsistency and Thread Interference like deadlock.

### Advantages of Synchronization:

- No data inconsistency problem.
- No thread interference.

### Disadvantages of Synchronization:

- Increases the waiting time period of threads.
- Create perfomance problems.  

**Note:** To overcome synchronization disadvantages, java provides one packagae java.util.concurrent

---

## 10. How to achieve synchronization?
There are 2 ways to achieve Thread Synchronization:  
(a) **Mutual Exclusive**: Achieved by  
	- Synchronized Method  
	- Synchronized Block  
	- Static Synchronization  

(b) **Cooperation** (Inter Thread Communication in Java): Achieved using methods of `Object` class:  
	- `wait()`  
	- `notify()`  
	- `notifyAll()`  

---

### (a) Synchronized Method:
In this, we use the **synchronized** keyword in the method declaration. It locks the object for any synchronized method.

**Example**
```java
class Table {
    synchronized void printTable(int n) { // synchronized method
        for(int i = 1; i <= 5; i++) {
            System.out.println(n * i);
            try { 
                Thread.sleep(400); 
            } catch(Exception e) { 
                System.out.println(e); 
            }
        }
    }
}

class MyThread1 extends Thread {
    Table t;
    MyThread1(Table t) { 
        this.t = t; 
    }
    public void run() { 
        t.printTable(5); 
    }
}

class MyThread2 extends Thread {
    Table t;
    MyThread2(Table t) { 
        this.t = t; 
    }
    public void run() { 
        t.printTable(100); 
    }
}

public class TestSynchronization {
    public static void main(String args[]) {
        Table obj = new Table(); // only one object
        MyThread1 t1 = new MyThread1(obj);
        MyThread2 t2 = new MyThread2(obj);
        t1.start();
        t2.start();
    }
}
```

---

### (b) Synchronized Block:
Synchronized block is used to lock only a portion of code inside a method. It improves performance by reducing the scope of synchronization.

**Example**
```java
class Table {
    void printTable(int n) {
        synchronized(this) { // synchronized block
            for(int i = 1; i <= 5; i++) {
                System.out.println(n * i);
                try { 
                    Thread.sleep(400); 
                } catch(Exception e) { 
                    System.out.println(e); 
                }
            }
        }
    }
}

class MyThread1 extends Thread {
    Table t;
    MyThread1(Table t) { 
        this.t = t; 
    }
    public void run() { 
        t.printTable(5); 
    }
}

class MyThread2 extends Thread {
    Table t;
    MyThread2(Table t) { 
        this.t = t; 
    }
    public void run() { 
        t.printTable(100); 
    }
}

public class TestSynchronizedBlock {
    public static void main(String args[]) {
        Table obj = new Table(); // only one object
        MyThread1 t1 = new MyThread1(obj);
        MyThread2 t2 = new MyThread2(obj);
        t1.start();
        t2.start();
    }
}
```

---

### (c) Static Synchronization:
When threads access synchronized static methods, the lock is on the class (not the object). Useful when data inconsistency happens due to multiple object access.

**Example**
```java
class Table {
    synchronized static void printTable(int n) {
        for(int i = 1; i <= 5; i++) {
            System.out.println(n * i);
            try { 
                Thread.sleep(400); 
            } catch(Exception e) { 
                System.out.println(e); 
            }
        }
    }
}

class MyThread1 extends Thread {
    public void run() { 
        Table.printTable(1); 
    }
}

class MyThread2 extends Thread {
    public void run() { 
        Table.printTable(10); 
    }
}

public class TestStaticSync {
    public static void main(String args[]) {
        MyThread1 t1 = new MyThread1();
        MyThread2 t2 = new MyThread2();
        t1.start();
        t2.start();
    }
}
```
---

## 11. How writing the synchronized keyword in front of the method works internally?
- When we use **synchronized** keyword, then it means we are telling the JVM that at a time only one thread can access the shared resource. In Java, every object has its own lock. So the internal working 
of **synchronized** keyword is like when multiple threads comes to access the shared resource, the lock of the object is acquired by one of the thread (it can be any thread depends on the JVM) and rest 
of the thread will go into the waiting state. Now the first thread that has acquired the lock will do his job and releases the lock. So now one of the thread from waiting state comes to runnable state 
and acquired the lock and do its job and the process goes on.

---

## 12. Deadlock in Threads
- Deadlock is a situation in concurrent programming where two or more threads are blocked indefinitely, waiting for each other to release resources that they need in order to proceed. In other words, each
thread holds a resource that another thread needs and they are both waiting for each other to release their respective resources, resulting in a deadlock state.

---

## 13. What is a Race Conditions and How to Prevent it?
- A race condition occurs when two or more threads access shared data concurrently, and the final outcome depends on the timing of their execution.
- This can lead to unexpected, inconsistent, or corrupted results because threads are racing to read or write the same memory location.   

**Note:** In Race condition, threads compete for shared resource access while in Deadlock, threads wait forever due to a circular lock hold.

### How to Prevent Race Conditions?  
(a) Using synchronized keyword   
(b) Using  synchronized block   
(c) Using ReentrantLock (from java.util.concurrent.locks)   
(d) Using Atomic Variables   

---

## 14. What is Thread Safety and How to achieve it ?

### What is Thread Safety?
- A **class or method is thread-safe** if it behaves correctly when accessed by **multiple threads simultaneously** - without corrupting data or producing incorrect results.

- Thread safety ensures:
	- No race conditions
	- No inconsistent state
	- No unexpected behaviors

---

### How to Achieve Thread Safety

#### a) Immutability

> An **immutable object** is one whose state cannot change after construction.

Since immutable data never changes, it's inherently thread-safe.

##### Example:
```java
public final class User {
    private final String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

---

#### b) Thread Confinement

> Restrict data access to only one thread. If only one thread can access it, no synchronization is needed.

##### Example: Using local variables
```java
public void process() {
    int localCounter = 0; // Confined to this thread only
    localCounter++;
}
```

##### Example: Using ThreadLocal
```java
ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);
```

---

#### c) Synchronized Collections (Collections.synchronizedXXX())

Java provides synchronized wrappers to make collections thread-safe.

##### Example:
```java
List<String> list = Collections.synchronizedList(new ArrayList<>());
Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
```

- Still need manual synchronization when iterating:
```java
synchronized (list) {
    for (String item : list) {
        // safe iteration
    }
}
```

---

#### d) Concurrent Collections (java.util.concurrent)

These are designed for high concurrency - safer and faster than synchronized wrappers.

##### Common Classes:
| Class                   | Description                              |
|------------------------|------------------------------------------|
| `ConcurrentHashMap`    | Thread-safe map; better than HashMap      |
| `CopyOnWriteArrayList` | Safe for reads; good for infrequent writes |
| `BlockingQueue`        | Used in producer-consumer patterns        |

##### Example: ConcurrentHashMap
```java
Map<String, Integer> map = new ConcurrentHashMap<>();
map.put("key", 1);
int value = map.get("key");
```

- No need for explicit synchronization - thread-safe by design.

---

#### When to Use What?

| Scenario                          | Recommended Approach                     |
|----------------------------------|------------------------------------------|
| Shared read-only data             | Immutability                          |
| Per-thread logic                  | Thread Confinement / ThreadLocal      |
| Simple shared collections         | Collections.synchronizedXXX()         |
| High-concurrency environment      | ConcurrentHashMap, CopyOnWriteArrayList |

---

#### Final Tip
- Thread safety is not just about locking - it's about **designing your program to minimize or isolate shared state**, so that threads don't step on each other.

---

## 15. Producer-Consumer Problem   

The **Producer-Consumer problem** is a classic synchronization problem in multithreading where:

- The **Producer** thread generates data and stores it in a shared resource.
- The **Consumer** thread takes the data from the shared resource and processes it.
- The challenge lies in proper coordination to ensure:
  - The producer doesn't add data when the buffer is full.
  - The consumer doesn't read data when the buffer is empty.

---

### Concepts Involved

- Shared resource (e.g., Queue or Buffer)
- Thread synchronization using `wait()` and `notify()`
- Thread-safe Java utilities like `BlockingQueue`

---

### Solution 1: Using `wait()` and `notify()`

```java
import java.util.LinkedList;
import java.util.Queue;

class SharedBuffer {
    private final Queue<Integer> buffer = new LinkedList<>();
    private final int CAPACITY = 5;

    public synchronized void produce(int value) throws InterruptedException {
        while (buffer.size() == CAPACITY) {
            wait(); // wait until space is available
        }
        buffer.add(value);
        System.out.println("Produced: " + value);
        notify(); // notify consumer that item is available
    }

    public synchronized int consume() throws InterruptedException {
        while (buffer.isEmpty()) {
            wait(); // wait until item is available
        }
        int value = buffer.remove();
        System.out.println("Consumed: " + value);
        notify(); // notify producer that space is available
        return value;
    }
}
```

```java
public class ProducerConsumerExample {
    public static void main(String[] args) {
        SharedBuffer buffer = new SharedBuffer();

        Thread producer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    buffer.produce(i);
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    buffer.consume();
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        producer.start();
        consumer.start();
    }
}
```

---

### Solution 2: Using `BlockingQueue`

Java provides `BlockingQueue` in `java.util.concurrent`, which internally manages synchronization.

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class ProducerConsumerBlockingQueue {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

        Thread producer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    queue.put(i); // blocks if queue is full
                    System.out.println("Produced: " + i);
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                try {
                    int value = queue.take(); // blocks if queue is empty
                    System.out.println("Consumed: " + value);
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        producer.start();
        consumer.start();
    }
}
```

---

### Summary

| Approach               | Pros                              | Cons                                  |
|------------------------|-----------------------------------|---------------------------------------|
| `wait()` / `notify()`  | Fine-grained control               | More error-prone and verbose          |
| `BlockingQueue`        | Simple and thread-safe            | Less flexibility, depends on Java lib |

---
