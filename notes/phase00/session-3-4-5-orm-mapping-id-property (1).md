# Phase 0 – Session 3, 4 & 5: Introduction to O-R Mapping, Advantages & Id Property in Entity Class

**Date:** December 20–22, 2021 (Notes Reference)
**Topic:** ORM Concepts, Advantages of O-R Mapping, and Identity Property in Entity Classes

---

## 1. Limitations of JDBC — Why ORM?

JDBC requires developers to write raw SQL queries for every persistence operation. This tightly couples Java code to a specific database and makes the codebase verbose and error-prone.

**O-R Mapping (Object-Relational Mapping)** solves this by letting developers work purely with Java objects, while the ORM framework handles SQL generation internally.

---

## 2. What is Serialization?

Serialization is the process of converting a Java object into **bits and bytes** so it can be:
- Written to a file
- Sent over a network

To make a class serializable, it must implement the `java.io.Serializable` marker interface.

```java
public class Emp2 implements java.io.Serializable {
    private int eno;
    private String ename;
    // ...
}
```

> **Marker Interface** — An empty interface that signals the JVM to enable special runtime behaviour (e.g., `java.io.Serializable`, `java.lang.Cloneable`, `java.rmi.Remote`, `javax.servlet.SingleThreadModel`).

---

## 3. What is O-R Mapping?

**O-R Mapping** is the process of **mapping Java classes to database tables** such that:

| Java Side         | DB Side              |
|-------------------|----------------------|
| 1 Java class      | 1 DB table           |
| 1 member variable | 1 column of DB table |
| 1 Java object     | 1 row/record in DB   |

The ORM framework keeps **objects and DB table rows in sync** — modifications to an object are reflected in the DB row, and vice versa.

### How Synchronization Works

```
Java App                         Oracle/MySQL DB
─────────────────────────────    ─────────────────────────
e1 (Employee obj) → empno=101    Row 101 | raja | hyd | 90000
e2 (Employee obj) → empno=102    Row 102 | ramesh | delhi | 10000

ORM (Hibernate) generates:
  UPDATE Employee SET salary=91000 WHERE empno=101  ← object to row
  SELECT * FROM Employee WHERE empno=102            ← row to object
```

### CRUD Mapping in ORM

| ORM Operation      | Generated SQL             |
|--------------------|---------------------------|
| Save object        | `INSERT INTO ...`         |
| Update object      | `UPDATE ... SET ...`      |
| Load object        | `SELECT * FROM ...`       |
| Delete object      | `DELETE FROM ...`         |

---

## 4. Entity Class / Persistence Class / Model Class

```java
public class Employee {
    // Member variables / attributes
    private int eno;
    private String ename;
    private String eadd;
    private float salary;

    // Setters — for modifying data
    // Getters — for reading data
}
```

The mapping between the Java class and DB table is configured via **XML or Annotations** (O-R mapping config files).

```
Employee (class)  ←──[o-r mapping cfg]──→  Employee (db table)
    eno                                          empno (PK)
    ename                                        empname
    eadd                                         empadd
    salary                                       empsalary
```

---

## 5. Java-based ORM Software List

| ORM Framework | Vendor             |
|---------------|--------------------|
| Hibernate     | SoftTree / RedHat  |
| iBatis        | Apache             |
| Eclipse Link  | Eclipse            |
| TopLink       | Oracle Corp        |
| MyBatis       | Apache             |
| JDO           | Adobe              |

---

## 6. Developer Workflow with Hibernate

```
Developer
    ↓  develops
Java App (O-R mapping logic)
    ↓  uses
Hibernate (ORM Software)
    ↓  uses
JDBC + SQL Queries
    ↓  uses
JDBC Driver
    ↓  talks to
RDBMs DB s/w (MySQL / Oracle)
```

> Hibernate **hides** the JDBC complexity and SQL from developers. It provides abstraction on top of JDBC by generating SQL queries internally based on persistence operations.

---

## 7. The Id Property in Entity Class

### Why is the Id Property Needed?

- **JVM** identifies each object by its `hashCode`.
- **Hibernate** identifies each Entity object by its **Id value**.

The Id property is **mandatory** in every O-R mapping configuration (via `<id>` tag or `@Id` annotation).

