# Session 01 — What is Persistence?

## What is Persistence?

The process of storing and managing data for a long time is called **persistence**.

- Data stored in application variables and objects is **not** persistence
  - They live in RAM only during application execution
  - Once execution ends, data is lost
  - Cannot be shared across multiple executions or different apps
- To solve this, we write application data to **secondary memory devices** like HDD using files or database software

---

## Important Terminologies

### 1. Persistence Store
The place where data is saved and managed for a long time.

**Examples:** Files, Database software (Oracle, MySQL, PostgreSQL)

### 2. Persistent Data
The data that lives inside a persistence store.

**Examples:** File contents, DB tables and their records

### 3. Persistence Operations
Operations performed on persistent data — also called **CRUD / SCUD** operations.

| Short Form | Full Form |
|------------|-----------|
| C | Create (Insert) |
| R | Read (Select) |
| U | Update (Modify) |
| D | Delete (Remove) |

### 4. Persistence Logic
The code/logic written to perform CRUD operations on a persistence store.

**Examples:**
- I/O Stream logic (Serialization, Deserialization) → for Files
- JDBC code → for RDBMS
- Hibernate code → for RDBMS (via ORM)
- Spring JDBC / Spring ORM / Spring Data → for RDBMS

### 5. Persistence Technology / Framework / Tool
The technology or framework used to **develop** persistence logic.

| Technology / Framework | Type |
|------------------------|------|
| JDBC | Technology |
| Hibernate | Framework / Tool |
| Spring JDBC | Framework |
| Spring ORM | Framework |
| Spring Data | Framework (hot cake 🔥) |

---

## Java Learning Path (as explained)

```
Java Learning
  ├── Language     → Core Java
  ├── Technologies → Advanced Java (JDBC, Servlets...)
  └── Frameworks   → Spring, Hibernate...
```

---

## Data Storage Software vs Data Access Technology

| | What it does | Examples |
|---|---|---|
| **Data Storage Software** | Stores and manages data | Oracle, MySQL, PostgreSQL |
| **Data Access Technology** | Accesses and manipulates data of storage software | JDBC, Hibernate, Spring JDBC |

### How Java apps connect to different storage types

```
Java App  ──[I/O Streams: Serialization]──────►  Flat Files (.txt, .java, etc.)

Java App  ──[JDBC / ORM Mapping]───────────────►  RDBMS (Oracle, MySQL, etc.)

Java App  ──[JDBC + SQL/J]──────────────────────►  ODB Software (Object Databases)
```

---

## Limitations of Flat Files as Persistence Stores

| # | Limitation |
|---|-----------|
| 1 | No security |
| 2 | No constraints |
| 3 | No SQL support |
| 4 | Getting data with multiple conditions is very complex |
| 5 | Performing update and delete is very complex |
| 6 | No relationships between data |
| 7 | Merging and comparison of data is very complex |

> **Note:** Files are used as persistence stores only in memory-critical apps like mobile apps, mobile games, and desktop games.

---

## Key Takeaway

Hibernate is a **persistence framework** — it sits between your Java application and your RDBMS, and handles all the persistence logic so you don't have to write raw JDBC code manually.

