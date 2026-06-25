# Three Important Components of Hibernate Programming

> **Learning Notes** — Dec 28th, 2021

---

## Overview

We can place Hibernate's O-R mapping persistence logic in any Java, JEE, or Java framework app — either directly in the App or in the DAO class (layered App) of the Application.

While writing Persistence logic, we need **3 important components:**

| # | Component |
|---|-----------|
| (a) | Configuration object |
| (b) | SessionFactory object |
| (c) | Session object |

> **Analogy with JDBC:** In JDBC persistence logic development, we need `Connection`, `Statement`, and `ResultSet`. Similarly, Hibernate's persistence logic development also needs these 3 objects.

---

## 1. Configuration Object

### What It Does
- **Activates / Bootstraps** Hibernate f/w by using the jar files (libraries) added to `CLASSPATH/BUILDPATH`
- **Reads and holds** the given Hibernate cfg file or the default Hibernate file content
- This object is called **Bootstrap object** because it is bootstrapping (starting) Hibernate f/w by loading the required Java app based on the jar files added to `CLASSPATH/Build path`

### How to Create It

```java
// Option 1: Specify cfg file location explicitly
Configuration cfg = new Configuration();
cfg.configure("com/nt/cfgs/hibernate.cfg.xml");
// Activates Hibernate framework by taking the given hibernate.cfg file from the specified location

// Option 2: Use default hibernate.cfg.xml from CLASSPATH/BUILD Location
Configuration cfg = new Configuration();
cfg.configure();
// Activates/Bootstraps the hibernate framework by taking hibernate.cfg.xml
// from CLASSPATH/BUILD Location (i.e., src folder of Eclipse Project)
```

> **Note:** The `src` folder of an Eclipse Project will be in `CLASSPATH/BUILD Path` by default.
> The Configuration object holds: `hibernate.cfg file`, `hbm mapping files`, `HR mapping files`, etc.

---

## 2. SessionFactory Object

> **Factory to create Session objects**

### What It Does
- Created from the **Configuration object**
- Collects Hibernate cfg file info, also Hibernate mapping files info
- Prepares necessary other objects like dialect, data source pointing to JDBC connection pool, caches, etc.
- Lastly creates a **SessionFactory object** having all those objects and information

### What It Contains (Internally)

```
SessionFactory obj
  ├── HB cfg file data
  ├── HB mapping files data
  ├── Dialect comp
  ├── jdbc connection pool
  ├── caching info
  └── generator info
  └── ...
```

### Characteristics
- **Heavy weight object** — contains multiple details required for the whole App based on instructions collected from mapping files (xml files) and cfg file (xml file)
- **Immutable object** — once the object is created with data, it cannot be modified
- **Thread safe object** — though multiple threads are acting on a single object, no one can modify the data
- **Long lived** object in Hibernate App
- Object of Java class that implements `org.hibernate.SessionFactory` interface
  - If an object name is called with Interface, it is called object of the java class that implements the Interface

### How to Create It

```java
SessionFactory factory = cfg.buildSessionFactory();
```

---

## 3. Session Object

### What It Does
- Created using **SessionFactory object**
- Collects connection object from JDBC connection pool and Mapping file info, and creates the Session object:

```
Session object = con++ (jdbc con obj + Mapping file information)
```

- **Bridge** between Java App and ORM Software (Hibernate framework) to give objects-based Persistence Instructions to ORM Software (Hibernate framework)
- The Applications use HB Session object to give Entity class objects-based Persistence Instructions to Hibernate software (framework)
- The Hibernate software internally uses **Dialect comp support** to generate the equivalent SQL Queries based on the received Objects-based Persistence Instructions

### Characteristics
- **Lightweight object** — contains very little information like `con + Mapping file data`
- **Not thread safe** by default
- **Mutable object** — data can be changed
- Generally we create **more Session objects** in one App — one Session per each persistence operation
  - So we can say these are **short lived objects**
- Object of Java class that implements `org.hibernate.Session` interface

### How to Create It

```java
Session ses = factory.openSession();
```

---

## Full Code Structure in Client App / DAO Class

```java
// Activate or Bootstrap Hibernate f/w
Configuration cfg = new Configuration();
cfg.configure("com/nt/cfgs/hibernate.cfg.xml");

// Create SessionFactory object
SessionFactory factory = cfg.buildSessionFactory();

// Create Session object
Session ses = factory.openSession();
```

---

## Object Creation Summary

| Object | Created Per |
|--------|-------------|
| **Configuration obj** | 1 per DB s/w basis |
| **SessionFactory obj** | 1 per DB s/w basis |
| **Session obj** | 1 per persistence operation basis |

---

## Persistence Operations in Hibernate (Based on O-R Mapping Logic)

### Two Categories

| Category | Description |
|----------|-------------|
| **Single Row Operation** | Manipulates one record at a time |
| **Bulk Operations** | Manipulates more than 1 record at a time |

---

### Single Row Operation Methods

| Method | Purpose |
|--------|---------|
| `ses.save(-)` | For **save** obj operation (Inserting a record) |
| `ses.persist(-)` | For **save** obj operation |
| `ses.load(-)` | For **Load** object operation (selecting a record) |
| `ses.get(-)` | For **Load** object operation |
| `ses.update(-)` | For **update** object creation (updating a record) |
| `ses.delete(-)` | For **delete** object operation (deleting a record) |
| `ses.saveOrUpdate(-)` | For **save or update** obj operation (saving or updating the record) |
| `ses.merge(-)` | For **save or update** obj operation |

---

### Bulk Operations APIs

| API | Full Form |
|-----|-----------|
| **a) HQL / JPQL** | HQL = Hibernate Query Language, JPQL = Java PErsistence Query Language |
| **b) Native SQL** | — |
| **c) HB Criteria API** | — |
| **d) JPA Criteria API** | — |

---

*Notes taken from classroom session — Dec 28th, 2021*
