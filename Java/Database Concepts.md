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
pstmt.setString(2, "john");
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

## 3. Relational vs Non-Relational Database:

### Relational Database
- **Structure**: Stores data in tables (rows and columns) with predefined schema
- **Schema**: Fixed schema that must be defined before data insertion
- **Examples**: MySQL, PostgreSQL, Oracle, SQL Server, SQLite

### Key Characteristics
- ACID compliance (Atomicity, Consistency, Isolation, Durability)
- Uses SQL for querying
- Strong data integrity and consistency
- Well-defined relationships between tables

### Non-Relational Database (NoSQL)
- **Structure**: Stores data in flexible formats like JSON, key-value pairs, documents, graphs, etc.
- **Schema**: No fixed schema - schema-less or dynamic schema
- **Examples**: MongoDB, Neo4j, Redis, CouchDB, DynamoDB, Cassandra

### Types of NoSQL Databases
- **Document**: MongoDB, CouchDB
- **Key-Value**: Redis, DynamoDB
- **Column-Family**: Cassandra, HBase
- **Graph**: Neo4j, ArangoDB

### Key Characteristics
- Horizontal scalability
- Flexible schema
- High performance for specific use cases
- Better for handling unstructured data

### When to Use Which?

| Use Case | Relational | Non-Relational |
|----------|------------|----------------|
| **Structured data** | ✅ Excellent | ❌ Not ideal |
| **Complex queries** | ✅ SQL support | ⚠️ Limited |
| **ACID transactions** | ✅ Strong support | ⚠️ Varies |
| **Scalability** | ⚠️ Vertical scaling | ✅ Horizontal scaling |
| **Flexible schema** | ❌ Rigid | ✅ Very flexible |
| **Real-time analytics** | ⚠️ Moderate | ✅ Excellent |

---

## 4. PUT vs PATCH:

### PUT Method
- **Purpose**: Full replacement of a resource
- **Requirement**: Must send the entire object
- **Behavior**: Creates a new record if the record doesn't exist (idempotent)

#### Example - Initial Resource
```json
{
  "id": 1,
  "name": "Yogesh",
  "email": "ymangal@gmail.com",
  "isActive": true
}
```

#### Example - PUT Request to Update Name
**Endpoint**: `PUT /users/1`

**Request Body** (must include ALL fields):
```json
{
  "id": 1,
  "name": "Yogesh Mangal",
  "email": "ymangal@gmail.com",
  "isActive": true
}
```

**Note**: Even if you're updating just one field, you must send the entire object.

---

### PATCH Method
- **Purpose**: Partial update of a resource
- **Requirement**: Send only the specific fields to update
- **Behavior**: Usually does nothing if the resource doesn't exist

#### Example - PATCH Request to Update Name
**Endpoint**: `PATCH /users/1`

**Request Body** (only the field to update):
```json
{
  "name": "Yogesh Mangal"
}
```

---

## 5. What is GraphQL?

**GraphQL** is a query language and runtime for APIs. It allows clients to request exactly the data they need (no more, no less) from APIs.

## Example Query
If you want a user's name and email only from a GraphQL API:

**Query:**
```graphql
{
  user(id: 1) {
    name
    email
  }
}
```

**Response:**
```json
{
  "data": {
    "user": {
      "name": "Yogesh",
      "email": "ymangal@gmail.com"
    }
  }
}
```

## GraphQL vs REST

| Aspect | GraphQL | REST |
|--------|---------|------|
| **Endpoints** | Single endpoint | Multiple endpoints |
| **Data Fetching** | Exact data needed | Fixed data structure |
| **Over-fetching** | Eliminated | Common problem |
| **Under-fetching** | Single request | Multiple requests needed |
| **Caching** | Complex | Simple |
| **Learning Curve** | Steeper | Gentler |

---


