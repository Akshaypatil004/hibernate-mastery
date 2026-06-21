# Hibernate Mastery

![Java](https://img.shields.io/badge/Java-17-orange)
![Hibernate](https://img.shields.io/badge/Hibernate-6.6.x-blue)
![MySQL](https://img.shields.io/badge/MySQL-8.x-lightblue)
![Maven](https://img.shields.io/badge/Maven-3.x-red)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

> Phase-based Hibernate 6 learning repo — entity mapping, associations, HQL, caching, lifecycle, and JPA. Built with depth, documented with notes, tested with practice.

A hands-on Hibernate 6 ORM reference built as a progressive learning path — covering everything from raw JDBC pain points to entity mapping, associations, querying with HQL, caching strategies, entity lifecycle management, and JPA fundamentals. Each phase contains working code, practice tasks, and personal notes. Culminates in a mini e-commerce project applying every concept end-to-end.

---

## Table of Contents

- [Topics Covered](#topics-covered)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [What I Learned](#what-i-learned)

---

## Topics Covered

| Phase | Topic | Concepts | Status |
|-------|-------|----------|--------|
| Phase 0 | Pre-flight: JDBC vs ORM | JDBC pain points, what ORM solves, Maven setup | ⏳ Upcoming |
| Phase 1 | Configuration & First Entity | SessionFactory, Session, Transaction, @Entity, save(), get() | ⏳ Upcoming |
| Phase 2 | Entity Mapping | @Column, @Embeddable, Enum mapping, Inheritance strategies | ⏳ Upcoming |
| Phase 3 | Associations | @OneToMany, @ManyToMany, CascadeType, FetchType, N+1 problem | ⏳ Upcoming |
| Phase 4 | HQL & Criteria API | HQL, JOIN FETCH, projections, Criteria API, pagination | ⏳ Upcoming |
| Phase 5 | Caching | L1 cache, L2 cache, EHCache, query cache | ⏳ Upcoming |
| Phase 6 | Entity Lifecycle | Entity states, dirty checking, flush, @Version, locking | ⏳ Upcoming |
| Phase 7 | JPA Fundamentals | JPA vs Hibernate, EntityManager, JPQL | ⏳ Upcoming |
| Mini Project | E-Commerce Order System | All concepts applied end-to-end | ⏳ Upcoming |

---

## Project Structure

```
hibernate-mastery/
├── src/
│   └── main/
│       └── java/
│           └── com/akshay/hibernate/
│               ├── phase00_preflight/
│               ├── phase01_config/
│               ├── phase02_mapping/
│               ├── phase03_associations/
│               ├── phase04_hql/
│               ├── phase05_caching/
│               ├── phase06_lifecycle/
│               ├── phase07_jpa/
│               └── miniproject/
├── notes/
│   ├── phase00/
│   ├── phase01/
│   ├── phase02/
│   ├── phase03/
│   ├── phase04/
│   ├── phase05/
│   ├── phase06/
│   └── phase07/
├── pom.xml
├── .gitignore
└── README.md
```

> Each phase package gets `entity/` and `runner/` sub-packages added when that phase begins.
> The `notes/` folder contains personal Markdown notes and phase summaries.

---

## Tech Stack

- Java 17
- Hibernate ORM 6.6.x
- MySQL 8.x
- Maven 3.x
- EHCache 3.x (Phase 5 — L2 caching)
- SLF4J (logging)

---

## Getting Started

### Prerequisites

- Java 17+
- Maven 3.x
- MySQL 8.x running locally

### Clone and Run

```bash
git clone https://github.com/Akshaypatil004/hibernate-mastery.git
cd hibernate-mastery
```

Create the database in MySQL:

```sql
CREATE DATABASE hibernate_mastery;
```

Update your MySQL credentials in `src/main/resources/hibernate.cfg.xml`, then compile:

```bash
mvn compile
```

Run any phase's main class directly from your IDE.

---

## What I Learned

*This section grows after every phase is completed.*

---

Made with focus by Akshay Patil — [LinkedIn](www.linkedin.com/in/akshay-patil-java-dev)
