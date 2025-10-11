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

## 2. What is Serialization and Deserialization?