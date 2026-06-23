# Session 02 — JDBC Limitations & Introduction to ORM

## ODB / OODB Software (Quick Overview)

Object-Oriented Database software can store Java objects **directly** as column values.

```java
public class Student {
    private int sno;
    private String sname;
    private String sadd;
    private float avg;
    // getters & setters
}

Student s = new Student(101, "raja", "hyd", 56.55f);
// This entire object can be stored directly in ODB
```

### Pros of ODB
- No need to convert objects to simple values and back
- All CRUD operations directly on objects
- No SQL needed — works as NoSQL

### Cons of ODB
- Does not support relationships between data
- Tight coupling between ODB software and the programming language
- Violates Normalization Rule 1 (storing multiple values in single column)
- Not industry standard — never gained popularity over RDBMS

---

## Types of Database Software

| Type | Full Form |
|------|-----------|
| HDBMS | Hierarchical DBMS |
| NDBMS | Network DBMS |
| RDBMS | Relational DBMS ✅ Industry standard |
| OODBMS | Object Oriented DBMS |

> RDBMS (Oracle, MySQL, PostgreSQL) won the industry because of popularity,
> maturity, and strong relationship support. ODB never took off.

---

## Limitations of JDBC

### (a) DB Software Dependent — Not Portable
- JDBC uses SQL queries which are **DB software dependent**
- Changing DB in middle of development or after release is very complex
- Code written for Oracle may not work directly on MySQL

### (b) Boilerplate Code Problem
Every JDBC operation repeats the same structure:

```
Common logic (repeated everywhere):
  → Load JDBC driver
  → Establish connection
  → Create Statement object

App-specific logic:
  → Send and execute SQL query
  → Gather and process results

Common logic again (repeated everywhere):
  → Handle exceptions
  → Close JDBC objects and connection
```

> Boilerplate code = code that repeats across multiple parts of the app
> with no changes or minor changes.

### (c) Checked SQLException Problem
- JDBC throws `SQLException` which is a **checked exception**
- Mandatory to handle — cannot propagate to caller easily
- Unchecked exceptions are better — optional to catch, support propagation by default

### (d) Single SQLException for All Problems
- No detailed exception classes for different problems
- One generic `SQLException` covers everything — hard to identify exact issue

### (e) Complex JDBC Object Closing
Closing JDBC objects is tricky — wrong approach causes resource leaks.

**Bad practice:**
```java
finally {
    try {
        rs.close();   // if this throws exception,
        st.close();   // st and con never get closed
        con.close();
    } catch(Exception e) {}
}
```

**Good practice — close each separately:**
```java
finally {
    try { if (rs != null) rs.close(); } catch(Exception e) {}
    try { if (st != null) st.close(); } catch(Exception e) {}
    try { if (con != null) con.close(); } catch(Exception e) {}
}
```

**Modern approach — try-with-resources (auto close):**
```java
try (Connection con = DriverManager.getConnection(url, user, pass)) {
    try (Statement st = con.createStatement()) {
        try (ResultSet rs = st.executeQuery("SELECT * FROM STUDENT")) {
            // process results
        }
    }
} catch(Exception e) { }

// Note: Transaction management (commit/rollback) is complex with try-with-resources
```

### (f) ResultSet is Not Serializable
- Cannot send `ResultSet` data directly over network
- Must convert records to other objects/values before sending
- Serialization = converting object data to bits and bytes for network transfer
- Only classes implementing `java.io.Serializable` can be serialized

### (g) Cannot Use Java Objects Directly in SQL
Java is OOP — but JDBC forces you to break objects into simple values:

```java
Student st = new Student(101, "raja", "hyd");

PreparedStatement ps = con.prepareStatement(
    "INSERT INTO STUDENT VALUES(?, ?, ?)");

// Must extract each field manually — object cannot go in directly
ps.setInt(1, st.getSno());
ps.setString(2, st.getSname());
ps.setString(3, st.getSadd());
```

> ORM solves this — you pass the object directly, ORM handles the mapping.

### (h) No Proper Transaction Management Support
**Transaction Management** = combining related operations into one unit,
applying "do everything or nothing" principle.

**Example:** Transfer money = withdraw + deposit — both must succeed or both fail.

| Type | Description | JDBC Support |
|------|-------------|--------------|
| Local TxMgmt | All operations on single DB | ✅ Good support |
| Global TxMgmt | Operations across different DBs | ❌ No proper support |

### (i) Only Positional Parameters — No Named Parameters
```sql
-- JDBC (positional) — hard to read with many params
INSERT INTO STUDENT VALUES (?, ?, ?, ?, ?, ?, ?, ?)

-- Named parameters (not supported in JDBC) — much cleaner
INSERT INTO STUDENT VALUES (:no, :name, :addr, :age, :avg, :m1, :m2, :m3)
```

### (j) Strong SQL Knowledge Required
- Developer must know SQL deeply to work with JDBC
- DB-specific SQL syntax differences add complexity

### (k) Cannot Use OOP Features
- Inheritance, polymorphism, composition — cannot be used in JDBC persistence logic
- JDBC does not accept Java objects as SQL input values

### (l) No Built-in Versioning and Timestamping
- **Versioning** — tracks how many times a record was modified
- **Timestamping** — tracks when record was inserted and last modified
- In JDBC, must write lots of manual code to implement these
- ORM frameworks provide these as built-in features (`@Version`, `@CreationTimestamp`)

---

## Solution — ORM Frameworks

To solve most JDBC problems, use **Object-Relational Mapping (ORM)** frameworks:

| ORM Framework | Notes |
|---------------|-------|
| **Hibernate** | Most popular, industry standard ✅ |
| iBatis / MyBatis | Lighter alternative |
| TopLink | Oracle's ORM |
| EclipseLink | JPA reference implementation |
| Spring ORM | Uses Hibernate/ORM internally |
| Spring Data | Uses ORM internally — hot cake 🔥 |

> Spring ORM and Spring Data internally use one or another ORM framework.
> Learning Hibernate first gives you the foundation to understand all of them.

---

## Key Takeaway

JDBC has 12 major limitations — most of them come down to:
1. You're working with **SQL and simple values** instead of **Java objects**
2. Too much **boilerplate and manual code**
3. **Not portable** across databases

**Hibernate solves all of this** by letting you work with Java objects directly
and handling the SQL generation, mapping, transaction management, and
caching automatically.

