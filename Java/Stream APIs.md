# Java Stream APIs
The Java Stream API provides a powerful way to process collections of objects using functional-style operations such as filtering, mapping, reducing, and sorting.

---

## For-each Method

```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    lst.forEach(n -> System.out.println(n));
  }
}
```

**Output:**
```
12
7
11
15
```

---

## 1. `map()` Function

### Example 1:
```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    Stream<Integer> data = lst.stream().map(n -> n * 2);
    data.forEach(n -> System.out.println(n));
  }
}
```

**Output:**
```
24
14
22
30
```

### Example 2 (Chained):
```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    lst.stream().map(n -> n * 2).forEach(n -> System.out.println(n));
  }
}
```

### Example 3 (Using collect):
```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    List<Integer> data = lst.stream().map(n -> n * 2).collect(Collectors.toList());
    data.forEach(n -> System.out.println(n));
  }
}
```

**Note:** Once a stream is used, it **cannot be reused**. Attempting to do so will result in an exception.

```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    Stream<Integer> data = lst.stream().map(n -> n * 2);
    data.forEach(n -> System.out.println(n));
    data.forEach(n -> System.out.println(n)); // This will cause an exception
  }
}
```

**Output:**
```
24
14
22
30
Exception in thread "main" java.lang.IllegalStateException: stream has already been operated upon or closed
```

---

## 2. `sorted()` Function

```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 15);
    Stream<Integer> data = lst.stream().sorted();
    data.forEach(n -> System.out.println(n));
  }
}
```

**Output:**
```
7
11
12
15
```

---

## 3. `filter()` Method

```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 14);
    Stream<Integer> data = lst.stream().filter(n -> n % 2 == 1);
    data.forEach(n -> System.out.println(n));
  }
}
```

**Output:**
```
7
11
```

---

## 4. `reduce()` Method

```java
public class Program {
  public static void main(String[] args) {
    List<Integer> lst = Arrays.asList(12, 7, 11, 14);
    int ans = lst.stream().filter(n -> n % 2 == 1).reduce(0, (c, e) -> (c + e));
    System.out.println(ans);
  }
}
```

**Output:**
```
18
```

---