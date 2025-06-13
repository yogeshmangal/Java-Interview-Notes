# Internal Working of HashMap in Java

`HashMap` provides average-case **O(1)** time complexity for **insertion**, **search**, and **deletion**, and works on the principle of **hashing**.

Hashing is a technique where input data is processed through a **hash function**, producing a fixed-size **hash code** or **hash value**.

---

## 🔍 Step-by-Step Internal Working:

### ✅ Step 1: Generate Hash Code
When a key-value pair is inserted, the `hashCode()` method of the key is called to generate a hash code.  

---

### ✅ Step 2: Determine Index
HashMap maintains an internal **array of buckets** (each bucket may be empty or contain a list/tree of entries).  
The final index in the array is calculated using bitwise operation.
```java
index = (n - 1) & hash; // where n is the length of the array
```

---

### ✅ Step 3: Handle Collisions
If multiple keys result in the same index (i.e., hash collision), the following collision resolution strategies are used:
- Initially, entries are stored in a **LinkedList**.
- If entries in a bucket exceed a threshold (typically 8), the list is converted into a **Red-Black Tree** to improve performance to **O(log n)**.

---

### ✅ Step 4: Retrieve Value
To retrieve a value:
- HashMap calculates the hash code and index.
- Then it checks each entry in the bucket using `equals()` to find the **exact key**.

---

### ✅ Step 5: Resize and Rehash
HashMap uses a **load factor** (default is **0.75**) to decide when to resize.
- If the number of elements exceeds `capacity * loadFactor`, a **resize** occurs.
- A new larger array is created, and **all existing entries are rehashed** and placed into new buckets.

---

## ⚙️ Time Complexity

| Operation     | Average Case | Worst Case (due to collision) |
|---------------|--------------|-------------------------------|
| Insert        | O(1)         | O(log n)                      |
| Search        | O(1)         | O(log n)                      |
| Delete        | O(1)         | O(log n)                      |

---
