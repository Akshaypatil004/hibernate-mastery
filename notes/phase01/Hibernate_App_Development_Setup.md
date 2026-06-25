# Hibernate App Development Setup

> **Learning Notes** — Dec 23–24, 2021

---

## Architecture Overview

A **Hibernate App** consists of a **Client App** that uses Hibernate as its ORM (Object-Relational Mapping) layer to communicate with a database.

```
Client App  →  Hibernate Components  →  JDBC  →  RDBMS (DB s/w)
```

> **Note:** "Client App" here means a client of Hibernate (not a client of the DB directly). It is a Java application that uses Hibernate-based O-R mapping persistence logic to locate and interact with DB software.

---

## How It Works (Step-by-Step)

| Step | Description |
|------|-------------|
| **(a)** | Client App uses Hibernate API and Hibernate components to develop **entity class objects** based on O-R mapping persistence logic |
| **(b)** | Client App gives **objects-based persistence instructions** to ORM s/w (Hibernate) |
| **(c)** | ORM s/w internally uses its **libraries (jar files)** and built-in components to generate **JDBC code + SQL query** for objects-based persistence instructions |
| **(d)** | SQL goes to DB s/w and **executes** in DB s/w to complete the persistence operation |
| **(e)** | DB s/w gives **SQL Query execution results** back to ORM s/w (Hibernate) |
| **(f)** | Hibernate **converts SQL Query results** as required by the Client App and sends to Client App |

---

## Components of Hibernate App Development

| # | Component |
|---|-----------|
| (a) | **hibernate.cfg file(s)** — XML file |
| (b) | **Hibernate mapping file(s)** — XML file |
| (c) | **Entity class(es)** |
| (d) | **Client App(s)** |
| (e) | **Hibernate Libraries** — hibernate jar files |

---

## 1. Hibernate Configuration File (`hibernate.cfg.xml`)

### What It Contains
- **JDBC driver details** helping Hibernate locate and connect to the DB s/w
- **Other details** like dialect to use, dynamic schema generation mode, etc.

### Key Concepts Inside the Config File

**Dialect**
- Built-in component of Hibernate that actually generates the SQL query by taking objects-based persistence instructions

**Dynamic Schema Generation**
- Tells Hibernate whether to **create or not** create the DB table based on mapping file/entity class details

**Mapping file names/locations**
- Contains the names and locations of all Hibernate mapping files

### Default File Behaviour
- If you do not specify the name of the Hibernate cfg file in the Application, the app takes `hibernate.cfg.xml` (from the classpath) as the default file
- Any `<file-name>.xml` can be taken as the cfg file — if you don't specify any name, the default file name (`hibernate.cfg.xml`) will be taken
- Information in the cfg file is given as **key-value pairs**, where keys are fixed Hibernate cfg properties and values are provided by the programmer based on the DB s/w and its version

### How Many Config Files?
- Generally **one cfg file per DB** on which the app runs
- If the App talks with one DB s/w → one hibernate cfg file
- If the App talks with two DB s/ws → two hibernate cfg files

---

## Sample `hibernate.cfg.xml`

```xml
<hibernate-configuration>
  <session-factory>

    <!-- JDBC Properties to locate and connect the DB s/w -->
    <property name="hibernate.connection.driver_class">oracle.jdbc.driver.OracleDriver</property>
    <property name="hibernate.connection.url">jdbc:oracle:thin:@localhost:1521:xe</property>
    <property name="connection.username">system</property>
    <property name="connection.password">manager</property>

    <!-- Dialect component to use to generate SQL queries -->
    <property name="hibernate.dialect">org.hibernate.dialect.Oracle10gDialect</property>

    <!-- Name(s) of mapping file(s) -->
    <mapping resource="comt/nt/hbm/Employee.hbm.xml"/>

  </session-factory>
</hibernate-configuration>
```

> **Note:** The word `"hibernate"` in key/property names is **optional**.

