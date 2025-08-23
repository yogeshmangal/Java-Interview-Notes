# Sharding vs Partitioning in Databases

## Partitioning
- **Definition**: Splitting a single database table into smaller, more manageable pieces (partitions).
- **Scope**: Happens within a **single database instance**.
- **Purpose**: Improve performance, manageability, and reduce query scans.
- **Types**:
  - **Horizontal Partitioning**: Divide rows based on a key (e.g., `region`).
  - **Vertical Partitioning**: Divide columns into separate tables (frequently used vs rarely used).

### Horizontal Partitioning Example
**Users Table**  
| user_id | name  | region | email            |
|---------|-------|--------|------------------|
| 1       | Rahul | North  | rahul@mail.com   |
| 2       | Priya | South  | priya@mail.com   |
| 3       | Arjun | North  | arjun@mail.com   |
| 4       | Kavya | East   | kavya@mail.com   |

**Partitions**
- `Users_North` → rows with `region = North`
- `Users_South` → rows with `region = South`
- `Users_East` → rows with `region = East`

You still query `SELECT * FROM users WHERE region='North'`, and the DB routes to the correct partition.

### Vertical Partitioning Example
**Users Table (before)**  
| user_id | name  | email | password_hash | address | bio (TEXT) | profile_pic (BLOB) |

**After Vertical Partitioning**

**Users_Core**  
| user_id | name  | email | password_hash |

**Users_Profile**  
| user_id | address | bio (TEXT) | profile_pic (BLOB) |

Queries for login only hit `Users_Core`. Profile viewing joins with `Users_Profile`.

---

## Sharding
- **Definition**: Distributing data across **multiple database servers (shards)**.
- **Scope**: Happens across **many DB instances**.
- **Purpose**: Handle **scalability** and **large data/traffic** beyond one DB server.
- **How it works**:
  - Each shard is its own database with its own storage and compute.
  - A shard key (e.g., `user_id`) decides where data lives.

### Sharding Example
- Shard 1 → `users` table with `user_id` 1–1M
- Shard 2 → `users` table with `user_id` 1M–2M
- App logic (or middleware) must decide:
  ```pseudo
  shard_id = user_id % 2   # Even → Shard1, Odd → Shard2
  ```

---

## Key Differences

| Feature           | Partitioning (Single DB)                   | Sharding (Multiple DBs)            |
|-------------------|---------------------------------------------|-------------------------------------|
| **Level**         | Within a single DB instance                | Across multiple DB instances        |
| **Goal**          | Performance, manageability                 | Scalability, load distribution      |
| **Management**    | DB engine handles partitions internally    | Application/middleware handles logic|
| **Visibility**    | Appears as one logical table               | Separate DBs/tables per shard       |
| **Querying**      | Normal SQL query                           | App must route query to correct shard|

---

## ✅ In Short
- **Partitioning** = Dividing data inside one DB (transparent to user).
- **Sharding** = Dividing data across multiple DBs (app must handle routing).
