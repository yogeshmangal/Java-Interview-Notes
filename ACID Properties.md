# ACID Properties

**ACID** is a set of properties that ensure reliable processing of database transactions. It stands for:

---

## 💡 A - Atomicity
**All or nothing.**

- A transaction must either complete entirely or not at all.
- If any part of the transaction fails, the entire transaction is rolled back.

**Example:**  
If you're transferring money from Account A to B, both debit and credit should happen—or none at all.

---

## 💡 C - Consistency
**Valid state before and after the transaction.**

- A transaction should bring the database from one valid state to another.
- It ensures that rules (constraints, triggers, etc.) are not violated.

**Example:**  
If your account balance must not go negative, a transaction violating that will be rejected.

---

## 💡 I - Isolation
**Transactions don’t interfere with each other.**

- Even when multiple transactions happen at the same time, each should work as if it's alone.
- Levels of isolation: Read Uncommitted, Read Committed, Repeatable Read, Serializable.

**Example:**  
While you’re booking a movie ticket, someone else shouldn’t book the same seat at the same time.

---

## 💡 D - Durability
**Once committed, it sticks.**

- After a transaction is committed, it remains so—even in the event of a crash.
- Data changes are persisted to disk and survive system failures.

**Example:**  
If you get a success message after booking a ticket, the booking won’t disappear if the system restarts.
