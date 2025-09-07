# Database Concepts:

## 1. JDBC Connectivity:
JDBC (Java Database Connectivity) is a Java API that allows Java applications to connect and interact with relational databases like MySQL, PostgreSQL, Oracle, etc.

### Steps in JDBC Connectivity:

### (i). Load the JDBC Driver
```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

### (ii). Establish the Connection
```java
Connection conn = DriverManager.getConnection("url", "username", "password");
```

### (iii). Create a Statement
```java
Statement stmt = conn.createStatement();
```

### (iv). Execute the Query
```java
ResultSet rs = stmt.executeQuery("SELECT * FROM Users");
```

### (v). Process the Result
```java
while(rs.next()) {
    System.out.println(rs.getString("name"));
}
```

### (vi). Close the Connection
```java
conn.close();
```

### Complete Example
```java
// Step 1: Load the driver
Class.forName("com.mysql.cj.jdbc.Driver");

// Step 2: Establish connection
Connection conn = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/database_name", 
    "username", 
    "password"
);

// Step 3: Create statement
Statement stmt = conn.createStatement();

// Step 4: Execute query
ResultSet rs = stmt.executeQuery("SELECT * FROM Users");

// Step 5: Process results
while(rs.next()) {
    System.out.println(rs.getString("name"));
}

// Step 6: Close resources (important!)
rs.close();
stmt.close();
conn.close();
```

### Best Practice
Always close resources in reverse order: ResultSet → Statement → Connection

---

## 2. JDBC Statements

## (i). Statement

### Creation
- Created using `Connection.createStatement()`
- Used for simple SQL queries where parameters don't change much
- The SQL query is written as a String each time you execute it

### Example
```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM users WHERE id = 10");
```

### Characteristics
- SQL query is sent to the database every time and the database has to parse and compile it again
- Performance overhead due to repeated parsing and compilation

### Security Vulnerability
If you pass user input directly into the query, it becomes vulnerable to SQL injection:

```java
String userInput = "105 OR 1=1"; 
String query = "SELECT * FROM Users WHERE id = " + userInput; 
Statement stmt = conn.createStatement(); 
ResultSet rs = stmt.executeQuery(query);
```

**Result:** This is vulnerable to SQL injection attacks as malicious input can alter the query structure.

---


## SQL Injection
SQL Injection is a type of security vulnerability where an attacker inserts or injects malicious SQL code into a query, allowing them to manipulate the database beyond the intended logic.

### Example Attack
```java
String userInput = "105 OR 1=1"; 
String query = "SELECT * FROM Users WHERE id = " + userInput; 
Statement stmt = conn.createStatement(); 
ResultSet rs = stmt.executeQuery(query);
```

### What Happens
The final query becomes:
```sql
SELECT * FROM Users WHERE id = 105 OR 1 = 1;
```

### The Problem
- The condition `1 = 1` is always true
- This causes the database to return **all users**, not just the user with `id = 105`
- The attacker has successfully bypassed the intended query logic
- This can lead to unauthorized data access, data theft, or database manipulation

**Note:** SQL Injection can be prevented using Prepared Statements (Parameterized Queries).

---

## (ii). PreparedStatement

## Creation
- Created using `Connection.prepareStatement(sql)`
- You write the SQL query once with placeholders (`?`), then bind values at runtime

## Example
```java
PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM Users WHERE id = ? and name = ?");
pstmt.setInt(1, 10);
ps.setString(2, "john");
ResultSet rs = pstmt.executeQuery();
```

## Characteristics
- **SQL query is compiled once** by the database
- **Faster for repeated execution** due to pre-compilation
- **Prevents SQL Injection** because values are bound safely instead of string concatenation

## Key Benefits
- **Performance**: Query is parsed and compiled only once
- **Security**: Parameters are properly escaped and bound
- **Reusability**: Same PreparedStatement can be executed multiple times with different parameters

---

## (iii). CallableStatement
CallableStatement is a special JDBC interface used to call Stored Procedures in the database.

## JDBC Statements Summary

| Statement Type | Purpose | Performance | Security |
|---------------|---------|-------------|----------|
| **Statement** | Direct SQL query | Slow (recompiled each time) | Vulnerable to SQL injection |
| **PreparedStatement** | Parameterized SQL query | Fast (compiled once) | Prevents SQL injection |
| **CallableStatement** | Call stored procedures/functions | Fast (pre-compiled in DB) | Secure parameter binding |

## Key Points
- **Statement**: Direct SQL queries with poor performance
- **PreparedStatement**: Parameterized SQL queries with better performance
- **CallableStatement**: Calls stored procedures and functions stored in the database

---