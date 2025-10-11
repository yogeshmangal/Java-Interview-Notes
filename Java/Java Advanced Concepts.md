# Java Advanced Concepts

## 1. What is Stream, InputStream, OutputStream?

### Stream
A stream is basically a sequence of data (like water flowing in a pipeline). You can either read from it (input) or write to it (output).

- **Stream**: An abstraction in Java used for reading/writing data
- **Direction**: Data flows in one direction
- **Types**: 
  - **InputStream**: Data flows **into** the program
  - **OutputStream**: Data flows **out** of the program

---

### InputStream
- **Definition**: An abstract class in `java.io` package
- **Purpose**: Represents a stream of bytes coming **into** your program
- **Usage**: Use it when you want to **read** data
- **Direction**: External source → Your program

### OutputStream
- **Definition**: An abstract class in `java.io` package
- **Purpose**: Represents a stream of bytes going **out** of your program
- **Usage**: Use it when you want to **write** data
- **Direction**: Your program → External destination

---

### Real-World Examples

#### Upload File Scenario
1. Your program **reads** the file (`InputStream`)
2. Then **writes** it to the server (`OutputStream`)

```
Local File → [InputStream] → Your Program → [OutputStream] → Server
```

#### Download File Scenario
1. Your program **reads** the file from the server (`InputStream`)
2. Then **writes** it to your local file (`OutputStream`)

```
Server → [InputStream] → Your Program → [OutputStream] → Local File
```

---

### Quick Summary
- **Stream**: Flow of data
- **InputStream**: Reading bytes (data coming in)
- **OutputStream**: Writing bytes (data going out)

---

## 2. What is Serialization, Deserialization and transient keyword ?

### a) Serialization
The process of writing the state of an object to a file is called **Serialization**.  
Strictly speaking, it is the process of converting an object from a **Java-supported form** to a **file-supported** or **network-supported** form.  

We can achieve serialization using `FileOutputStream` and `ObjectOutputStream` classes.

### b) Deserialization
The process of reading the state of an object from a file is called **Deserialization**.  
Strictly speaking, it is the process of converting an object from a **file** or **network-supported** form into a **Java-supported** form.  

We can achieve deserialization using `FileInputStream` and `ObjectInputStream` classes.

### Example
```java
import java.io.*;

class Dog implements Serializable {
    int i = 10;
    int j = 20;
}

public class SerializableDemo {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Dog d1 = new Dog();

        // Serialization
        FileOutputStream fos = new FileOutputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(d1);

        // Deserialization
        FileInputStream fis = new FileInputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Dog d2 = (Dog) ois.readObject();

        System.out.println(d2.i + "--" + d2.j);
    }
}
```
#### Output
```java
10--20
```

---

### Notes

a) We can **serialize only serializable objects**.  
b) An object is said to be **serializable only if its class implements the `Serializable` interface**.  
   (In the above example, since the `Dog` class implements `Serializable`, its object can be serialized.)  
c) If we try to serialize a **non-serializable object**, we get a **RuntimeException**: `NotSerializableException`.

### Example
```java
import java.io.*;

class Dog {
    int i = 10;
    int j = 20;
}

public class SerializableDemo {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Dog d1 = new Dog();

        // Serialization
        FileOutputStream fos = new FileOutputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(d1); // ❌ This will throw NotSerializableException

        // Deserialization
        FileInputStream fis = new FileInputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Dog d2 = (Dog) ois.readObject();

        System.out.println(d2.i + "--" + d2.j);
    }
}
```
#### Output
```java
Exception in thread "main" java.io.NotSerializableException: Dog
    at java.base/java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1196)
    at java.base/java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:355)
    at SerializableDemo.main(SerializableDemo.java:15)
```
---

### c) transient Keyword

`transient` is an **access modifier** applicable **only for variables**.  

During **serialization**, if we don’t want to save the value of a particular variable (for example, sensitive data like passwords), we should mark it as `transient`.

At the time of serialization, the **JVM ignores the original value** of transient variables and **stores their default values** in the file.  
Hence, `transient` means **"do not serialize"**.

### Example
```java
import java.io.*;

class Dog implements Serializable {
    int i = 10;
    transient int j = 20; // 'j' will not be serialized and the default value 0 will be saved at the time of Serialization
}

public class SerializableDemo {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Dog d1 = new Dog();

        // Serialization
        FileOutputStream fos = new FileOutputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(d1);

        // Deserialization
        FileInputStream fis = new FileInputStream("C:\\Users\\Yogesh Mangal\\Desktop\\abc.ser");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Dog d2 = (Dog) ois.readObject();

        System.out.println(d2.i + "--" + d2.j);
    }
}
```
#### Output
```java
10--0
```
---

### d) static vs transient Keyword

- `static` variables belong to the **class**, not to any particular object.  
- Serialization saves **only the object’s state** (i.e., instance variables).  
- Hence, `static` variables are **never serialized**, irrespective of whether you mark them `transient` or not.  

📘 **Therefore:**  
Declaring a `static` variable as `transient` has **no effect at all**.  
Both mean “don’t serialize me,” but `static` already guarantees that.

---

### e) final vs transient Keyword

- `final` variables are **constant per object**, and their values are stored in the object’s state.  
- Hence, **final variables are serialized** like normal instance variables.  
- Declaring a variable as both `final` and `transient` means:
  - It **won’t be serialized** (because of `transient`).
  - But since it’s `final`, its **value must be initialized at declaration** — and **that value will be restored** by the constructor or class definition, not from the serialized stream.

📘 **Therefore:**  
Declaring a `final` variable as `transient` has **no practical use**, because its value will be reinitialized to the default defined one.

---

### ✅ Summary Table

| Declaration | Serialized? | Output (i--j) | Explanation |
|--------------|--------------|----------------|--------------|
| `int i = 10; int j = 20;` | ✅ both | `10--20` | Normal case |
| `int i = 10; transient int j = 20;` | `i` only | `10--0` | `j` not serialized, gets default `0` |
| `transient static int i = 10; transient int j = 20;` | none | `10--0` | `i` static → class value, not serialized; `j` transient → skipped |
| `transient final int i = 10; transient int j = 20;` | `i` (as constant) | `10--0` | `i` comes from declaration (since final); `j` skipped |

---