### Useful Reference
- All 200+ Hibernate cfg properties (keys):  
  [https://docs.jboss.org/hibernate/orm/3.3/reference/en/html/session-configuration.html](https://docs.jboss.org/hibernate/orm/3.3/reference/en/html/session-configuration.html)

### SessionFactory Object
- The Hibernate App internally reads and stores the information collected from the cfg file and specified mapping file in a special object called the **SessionFactory** object

---

## 2. Entity Class / Model Class / Persistence Class

### Characteristics
- Must be developed as a **Java Bean class** (class with setter and getter methods)
- Mapped with a **DB table** — its properties/attributes/fields will be mapped with DB table columns
- Client Apps use objects of the Entity class to provide **objects-based Persistence Instructions** to ORM software (Hibernate) as part of O-R mapping logic

### Mapping Scale
- Taken on **1 per DB table** basis
- If you have 10 DB tables → 10 Entity classes
- While developing Entity classes, all Java features like **inheritance, composition**, etc. can be applied

### Example

```java
public class Employee {
    private int eno;
    private String ename;
    private String eadd;
    private float salary;

    // 4 setter methods
    // ....

    // 4 getter methods
    // ....
}
```

---

## 3. Hibernate Mapping File (`.hbm.xml`)

### Purpose
- Provides **O-R (Object-Relational) mapping details** to Hibernate, telling it how to map the Entity class to a DB table

### Contents
- Contains entity class name and the corresponding DB table name
- Contains member variable names and the corresponding DB table column names

### Default Name
- There is **no default name** for the Hibernate mapping file
- All mapping file names and locations **must be specified** in `hibernate.cfg.xml`
- Member variables with DB table column and `id` identity property (`field.hbm`) in the cfg file

### Sample Mapping File

```xml
<hibernate-mapping>
  <class name="pkg.Employee" table="EMPLOYEE">     <!-- entity class to DB table in Oracle -->
    <id property name="eno" column="EMPNO (pk)"/>
    <property name="ename" column="ENAME (VC)"/>
    <property name="eadd" column="EADD (VC)"/>
    <property name="salary" column="SALARY (FLOAT)"/>
  </class>
</hibernate-mapping>
```

> **Note:** "Hibernate" in the file name (e.g., `Employee.hbm.xml`) is **optional**. The mapping can be named anything — by convention, the property name is the key name.

---

## 4. Hibernate Libraries (Jar Files)

The jar files can be obtained from Hibernate software downloaded from the internet (in the form of a zip file).

- If working with **Ant/Gradle/Maven**, build tools can facilitate the download of Hibernate App from internet repositories without downloading any software from the internet
- These jar files contain all Programmer interfaces (names, annotations, interfaces used for creating the O-R mapping persistence logic):
  - In Java: API creates a set of classes and functions that are given in the form header files
- While developing Hibernate's O-R mapping persistence logic, we can use:
  - **(a) Pre-defined** apis/programmes defined in Hibernate (or jar files)
  - **(b) User-defined** apps (programmes we write ourselves)
  - **(c) Third party** (like libxml.zip, apache.dbcp.fontain.api, etc.) — **Library/set of APIs**

---

## 5. Client App

- It is a client of Hibernate framework/software
- It is a client of DB s/w having O-R mapping Persistence logic
- The Client App can be any Java App, JEE Technologies App, JEE Framework App having Hibernate API and related libraries and using the steps for building persistence logic in DAO class (DataAccessObject class)
- The Client App contains persistence logic and sends/receives objects-based persistence instructions using Hibernate either directly in the Client App (stand-alone) or in DAO class (if the App is layered App)

---

## Ways to Develop Hibernate Apps

### Two Styles

**(a) Using xml driven:**
- O-R mapping `cfg` takes in mapping files called Hibernate mapping files (mapping annotation creation: `cfg`)
- In Entity classes itself annotation is used for O-R mapping

**(b) Using annotation driven (separate entity class/java bean):**
- In mapping `cfg` takes mapping files called Hibernate mapping files [also `cfg` finds itself annotation for O-R mapping]

---

## Types of Projects

### 1. Product-type Projects
- Requirements will be gathered through market surveys and the software company develops the project as product and keeps the marketing valid
- e.g., Tally, Windows OS, MS Office, Anro-Mine s/w, etc.

### 2. Service-based Projects
- Requirements are gathered from customers, annotations, interfaces that are given in the form pages
- In Java: API creates set of classes and functions that are given in the form header files

**Examples:**
- **AMEX project by CTS** — Airbus project by capgemini
- **TJ governance by TCS** — Carnival Pool-To-Work by TCS
- **Author project by Infosys** and others...

---

## Developer Work Modes in Projects

| Mode | Description |
|------|-------------|
| **Scratch Level Development** | 0 to 100% development |
| **Migration Projects** | Changing Project technologies or software or processes and etc. |
| **(c) Software Engineer in Bug Fixing and Issue Tracking** | — |

---

*Notes taken from classroom session — Dec 23–24, 2021*
