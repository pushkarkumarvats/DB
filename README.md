# Database Handbook – A Complete Guide for Developers and Students

**Subtitle:** From classical RDBMS to modern distributed databases — theory, practice, operations, and real-world gotchas.

---

## 1. Title & Short Summary
**One-liner:** A production-grade handbook that bridges database fundamentals and modern systems — teaching how databases *actually work* and how to use them effectively in real-world applications.

**Summary:**  
This handbook serves as a comprehensive, production-quality reference for developers (students → SDE2). It covers relational theory (ACID, normalization, keys), SQL mastery, PostgreSQL deep-dive, distributed systems, NoSQL, NewSQL, schema design, performance tuning, observability, migrations, and operational best practices — with runnable examples and exercises.

---

## 2. Table of Contents
1. [Why databases matter — historical context](#why-databases-matter--historical-context)  
2. [Core Concepts — RDBMS fundamentals](#core-concepts--rdbms-fundamentals)  
3. [SQL essentials (practical)](#sql-essentials-practical)  
4. [PostgreSQL deep dive (features students often miss)](#postgresql-deep-dive-features-students-often-miss)  
5. [NewDatabase landscape: NoSQL / NewSQL / specialized DBs](#newdatabase-landscape-nosql--newsql--specialized-dbs)  
6. [Schema design trade-offs (what-students-miss)](#schema-design-trade-offs-what-students-miss)  
7. [Distributed systems & transactions](#distributed-systems--transactions)  
8. [Performance & operational best practices](#performance--operational-best-practices)  
9. [Security & compliance](#security--compliance)  
10. [Testing, CI/CD and Migrations](#testing-cicd-and-migrations)  
11. [Observability & incident handling](#observability--incident-handling)  
12. [Anti-patterns and common mistakes](#anti-patterns-and-common-mistakes)  
13. [Hands-on example (runnable)](#hands-on-example-runnable)  
14. [Exercises & learning roadmap](#exercises--learning-roadmap)  
15. [Cheatsheets & quick references](#cheatsheets--quick-references)  
16. [Further reading & resources](#further-reading--resources)  
17. [Contributing & License](#contributing--license)  
18. [Production checklist](#production-checklist)

---

## 3. Why databases matter — historical context
### Evolution
- **1970 – 2000:** RDBMS (Oracle, MySQL, PostgreSQL). Schema-on-write, vertical scaling, ACID.  
- **2000 – 2010:** NoSQL era — horizontal scale, flexible schema, BASE.  
- **2010 – 2020:** NewSQL — distributed SQL + strong consistency.  
- **Now:** Polyglot persistence: OLTP (Postgres), OLAP (Snowflake), cache (Redis), search (Elasticsearch).

| Era | Model | Scaling | Consistency | Typical Use |
|------|--------|----------|--------------|--------------|
| Classical RDBMS | Tables | Vertical | Strong (ACID) | OLTP |
| NoSQL | Doc / KV / Graph | Horizontal | Eventual | Web-scale |
| NewSQL | SQL + Distributed | Horizontal | Strong (via consensus) | Global OLTP |

> 🔥 **Student-miss:** SQL vs NoSQL is not a war. Real systems mix both.

---

## 4. Core Concepts — RDBMS fundamentals
Relational databases organize data into **tables** (relations) of **rows** and **columns** enforcing **constraints**.

### Keys
| Type | Example | Pros | Cons |
|------|----------|------|------|
| Primary | `id SERIAL` | Uniqueness, fast lookup | Synthetic |
| Foreign | `user_id → users.id` | Referential integrity | Joins cost |
| Composite | `(order_id,item_id)` | Natural relation key | Complex |
| Surrogate vs Natural | UUID vs Email | Stability vs meaning |

### Normalization
**1NF** atomic values → **2NF** no partial dep → **3NF** no transitive → **BCNF** every determinant is a key.

```sql
-- Before
CREATE TABLE orders_bad (
  order_id SERIAL PRIMARY KEY,
  customer_name TEXT,
  total NUMERIC
);
-- After
CREATE TABLE customers(id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE orders(id SERIAL PRIMARY KEY,
                    customer_id INT REFERENCES customers(id),
                    total NUMERIC);