```
JVM
├── Hibernate Framework
│   └── Entity obj
│       ├── ...
│       ├── Id value = 101    ← used by Hibernate for sync
│       └── hashCode = 35AF3545  ← used by JVM
```

### Strategies to Pick the Id Property

| Scenario | Strategy |
|----------|----------|
| DB table has a **single-column PK** | Map that column as the Id field using `<id>` tag or `@Id` annotation → called **Singular Id Property** |
| DB table has a **composite PK** (multiple columns) | Map those columns as composite Id using `<composite-id>` tag or `@EmbeddedId` annotation → called **Composite Id Property** |
| DB table has **no PK or unique constraint** | Reject that table — ORM cannot reliably sync without a unique identifier |

### Key Definitions

- **Singular PK** — PK constraint applied on 1 column of a DB table → `empno(PK)`
- **Composite PK** — PK constraint applied on multiple columns → `empno + ename` together form the PK
- **Singular Id Property** — When 1 property of the Entity class is taken as the Id property
- **Composite Id Property** — When multiple properties together form the Id property

---

## 8. Advantages of O-R Mapping

### ✅ DB-Independent Persistence Logic
Write persistence logic once and it works across multiple DB s/w. Changing the DB in development or production is easy.

### ✅ No Need to Write SQL Queries
All persistence operations are performed through objects — no raw SQL required.

### ✅ Object-Based Data Transfer Over Network
By making Entity classes `Serializable`, DB table records can be sent over the network as serializable objects.

### ✅ Built-in Services
ORM frameworks provide rich built-in features:

| Feature      | Description |
|--------------|-------------|
| Versioning   | Tracks how many times a record has been modified |
| Timestamping | Tracks when an object was last updated |
| Caching      | Stores records/objects in a buffer to reduce DB round trips for repeated requests |

### ✅ Dynamic Schema Generation
Based on Entity class details, ORM can auto-generate DB tables dynamically.

### ✅ PK Generator Support
ORM provides generators to auto-generate Primary Key column values.

### ✅ Addresses Java–RDBMS Mismatches
For example, Java supports inheritance but RDBMS does not. ORM bridges this gap by mapping inheritance hierarchies to one or more DB tables.

---

## 9. Caching in ORM

```
Client App (O-R mapping logic)
    (a1) = getRecord()
                ↓  a1 to a4: 1st request/persistence operation
    buffer/cache ←──────────────── DB table (DB s/w)
    (a4) result
    (a2?) already in cache?
    ...
    (b3) gives the record ← b1 to b3: 2nd request (same record)
    b1 – get same record from cache (no DB hit)
```

> **Note:** To keep cached data fresh, the cache must be cleared at regular intervals (e.g., every 1 min or 30 sec).

---

## 10. When to Use JDBC vs Hibernate vs BigData

| Scenario | Recommendation |
|----------|---------------|
| Batch processing of 30L–2L records at a time | **JDBC** — creates a single `ResultSet`, less memory, less CPU |
| Small–medium data (30–40 records), needs DB portability, versioning, caching | **Hibernate (ORM)** — Banking apps, e-commerce, college/university apps |
| Very large data beyond RDBMS capacity (PB scale) | **BigData frameworks** — Hadoop or Spark (both Java-based) |

### BigData Size Reference
```
1024 bytes = 1 KB
1024 KB    = 1 MB
1024 MB    = 1 GB
1024 GB    = 1 TB
1024 TB    = 1 PB
1024 PB    = 1 EB
1024 EB    = 1 ZB
1024 ZB    = 1 YB
```
> YouTube generates ~13 PB/day. Facebook generates ~10 TB/hour. This data cannot be stored or processed by regular RDBMS like Oracle — BigData systems like Hadoop/Spark use a **collection of ordinary computers in a network** to store and process it.

---

## Summary

| Concept | Key Takeaway |
|---------|--------------|
| O-R Mapping | Maps Java class ↔ DB table; object ↔ row; variable ↔ column |
| Hibernate | Java-based, open-source, lightweight ORM framework by SoftTree/RedHat |
| Entity Class | Java class with member variables, getters/setters, mapped to a DB table |
| Id Property | Mandatory in ORM config; used by Hibernate to identify & sync objects |
| Caching | Reduces DB round trips for repeated requests using a buffer |
| ORM vs JDBC | ORM for small-medium apps; JDBC for massive batch processing |
