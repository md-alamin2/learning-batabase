# 📚 Learning Database

A beginner-friendly guide to the core concepts of databases: data, information, keys, the SDLC, and how to design a database system step by step.

## 📑 Table of Contents

1. [Database Basics](#-database-basics)
2. [Keys in a Database](#-keys-in-a-database)
3. [SDLC (Software Development Life Cycle)](#-sdlc-software-development-life-cycle)
4. [System Design: Designing a Database](#-system-design-designing-a-database)

---

## 🧱 Database Basics

### What is a database?

A **database** is a structured collection of related data, organized for efficient storage, retrieval, and management.

It helps us:

- Store large amounts of information
- Retrieve data quickly
- Update records efficiently
- Manage data securely and consistently

### What is data?

**Data** is a collection of facts, figures, symbols, or observations that can be recorded and processed.

| Type | Examples |
| ---- | -------- |
| Numbers | `200`, `18`, `75` |
| Text | names, addresses, messages |
| Dates | `2026-10-05` |
| Media | images, audio, video files |

### What is information?

**Information** is processed and organized data that has meaning and helps people make decisions.

| | Example |
| --- | --- |
| Raw data | `200, 18, 75` |
| Information | "A student scored 75 out of 100 in a test" |

> **Data + Context/Processing = Information**

---

## 🔑 Keys in a Database

A **key** in a relational database is a field (or a combination of fields) that uniquely identifies a record in a table.

### Types of keys

| Key | One-line meaning |
| --- | ---------------- |
| Super Key | Any attribute set that uniquely identifies a row |
| Candidate Key | A minimal super key |
| Primary Key | The candidate key chosen as the main identifier |
| Alternate Key | Candidate keys not chosen as the primary key |
| Simple Key | A key made of a single attribute |
| Composite Key | A key made of two or more attributes |
| Foreign Key | An attribute that refers to the primary key of another table |

We will use this `users` table in the examples:

| u_id | email | name |
| ---- | ----- | ---- |
| 1 | a@mail.com | Rahim |
| 2 | b@mail.com | Karim |

### Super Key

An attribute, or a set of attributes, that can identify each row uniquely. It may contain extra attributes that are not needed for uniqueness.

**Examples:** `{u_id}`, `{email}`, `{u_id, name}`

### Candidate Key

A super key whose proper subset is **not** a super key. Also called a **minimal super key**. It is a potential primary key.

- `{u_id}` → ✅ candidate key
- `{email}` → ✅ candidate key
- `{u_id, name}` → ❌ not a candidate key (`{u_id}` alone is already enough)

### Primary Key

The one candidate key chosen as the main identifier of the table.

Rules: it must be **unique**, **not null**, and **stable** (it should rarely change).

```text
candidate keys = {u_id}, {email}
primary key    = {u_id}
```

### Alternate Key

Candidate keys that were **not** chosen as the primary key.

```text
candidate keys = {u_id}, {email}
alternate key  = {email}
```

### Simple Key

A key that consists of **only one attribute**.

**Example:** `{u_id}`

### Composite Key

A key that consists of **two or more attributes** that together uniquely identify a record.

**Example:** in an `enrollment` table, `{student_id, course_id}` identifies each row, while neither column alone does.

### Foreign Key

An attribute in one table that refers to the **primary key of another table**, creating a relationship between the two tables.

```text
course.instructor_id  ──►  instructor.id
```

---

## 🔄 SDLC (Software Development Life Cycle)

The **SDLC** is a structured process for building software, from the first idea to a running system. Database design is one part of it.

```text
┌─────────────────┐
│  1. Planning    │
└────────┬────────┘
         ▼
┌─────────────────┐
│  2. Analysis    │
└────────┬────────┘
         ▼
┌─────────────────┐
│ 3. System Design│
└────────┬────────┘
         ▼
┌─────────────────┐
│  4. Building    │
└────────┬────────┘
         ▼
┌─────────────────┐
│  5. Testing     │
└────────┬────────┘
         ▼
┌─────────────────┐
│  6. Deployment  │
└─────────────────┘
```

| # | Phase | What happens |
| - | ----- | ------------ |
| 1 | **Planning** | Define the goal, scope, budget, timeline, and team. |
| 2 | **Analysis** | Gather and study requirements: what must the system do? |
| 3 | **System Design** | Design the architecture, database, and interfaces. |
| 4 | **Building** | Write the code and create the database. |
| 5 | **Testing** | Find and fix bugs; check that requirements are met. |
| 6 | **Deployment** | Release the system to real users and maintain it. |

---

## 🛠️ System Design: Designing a Database

System Design is phase 3 of the SDLC. Here we turn requirements into a database design.

**Example project:** a **Course Enrollment System**, where students enroll in courses taught by instructors.

### Step 1: Determining Entities

An **entity** is a real-world thing or concept about which we want to store data. Each entity usually becomes a **table**.

To find entities, read the requirements and pick out the important **nouns**.

| Requirement | Entity |
| ----------- | ------ |
| Students enroll in courses | `student` |
| Courses are available for enrollment | `course` |
| Instructors teach courses | `instructor` |

### Step 2: Determining Attributes for Each Entity

An **attribute** is a property that describes an entity. Each attribute becomes a **column**. Choose a primary key (PK) for each entity, and mark foreign keys (FK).

**student**

| Attribute | Notes |
| --------- | ----- |
| `id` | PK |
| `name` | |
| `email` | |

**course**

| Attribute | Notes |
| --------- | ----- |
| `id` | PK |
| `c_name` | course name |
| `instructor_id` | FK → `instructor.id` |

**instructor**

| Attribute | Notes |
| --------- | ----- |
| `id` | PK |
| `i_name` | instructor name |
| `gender` | |

### Step 3: Relationships Among Entities

A **relationship** shows how two entities are connected. **Cardinality** says how many records on one side can be linked to records on the other side.

| Cardinality | Meaning | Example |
| ----------- | ------- | ------- |
| One-to-One (1:1) | One record links to exactly one record | A person has one passport |
| One-to-Many (1:N) | One record links to many records | One instructor teaches many courses |
| Many-to-Many (M:N) | Many records link to many records | Many students enroll in many courses |

Relationships in our system:

| Entities | Cardinality | Explanation |
| -------- | ----------- | ----------- |
| `instructor` → `course` | **1 : N** | One instructor can teach many courses; each course has one instructor. |
| `student` ↔ `course` | **M : N** | One student can enroll in many courses; one course can have many students. |

### Step 4: Resolving Many-to-Many Relationships

A relational database **cannot** store a many-to-many relationship directly between two tables. If we tried, we would need to store lists of values in one column, which breaks the rules of good design and causes duplication and update problems.

**Solution:** create a third table, called a **junction table** (also called a bridge, linking, or associative table). It turns one M:N relationship into **two 1:N relationships**.

```text
student  M ────── N  course          (not allowed directly)

student  1 ──── N  enrollment  N ──── 1  course     (resolved)
```

The junction table:

- Holds a **foreign key to each** of the two tables
- Uses those two foreign keys together as a **composite primary key**
- Can store extra data that belongs to the relationship itself (for example `enrolled_on`, `grade`)

**Junction table: `enrollment`**

| Attribute | Notes |
| --------- | ----- |
| `student_id` | PK (part 1), FK → `student.id` |
| `course_id` | PK (part 2), FK → `course.id` |
| `enrolled_on` | extra attribute |

### Final ER Diagram

```text
┌───────────────┐ 1       N ┌───────────────┐
│  instructor   │───────────│    course     │
├───────────────┤  teaches  ├───────────────┤
│ id (PK)       │           │ id (PK)       │
│ i_name        │           │ c_name        │
│ gender        │           │ instructor_id │ (FK)
└───────────────┘           └───────┬───────┘
                                    │ 1
                                    │ has
                                    │ N
┌───────────────┐ 1       N ┌───────┴───────┐
│   student     │───────────│  enrollment   │
├───────────────┤   makes   ├───────────────┤
│ id (PK)       │           │ student_id    │ (PK, FK)
│ name          │           │ course_id     │ (PK, FK)
│ email         │           │ enrolled_on   │
└───────────────┘           └───────────────┘
```

### Example SQL

```sql
CREATE TABLE instructor (
    id      INT PRIMARY KEY,
    i_name  VARCHAR(100) NOT NULL,
    gender  VARCHAR(10)
);

CREATE TABLE student (
    id     INT PRIMARY KEY,
    name   VARCHAR(100) NOT NULL,
    email  VARCHAR(150) UNIQUE NOT NULL
);

CREATE TABLE course (
    id             INT PRIMARY KEY,
    c_name         VARCHAR(100) NOT NULL,
    instructor_id  INT,
    FOREIGN KEY (instructor_id) REFERENCES instructor(id)
);

-- Junction table resolving the student <-> course many-to-many relationship
CREATE TABLE enrollment (
    student_id   INT,
    course_id    INT,
    enrolled_on  DATE,
    PRIMARY KEY (student_id, course_id),          -- composite key
    FOREIGN KEY (student_id) REFERENCES student(id),
    FOREIGN KEY (course_id)  REFERENCES course(id)
);
```

---

## What is anomalies?

Anomalies in databases refer to inconsistencies or unexpected issues that can occur during data manipulation or retrieval.

There are three main types of anomalies:
- Update Anomalies
- Delete Anomalies
- Insert Anomalies

## What is functional dependency?

Functional dependency in simple terms means that the value of one attribute (or set of attributes) uniquely determines the value of another attribute(s) in a table.

## What is normal forms?

A set of rules applied to a database table to reduce redundancy and avoid anomalies in data by organizing it properly.

There is 4 kind of normal forms:
- 0NF
- 1NF
- 2NF
- 3NF

## Rules of 1NF

- Atomic Values
- Unique Column Names
- Positional dependency of data
- Column should contain data that are of the same type
- Determine Primary key

## Rules of 2NF

- Must be in 1NF
- No non-key attribute should depend on part of a candidate key

## Rules of 3NF

- Must be in 2NF
- Must not contain transitive dependency

## What is transitive dependency?

In a table if tow of abreast column that are non-key attribute and have functional dependency then it's a transitive dependency.

## What is SQL

SQL stands for Structured Query Language. The language we use to talk with databases.