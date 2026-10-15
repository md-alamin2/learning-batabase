# 📚 Learning Database

A beginner-friendly guide to the core concepts of databases: data, information, keys, normalization, the SDLC, database design, and SQL with PostgreSQL.

## 📑 Table of Contents

1. [Database Basics](#-database-basics)
2. [Keys in a Database](#-keys-in-a-database)
3. [Database Anomalies](#-database-anomalies)
4. [Functional Dependency](#-functional-dependency)
5. [Normal Forms](#-normal-forms)
6. [SDLC (Software Development Life Cycle)](#-sdlc-software-development-life-cycle)
7. [System Design: Designing a Database](#-system-design-designing-a-database)
8. [Introduction to SQL](#-introduction-to-sql)
9. [SQL Data Types (PostgreSQL)](#-sql-data-types-postgresql)
10. [SQL Constraints (PostgreSQL)](#-sql-constraints-postgresql)
11. [PostgreSQL ALTER TABLE Commands](#-postgresql-alter-table-commands)
12. [PostgreSQL SELECT Command](#-postgresql-select-command)
13. [Advanced Data Retrieval](#-advanced-data-retrieval)
    - [NULL Filtering](#1-null-filtering)
    - [COALESCE Function](#2-coalesce-function)
    - [Pagination with LIMIT and OFFSET](#3-pagination-with-limit-and-offset)
14. [Modifying Data: UPDATE and DELETE](#-modifying-data-update-and-delete)
15. [Grouping Data: GROUP BY and HAVING](#-grouping-data-group-by-and-having)
16. [Working with Related Tables](#-working-with-related-tables)
17. [Joins](#-joins)

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

## ⚠️ Database Anomalies

**Anomalies** in databases refer to inconsistencies or unexpected issues that can occur during data manipulation or retrieval. They are the main reason we normalize tables.

There are three main types of anomalies:

| Anomaly | What goes wrong |
| --- | --- |
| **Update Anomaly** | Updating one fact requires changing many rows; missing one creates inconsistent data. |
| **Delete Anomaly** | Deleting a row accidentally removes unrelated information along with it. |
| **Insert Anomaly** | You cannot insert a fact because some other (unrelated) required data is missing. |

---

## 🔗 Functional Dependency

**Functional dependency**, in simple terms, means that the value of one attribute (or set of attributes) uniquely determines the value of another attribute (or set of attributes) in a table.

**Notation:** `X → Y` means "if we know the value of `X`, we know the value of `Y`."

**Example:** In a `student` table, `student_id → name` holds if each student ID maps to exactly one name.

---

## 🧮 Normal Forms

**Normal forms** are a set of rules applied to a database table to reduce redundancy and avoid anomalies by organizing data properly.

Each higher normal form builds on the previous one:

| Normal Form | Rule (summary) |
| --- | --- |
| **0NF** (Unnormalized) | Raw data; may contain repeating groups or multiple values in one cell. |
| **1NF** | Atomic values, unique column names, same data type per column, and a primary key. |
| **2NF** | Must be in 1NF, and no non-key attribute depends on part of a composite candidate key. |
| **3NF** | Must be in 2NF, and no non-key attribute depends on another non-key attribute. |

### Rules of 1NF

- **Atomic values** — each cell holds a single, indivisible value
- **Unique column names**
- **Positional independence of data** — the order of rows/columns must not carry meaning
- Each column should contain data of the **same type**
- Determine a **primary key**

### Rules of 2NF

- Must be in **1NF**
- **No non-key attribute** should depend on **part of a candidate key** (no partial dependency)

This rule only matters when a table has a **composite** primary key.

### Rules of 3NF

- Must be in **2NF**
- Must **not contain transitive dependencies**

### Transitive Dependency

A **transitive dependency** occurs in a table when two non-key attributes have a functional dependency between them — that is, a non-key attribute determines another non-key attribute.

**Example:** In a table with `student_id → department_id` and `department_id → department_name`, the dependency `student_id → department_name` is transitive and violates 3NF. The fix is to move `department_name` into a separate `department` table.

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

## 💬 Introduction to SQL

**SQL** stands for **Structured Query Language** — the language we use to talk with databases.

It is divided into categories:

| Category | Purpose | Example commands |
| --- | --- | --- |
| **DDL** (Data Definition Language) | Define and modify structure | `CREATE`, `ALTER`, `DROP` |
| **DML** (Data Manipulation Language) | Insert, update, delete data | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** (Data Query Language) | Retrieve data | `SELECT` |
| **DCL** (Data Control Language) | Control access | `GRANT`, `REVOKE` |

The following sections cover PostgreSQL in detail: data types, constraints, `ALTER TABLE`, and `SELECT`.

---

## 🔢 SQL Data Types (PostgreSQL)

### 📌 Introduction

SQL data types define what kind of data a column can store in a database table. Choosing the correct data type helps maintain data accuracy, data integrity, and efficient storage.

For example:
- `INTEGER` stores whole numbers.
- `VARCHAR` stores variable-length text.
- `BOOLEAN` stores `TRUE` or `FALSE`.
- `DATE` stores dates.

---

### 1. Numeric Data Types

Numeric data types are used to store numbers.

| Data Type | Description | Example |
|---|---|---|
| `SMALLINT` | Small-range whole numbers | `25` |
| `INTEGER` or `INT` | Whole numbers | `1000` |
| `BIGINT` | Large whole numbers | `9000000000` |
| `DECIMAL(p, s)` | Exact decimal values | `99.99` |
| `NUMERIC(p, s)` | Exact decimal values | `1234.56` |
| `REAL` | Single-precision floating-point numbers | `3.14` |
| `DOUBLE PRECISION` | Double-precision floating-point numbers | `3.1415926535` |
| `SMALLSERIAL` | Auto-incrementing small integer | `1, 2, 3` |
| `SERIAL` | Auto-incrementing integer | `1, 2, 3` |
| `BIGSERIAL` | Auto-incrementing large integer | `1, 2, 3` |

#### Example

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    price NUMERIC(10, 2),
    quantity INTEGER
);
```

**Note:** `NUMERIC(10, 2)` allows up to 10 digits in total, including 2 digits after the decimal point.

---

### 2. Character and String Data Types

These data types store text.

| Data Type | Description | Example |
|---|---|---|
| `CHAR(n)` | Fixed-length character string | `'ABC'` |
| `VARCHAR(n)` | Variable-length string with a maximum length | `'Alamin'` |
| `TEXT` | Variable-length text without a declared length limit | `'Hello World'` |

#### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    gender CHAR(1),
    bio TEXT
);
```

**Difference:**
- `CHAR(n)` is fixed-length and space-padded when necessary.
- `VARCHAR(n)` has a maximum length.
- `TEXT` is suitable for text without a specific declared length limit.

---

### 3. Boolean Data Type

The `BOOLEAN` data type stores logical values.

| Value | Meaning |
|---|---|
| `TRUE` | True |
| `FALSE` | False |
| `NULL` | Unknown or missing value |

#### Example

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50),
    is_active BOOLEAN DEFAULT TRUE
);
```

Insert data:

```sql
INSERT INTO users (username, is_active)
VALUES ('Alamin', TRUE);
```

---

### 4. Date and Time Data Types

These types store dates and times.

| Data Type | Description | Example |
|---|---|---|
| `DATE` | Date only | `'2026-10-10'` |
| `TIME` | Time without time zone | `'14:30:00'` |
| `TIME WITH TIME ZONE` | Time with time zone information | `'14:30:00+06'` |
| `TIMESTAMP` | Date and time without time zone | `'2026-10-10 14:30:00'` |
| `TIMESTAMPTZ` | Timestamp with time zone semantics | `'2026-10-10 14:30:00+06'` |
| `INTERVAL` | A duration of time | `'2 days'` |

#### Example

```sql
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    event_name VARCHAR(100),
    event_date DATE,
    start_time TIME,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

**Note:** PostgreSQL stores `TIMESTAMPTZ` values internally as instants in time and displays them according to the session time zone.

---

### 5. Binary Data Type

#### `BYTEA`

The `BYTEA` data type stores binary data as a sequence of bytes.

It can be used for:
- Binary files
- Image data
- Other raw binary content

#### Example

```sql
CREATE TABLE files (
    id SERIAL PRIMARY KEY,
    file_name VARCHAR(100),
    file_data BYTEA
);
```

For many applications, files are stored in external storage, while their URLs or paths are saved in the database.

---

### 6. JSON Data Types

PostgreSQL supports JSON data.

| Data Type | Description |
|---|---|
| `JSON` | Stores JSON text and preserves its original formatting and key order |
| `JSONB` | Stores JSON in a decomposed binary format, generally better for querying and indexing |

#### Example

```sql
CREATE TABLE user_profiles (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50),
    details JSONB
);
```

Insert data:

```sql
INSERT INTO user_profiles (username, details)
VALUES (
    'Alamin',
    '{"city": "Dhaka", "country": "Bangladesh"}'
);
```

Query JSON data:

```sql
SELECT details ->> 'city' AS city
FROM user_profiles;
```

---

### 7. UUID Data Type

`UUID` stands for Universally Unique Identifier. It stores a 128-bit identifier.

It is useful when unique IDs should not be simple sequential integers.

#### Example

```sql
CREATE TABLE customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL
);
```

PostgreSQL supports UUID generation through `gen_random_uuid()`.

---

### 8. Array Data Type

PostgreSQL allows columns to store arrays of values of the same element type.

#### Example

```sql
CREATE TABLE courses (
    id SERIAL PRIMARY KEY,
    course_name VARCHAR(100),
    tags TEXT[]
);
```

Insert data:

```sql
INSERT INTO courses (course_name, tags)
VALUES (
    'PostgreSQL Basics',
    ARRAY['SQL', 'Database', 'Backend']
);
```

Access an array element:

```sql
SELECT tags[1]
FROM courses;
```

**Note:** PostgreSQL arrays are one-based by default, so the first element is at index `1`.

---

### 9. Special Data Types

PostgreSQL also supports several specialized data types.

| Data Type | Description |
|---|---|
| `ENUM` | A predefined set of allowed values |
| `INET` | An IPv4 or IPv6 host address |
| `CIDR` | An IPv4 or IPv6 network address |
| `MACADDR` | A MAC address |
| `POINT` | A geometric point |
| `XML` | XML data |

#### Example: ENUM

```sql
CREATE TYPE order_status AS ENUM (
    'pending',
    'shipped',
    'delivered',
    'cancelled'
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    status order_status DEFAULT 'pending'
);
```

The `status` column accepts only the values defined in the `order_status` type.

---

### 10. SQL Data Types Quick Reference

| Requirement | Recommended Type |
|---|---|
| Whole numbers | `INTEGER` |
| Very large whole numbers | `BIGINT` |
| Money-like exact decimal calculations | `NUMERIC(p, s)` |
| Short text with a maximum length | `VARCHAR(n)` |
| Long text | `TEXT` |
| True/false values | `BOOLEAN` |
| Birth date | `DATE` |
| Event date and time | `TIMESTAMPTZ` |
| Unique identifier | `UUID` |
| JSON documents | `JSONB` |
| Arrays of values | `TEXT[]`, `INTEGER[]`, etc. |
| Binary data | `BYTEA` |

---

### 11. Complete Example

The following example combines several common PostgreSQL data types.

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    age SMALLINT CHECK (age >= 18),
    cgpa NUMERIC(3, 2),
    is_active BOOLEAN DEFAULT TRUE,
    birth_date DATE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    skills TEXT[]
);
```

Insert a student:

```sql
INSERT INTO students (
    username,
    email,
    age,
    cgpa,
    birth_date,
    skills
)
VALUES (
    'alamin',
    'alamin@example.com',
    22,
    3.75,
    '2004-01-15',
    ARRAY['HTML', 'CSS', 'JavaScript']
);
```

Retrieve the data:

```sql
SELECT *
FROM students;
```

---

### 12. Important Notes

- Choose data types according to the data you need to store.
- Use `NOT NULL` when a column must always contain a value.
- Use `UNIQUE` to prevent duplicate values.
- Use `CHECK` to enforce conditions on values.
- Use `DEFAULT` to provide a value when one is not supplied.
- Use `PRIMARY KEY` to uniquely identify each row.
- Use `SERIAL` for a convenient auto-incrementing integer in PostgreSQL; identity columns are the SQL-standard alternative.
- `NULL` represents an unknown or missing value, not zero or an empty string.
- PostgreSQL's available types and their behavior may differ from MySQL, SQL Server, and other database systems.

---

## 🛡️ SQL Constraints (PostgreSQL)

### 📌 Introduction

SQL constraints are rules applied to table columns or entire tables to ensure the accuracy, validity, and integrity of data stored in a database.

Constraints prevent invalid data from being inserted, updated, or maintained in a table.

For example:
- A username must be unique.
- An email address cannot be empty.
- A student's age must be at least 18.
- Every student must have a unique ID.

PostgreSQL supports several types of constraints to enforce these rules.

---

### 1. NOT NULL Constraint

The `NOT NULL` constraint ensures that a column cannot store a `NULL` value.

#### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100)
);
```

Here, `username` must contain a non-`NULL` value, but `email` may be `NULL`.

#### Invalid Example

```sql
INSERT INTO students (username, email)
VALUES (NULL, 'alamin@example.com');
```

**Result:** PostgreSQL rejects the insertion because `username` cannot be `NULL`.

**Remember:** `NULL` is different from an empty string (`''`) or zero (`0`).

---

### 2. UNIQUE Constraint

The `UNIQUE` constraint ensures that values in a column or combination of columns do not duplicate one another.

#### Example

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(100) UNIQUE
);
```

In this example:
- Every non-`NULL` username must be unique.
- Every non-`NULL` email must be unique.
- Multiple `NULL` values are allowed by default in PostgreSQL's `UNIQUE` constraint.

#### Invalid Example

```sql
INSERT INTO users (username, email)
VALUES ('alamin', 'alamin@example.com');

INSERT INTO users (username, email)
VALUES ('alamin', 'another@example.com');
```

**Result:** The second insertion fails because the username already exists.

**Remember:** Use `NOT NULL UNIQUE` when a value must be both present and unique.

---

### 3. PRIMARY KEY Constraint

The `PRIMARY KEY` constraint uniquely identifies every row in a table.

A primary key:
- Must contain unique values.
- Cannot contain `NULL`.
- Can consist of one column or multiple columns.
- Allows only one primary key constraint per table, although that key can contain multiple columns.

#### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

Here, `id` uniquely identifies each student.

#### Composite Primary Key Example

A composite primary key uses more than one column.

```sql
CREATE TABLE enrollments (
    student_id INTEGER,
    course_id INTEGER,
    PRIMARY KEY (student_id, course_id)
);
```

The combination of `student_id` and `course_id` must be unique.

For example, a student cannot have the same course enrollment recorded twice, but different students can enroll in the same course.

---

### 4. FOREIGN KEY Constraint

The `FOREIGN KEY` constraint maintains referential integrity between related tables.

It ensures that a referenced value exists in the related table, subject to the foreign key's rules.

#### Example

```sql
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INTEGER,
    FOREIGN KEY (department_id)
        REFERENCES departments(id)
);
```

Here, `department_id` references `id` in the `departments` table.

#### How It Works

Suppose the `departments` table contains:

| id | department_name |
|---:|---|
| 1 | IT |
| 2 | HR |

The following insertion succeeds:

```sql
INSERT INTO employees (employee_name, department_id)
VALUES ('Alamin', 1);
```

The following insertion fails if department `99` does not exist:

```sql
INSERT INTO employees (employee_name, department_id)
VALUES ('Rahim', 99);
```

A foreign key can also define what happens when a referenced row is updated or deleted.

Common actions include:

- `ON DELETE CASCADE` — deletes related rows automatically.
- `ON DELETE SET NULL` — sets the referencing column to `NULL`.
- `ON DELETE RESTRICT` — prevents deletion when referencing rows exist.
- `ON DELETE SET DEFAULT` — sets the referencing column to its default value.
- `ON DELETE NO ACTION` — checks the constraint according to its enforcement rules; this is the default.

Example:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id)
        ON DELETE CASCADE
);
```

With `ON DELETE CASCADE`, deleting a customer also deletes that customer's referencing orders. Use it only when this behavior is appropriate.

---

### 5. CHECK Constraint

The `CHECK` constraint ensures that a value satisfies a specified condition.

#### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    age SMALLINT CHECK (age >= 18),
    cgpa NUMERIC(3, 2) CHECK (cgpa >= 0 AND cgpa <= 4.00)
);
```

Here:
- `age` must be at least 18 when it is not `NULL`.
- `cgpa` must be between `0` and `4.00` when it is not `NULL`.

#### Invalid Example

```sql
INSERT INTO students (username, age, cgpa)
VALUES ('Alamin', 16, 3.75);
```

**Result:** The insertion fails because the age violates the check constraint.

**Important:** A `CHECK` constraint passes when its expression evaluates to `TRUE` or `NULL`. If the value is required, combine `CHECK` with `NOT NULL`.

---

### 6. DEFAULT Constraint

The `DEFAULT` constraint provides a value automatically when an `INSERT` statement omits that column or explicitly uses the `DEFAULT` keyword.

#### Example

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

Insert a user:

```sql
INSERT INTO users (username)
VALUES ('Alamin');
```

PostgreSQL automatically assigns:
- `TRUE` to `is_active`.
- The current timestamp to `created_at`.

**Remember:** A default does not automatically replace an explicitly supplied `NULL`. If the column also has `NOT NULL`, inserting `NULL` is rejected.

---

### 7. Combining Multiple Constraints

You can apply multiple constraints to the same column.

#### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    age SMALLINT NOT NULL CHECK (age >= 18),
    is_active BOOLEAN NOT NULL DEFAULT TRUE
);
```

This table enforces the following rules:

| Column | Constraint | Purpose |
|---|---|---|
| `id` | `PRIMARY KEY` | Uniquely identifies each student |
| `username` | `NOT NULL`, `UNIQUE` | Required and unique username |
| `email` | `NOT NULL`, `UNIQUE` | Required and unique email |
| `age` | `NOT NULL`, `CHECK` | Required age of at least 18 |
| `is_active` | `NOT NULL`, `DEFAULT` | Defaults to `TRUE` |

---

### 8. Column-Level vs. Table-Level Constraints

Constraints can be defined at the column level or the table level.

#### Column-Level Constraint

Defined directly after a column's data type.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    price NUMERIC(10, 2) CHECK (price >= 0)
);
```

#### Table-Level Constraint

Defined separately after the column declarations.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    price NUMERIC(10, 2),
    CONSTRAINT positive_price CHECK (price >= 0)
);
```

Table-level constraints are especially useful for composite keys and rules involving multiple columns.

#### Example: Composite UNIQUE Constraint

```sql
CREATE TABLE course_registrations (
    id SERIAL PRIMARY KEY,
    student_id INTEGER NOT NULL,
    course_id INTEGER NOT NULL,
    CONSTRAINT unique_student_course
        UNIQUE (student_id, course_id)
);
```

This prevents the same student from registering for the same course more than once.

---

### 9. Naming Constraints

You can assign names to constraints using the `CONSTRAINT` keyword.

```sql
CREATE TABLE accounts (
    id SERIAL PRIMARY KEY,
    email VARCHAR(100) NOT NULL,
    balance NUMERIC(10, 2),
    CONSTRAINT unique_account_email UNIQUE (email),
    CONSTRAINT non_negative_balance CHECK (balance >= 0)
);
```

Meaning:
- `unique_account_email` is the name of the unique constraint.
- `non_negative_balance` is the name of the check constraint.

Naming constraints makes it easier to identify, modify, and remove them later.

---

### 10. Adding and Removing Constraints

PostgreSQL allows you to modify constraints on an existing table.

#### Add a Constraint

```sql
ALTER TABLE students
ADD CONSTRAINT check_student_age
CHECK (age >= 18);
```

#### Add a Foreign Key

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_department
FOREIGN KEY (department_id)
REFERENCES departments(id);
```

#### Remove a Constraint

```sql
ALTER TABLE students
DROP CONSTRAINT check_student_age;
```

**Note:** PostgreSQL generally requires the constraint's name when dropping a constraint. The automatically generated name may differ from the name you expect, so check the actual name if necessary.

---

### 11. SQL Constraints Quick Reference

| Constraint | Purpose |
|---|---|
| `NOT NULL` | Prevents `NULL` values |
| `UNIQUE` | Prevents duplicate non-`NULL` values by default |
| `PRIMARY KEY` | Uniquely identifies each row and disallows `NULL` |
| `FOREIGN KEY` | Enforces relationships between tables |
| `CHECK` | Enforces a condition on values |
| `DEFAULT` | Supplies a default value when a value is omitted |

---

### 12. Practice Example

Try creating a `books` table with the following requirements:

1. `id` must be an auto-incrementing primary key.
2. `title` must not be `NULL`.
3. `isbn` must be unique and required.
4. `price` must not be negative.
5. `available` must default to `TRUE`.
6. `published_year` must be between 1450 and 2100 when provided.

#### Solution

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    isbn VARCHAR(20) NOT NULL UNIQUE,
    price NUMERIC(10, 2) CHECK (price >= 0),
    available BOOLEAN NOT NULL DEFAULT TRUE,
    published_year INTEGER
        CHECK (published_year BETWEEN 1450 AND 2100)
);
```

Try inserting valid and invalid records to see how PostgreSQL enforces these rules.

---

## 🔧 PostgreSQL ALTER TABLE Commands

### Introduction

In PostgreSQL, the `ALTER TABLE` command is used to modify the structure of an existing table. We can use it to rename tables, add or remove columns, rename columns, change data types, and add or remove constraints.

### 1. Create a Table

The `CREATE TABLE` command creates a new table in the database.

```sql
CREATE TABLE employe (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

**Explanation:**
- `id SERIAL PRIMARY KEY`: Creates an auto-incrementing ID and sets it as the primary key.
- `name VARCHAR(50)`: Stores a name with a maximum length of 50 characters.
- `age INT`: Stores an integer value representing age.

### 2. Rename a Table

The `RENAME TO` clause changes the name of an existing table.

```sql
ALTER TABLE employe
RENAME TO employee;
```

**Explanation:** Renames the `employe` table to `employee`.

### 3. Add a Column

The `ADD COLUMN` clause adds a new column to an existing table.

```sql
ALTER TABLE employee
ADD COLUMN email VARCHAR(50);
```

**Explanation:** Adds an `email` column that can store strings up to 50 characters long.

### 4. Drop a Column

The `DROP COLUMN` clause removes a column from a table.

```sql
ALTER TABLE employee
DROP COLUMN email;
```

**Explanation:** Removes the `email` column from the `employee` table.

**Note:** Dropping a column also removes the data stored in that column. This operation can be destructive.

### 5. Rename a Column

The `RENAME COLUMN` clause changes the name of an existing column.

```sql
ALTER TABLE employee
RENAME COLUMN name TO user_name;
```

**Explanation:** Renames the `name` column to `user_name`.

### 6. Modify a Column's Data Type

The `ALTER COLUMN ... TYPE` clause changes the data type or size of a column.

```sql
ALTER TABLE employee
ALTER COLUMN user_name TYPE VARCHAR(100);
```

**Explanation:** Changes the maximum length of the `user_name` column from 50 to 100 characters.

**Note:** PostgreSQL may require a conversion if the existing data is incompatible with the new data type.

### 7. Add a NOT NULL Constraint

The `SET NOT NULL` clause prevents a column from containing `NULL` values.

```sql
ALTER TABLE employee
ALTER COLUMN email
SET NOT NULL;
```

**Explanation:** Ensures that every employee record must have a non-null value in the `email` column.

**Important:** The command will fail if existing rows contain `NULL` values in the `email` column. Also, the column must exist before applying this constraint.

### 8. Drop a NOT NULL Constraint

The `DROP NOT NULL` clause allows a column to contain `NULL` values again.

```sql
ALTER TABLE employee
ALTER COLUMN email
DROP NOT NULL;
```

**Explanation:** Removes the `NOT NULL` restriction from the `email` column.

### 9. Add a UNIQUE Constraint

A `UNIQUE` constraint prevents duplicate values in a column or combination of columns.

```sql
ALTER TABLE employee
ADD CONSTRAINT unique_employee_email
UNIQUE (email);
```

**Explanation:**
- `ADD CONSTRAINT`: Adds a new constraint to the table.
- `unique_employee_email`: Names the constraint.
- `UNIQUE (email)`: Prevents duplicate non-null email values.

**Note:** PostgreSQL allows multiple `NULL` values under a standard `UNIQUE` constraint by default. Existing duplicate non-null email values must be resolved before this constraint can be added successfully.

### 10. Drop a UNIQUE Constraint

The `DROP CONSTRAINT` clause removes an existing table-level constraint.

```sql
ALTER TABLE employee
DROP CONSTRAINT unique_employee_email;
```

**Explanation:** Removes the unique constraint named `unique_employee_email`, allowing duplicate email values again, provided no other constraint prevents them.

### Summary

| Operation | Command |
|---|---|
| Create a table | `CREATE TABLE` |
| Rename a table | `RENAME TO` |
| Add a column | `ADD COLUMN` |
| Drop a column | `DROP COLUMN` |
| Rename a column | `RENAME COLUMN` |
| Modify a data type | `ALTER COLUMN ... TYPE` |
| Add NOT NULL | `SET NOT NULL` |
| Drop NOT NULL | `DROP NOT NULL` |
| Add a constraint | `ADD CONSTRAINT` |
| Drop a constraint | `DROP CONSTRAINT` |

### Important Notes

- `ALTER TABLE` modifies the structure of an existing table.
- `ADD CONSTRAINT` is used to add constraints such as `UNIQUE`, `CHECK`, and `FOREIGN KEY`.
- `SET NOT NULL` and `DROP NOT NULL` specifically modify a column's nullability.
- Before dropping columns or constraints, make sure they are not needed by your application.
- Execute the commands in the correct order because later commands may depend on earlier changes.

---

## 🔍 PostgreSQL SELECT Command

The `SELECT` command is used to retrieve data from one or more tables in a database. It allows us to choose specific columns, filter records, sort results, remove duplicates, and perform calculations using built-in functions.

### Table of Contents

- [1. Basic SELECT](#1-basic-select)
- [2. Column Aliases](#2-column-aliases)
- [3. Sorting Data](#3-sorting-data)
- [4. DISTINCT](#4-distinct)
- [5. Filtering Data with WHERE](#5-filtering-data-with-where)
- [6. AND Operator](#6-and-operator)
- [7. OR Operator](#7-or-operator)
- [8. Comparison Operators](#8-comparison-operators)
- [9. BETWEEN Operator](#9-between-operator)
- [10. IN Operator](#10-in-operator)
- [11. LIKE and ILIKE Operators](#11-like-and-ilike-operators)
- [12. NOT Operator](#12-not-operator)
- [13. Scalar Functions](#13-scalar-functions)
- [14. Aggregate Functions](#14-aggregate-functions)

---

### 1. Basic SELECT

The `SELECT` statement retrieves data from a table.

#### Select Specific Columns

Retrieve only the `first_name` and `email` columns from the `students` table.

```sql
SELECT
    first_name,
    email
FROM
    students;
```

#### Select All Columns

Use the asterisk (`*`) to retrieve all columns.

```sql
SELECT
    *
FROM
    students;
```

**Explanation:**
- `SELECT` specifies which columns to retrieve.
- `FROM` specifies the table.
- `*` represents all columns.

### 2. Column Aliases

Column aliases allow us to rename columns in the query results without changing the actual table structure.

```sql
SELECT
    first_name AS "First Name",
    age AS user_age
FROM
    students;
```

**Explanation:**
- `AS` assigns an alias to a column.
- Double quotes preserve spaces and capitalization in an alias, such as `"First Name"`.
- Aliases make query results easier to read.

### 3. Sorting Data

The `ORDER BY` clause sorts query results in ascending or descending order.

```sql
SELECT
    first_name,
    age,
    country
FROM
    students
ORDER BY
    student_id DESC;
```

**Sorting options:**

- `ASC`: Ascending order (default).
- `DESC`: Descending order.

Example:

```sql
-- Sort students from youngest to oldest
SELECT *
FROM students
ORDER BY age ASC;

-- Sort students from oldest to youngest
SELECT *
FROM students
ORDER BY age DESC;
```

### 4. DISTINCT

The `DISTINCT` keyword removes duplicate values from the result.

```sql
SELECT DISTINCT
    country
FROM
    students;
```

**Explanation:** If multiple students belong to the same country, that country appears only once in the result.

### 5. Filtering Data with WHERE

The `WHERE` clause filters records based on a specified condition.

#### Equal Operator (`=`)

Find students from India.

```sql
SELECT *
FROM students
WHERE country = 'India';
```

Find students with an A grade in Physics.

```sql
SELECT *
FROM students
WHERE grade = 'A'
  AND course = 'Physics';
```

Find students with the A+ blood group.

```sql
SELECT *
FROM students
WHERE blood_group = 'A+';
```

### 6. AND Operator

The `AND` operator returns records only when **all specified conditions are true**.

#### Find students from Bangladesh whose age is 20

```sql
SELECT *
FROM students
WHERE country = 'Bangladesh'
  AND age = 20;
```

#### Find students from Bangladesh or India whose age is 20

```sql
SELECT *
FROM students
WHERE
    (country = 'Bangladesh' OR country = 'India')
    AND age = 20;
```

**Explanation:** Parentheses group conditions and help control how logical operators are evaluated.

### 7. OR Operator

The `OR` operator returns records when **at least one condition is true**.

#### Find students from Bangladesh or India

```sql
SELECT *
FROM students
WHERE country = 'Bangladesh'
   OR country = 'India';
```

#### Find students with grade A or B in Mathematics or Physics

```sql
SELECT *
FROM students
WHERE
    (grade = 'A' OR grade = 'B')
    AND
    (course = 'Mathematics' OR course = 'Physics');
```

**Explanation:** The query matches students with either grade A or B and whose course is either Mathematics or Physics.

### 8. Comparison Operators

Comparison operators compare values in a condition.

| Operator | Meaning |
|---|---|
| `=` | Equal to |
| `<>` | Not equal to |
| `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

#### Find students aged 20 or older

```sql
SELECT *
FROM students
WHERE age >= 20;
```

#### Find students younger than 20

```sql
SELECT *
FROM students
WHERE age < 20;
```

### 9. BETWEEN Operator

The `BETWEEN` operator selects values within a specified range. Both boundary values are included.

#### Find students aged between 20 and 22

```sql
SELECT *
FROM students
WHERE age BETWEEN 20 AND 22;
```

This query includes students aged 20, 21, and 22.

### 10. IN Operator

The `IN` operator checks whether a value matches any value in a specified list.

#### Find students enrolled in Physics, Mathematics, or English

```sql
SELECT *
FROM students
WHERE course IN ('Physics', 'Mathematics', 'English');
```

This is a shorter alternative to writing multiple `OR` conditions.

### 11. LIKE and ILIKE Operators

The `LIKE` operator searches for a specified pattern. In PostgreSQL, `ILIKE` performs a case-insensitive pattern match.

#### Pattern matching wildcards

| Wildcard | Meaning |
|---|---|
| `%` | Matches zero or more characters |
| `_` | Matches exactly one character |

#### Find students whose first name starts with A

```sql
SELECT *
FROM students
WHERE first_name LIKE 'A%';
```

#### Find students whose email starts with A, regardless of case

```sql
SELECT *
FROM students
WHERE email ILIKE 'A%';
```

#### Find students whose last name ends with n

```sql
SELECT *
FROM students
WHERE last_name LIKE '%n';
```

**Explanation:**
- `'A%'` matches values starting with A.
- `'%n'` matches values ending with n.
- `ILIKE` matches uppercase and lowercase variations, such as `A` and `a`.

### 12. NOT Operator

The `NOT` operator reverses a condition.

#### Find students whose grade is neither A+ nor A

```sql
SELECT *
FROM students
WHERE grade NOT IN ('A+', 'A');
```

This is equivalent to:

```sql
SELECT *
FROM students
WHERE NOT grade IN ('A+', 'A');
```

**Note:** SQL conditions involving `NULL` do not evaluate to true or false in the usual way. Rows with a `NULL` grade will not be returned by this query.

---

### 13. Scalar Functions

Scalar functions operate on individual values and return a result for each row.

#### 13.1 UPPER()

Converts a string to uppercase.

```sql
SELECT
    UPPER(first_name),
    first_name
FROM students;
```

Example: `alamin` becomes `ALAMIN`.

#### 13.2 LOWER()

Converts a string to lowercase.

```sql
SELECT
    LOWER(first_name),
    first_name
FROM students;
```

Example: `ALAMIN` becomes `alamin`.

#### 13.3 CONCAT()

Combines two or more strings into a single string.

```sql
SELECT
    CONCAT(first_name, last_name) AS "Full Name"
FROM students;
```

For a space between the first and last names, use:

```sql
SELECT
    CONCAT(first_name, ' ', last_name) AS "Full Name"
FROM students;
```

#### 13.4 LENGTH()

Returns the number of bytes in a string.

```sql
SELECT
    LENGTH(first_name)
FROM students;
```

For ordinary ASCII names, the result corresponds to the number of characters. For multibyte text, the byte count can differ from the character count.

To count characters instead, use `CHAR_LENGTH()`:

```sql
SELECT
    CHAR_LENGTH(first_name)
FROM students;
```

#### Scalar functions summary

| Function | Purpose |
|---|---|
| `UPPER()` | Converts text to uppercase |
| `LOWER()` | Converts text to lowercase |
| `CONCAT()` | Combines strings |
| `LENGTH()` | Returns the byte length of a string |
| `CHAR_LENGTH()` | Returns the character length of a string |

---

### 14. Aggregate Functions

Aggregate functions calculate a single result from a set of rows.

#### 14.1 AVG()

Returns the average of numeric values.

```sql
SELECT
    AVG(age)
FROM students;
```

#### 14.2 MAX()

Returns the maximum value.

```sql
SELECT
    MAX(age)
FROM students;
```

#### 14.3 MIN()

Returns the minimum value.

```sql
SELECT
    MIN(age)
FROM students;
```

#### 14.4 SUM()

Returns the sum of numeric values.

```sql
SELECT
    SUM(age)
FROM students;
```

#### 14.5 COUNT()

Counts rows or non-NULL values, depending on the expression.

Count all rows:

```sql
SELECT
    COUNT(*)
FROM students;
```

Count students with a non-NULL email:

```sql
SELECT
    COUNT(email)
FROM students;
```

**Explanation:**
- `COUNT(*)` counts all rows.
- `COUNT(email)` counts only rows where `email` is not `NULL`.
- `AVG()`, `MAX()`, `MIN()`, and `SUM()` ignore `NULL` values.
- `SUM()` and `AVG()` require numeric input.

#### Aggregate functions summary

| Function | Purpose |
|---|---|
| `AVG()` | Calculates the average |
| `MAX()` | Finds the maximum value |
| `MIN()` | Finds the minimum value |
| `SUM()` | Calculates the total |
| `COUNT()` | Counts rows or non-NULL values |

#### Using Multiple Aggregate Functions

You can use multiple aggregate functions in one query.

```sql
SELECT
    COUNT(*) AS total_students,
    AVG(age) AS average_age,
    MAX(age) AS oldest_student,
    MIN(age) AS youngest_student,
    SUM(age) AS total_age
FROM students;
```

This query returns one row containing all five calculated results.

---

### Conclusion

The PostgreSQL `SELECT` command is essential for retrieving and analyzing database records. By combining `WHERE`, `ORDER BY`, `DISTINCT`, logical operators, pattern matching, scalar functions, and aggregate functions, you can write flexible queries to retrieve meaningful information from a database.

**Key takeaways:**

- Use `SELECT` to retrieve data.
- Use `WHERE` to filter records.
- Use `ORDER BY` to sort results.
- Use `DISTINCT` to remove duplicate results.
- Use `AND`, `OR`, and `NOT` to combine or reverse conditions.
- Use `LIKE` and `ILIKE` for pattern matching.
- Use scalar functions to manipulate individual values.
- Use aggregate functions to summarize data.

---

---

## 🚀 Advanced Data Retrieval

Building on the `SELECT` command, these techniques handle missing values, provide defaults, and retrieve results page by page.

### 1. NULL Filtering

In SQL, `NULL` represents a missing or unknown value. You cannot check for `NULL` using the normal equality operator (`=`).

#### Incorrect way

```sql
SELECT *
FROM students
WHERE email = NULL;
```

This does not correctly find rows with missing emails because comparisons with `NULL` evaluate to unknown.

#### Correct way: IS NULL

```sql
SELECT *
FROM students
WHERE email IS NULL;
```

This retrieves students whose email is missing.

#### IS NOT NULL

To retrieve students whose email is available:

```sql
SELECT *
FROM students
WHERE email IS NOT NULL;
```

**Key points:**
- `IS NULL` finds missing values.
- `IS NOT NULL` finds non-`NULL` values.
- Do not use `= NULL` or `!= NULL` to test for NULL.

### 2. COALESCE Function

The `COALESCE()` function returns the first non-`NULL` value from the supplied arguments.

#### Example 1: Basic usage

```sql
SELECT COALESCE(NULL, NULL, 2, 3);
```

**Result:** `2`

PostgreSQL checks the values from left to right and returns the first non-NULL value.

#### Example 2: Replace missing emails

```sql
SELECT
    COALESCE(email, 'Not provided') AS email
FROM students;
```

If a student's email is `NULL`, the query displays `Not provided` instead.

**Syntax:**

```sql
COALESCE(value1, value2, value3, ...)
```

All arguments must be convertible to a compatible data type.

**Common use cases:**
- Providing default values.
- Replacing missing text in query results.
- Handling `NULL`s in calculations and reports.

### 3. Pagination with LIMIT and OFFSET

Pagination retrieves a limited number of records at a time. It is useful when displaying database records across multiple pages in an application.

#### LIMIT

`LIMIT` specifies the maximum number of rows to return.

```sql
SELECT *
FROM students
WHERE blood_group = 'A+'
LIMIT 5;
```

This returns at most five students with blood group A+.

#### OFFSET

`OFFSET` skips a specified number of rows before returning results.

```sql
SELECT *
FROM students
WHERE grade = 'A'
LIMIT 5 OFFSET 2;
```

This skips the first two matching rows and returns up to five subsequent rows.

#### Pagination example

```sql
-- Page 1
SELECT *
FROM students
ORDER BY student_id
LIMIT 5 OFFSET 0;

-- Page 2
SELECT *
FROM students
ORDER BY student_id
LIMIT 5 OFFSET 5;

-- Page 3
SELECT *
FROM students
ORDER BY student_id
LIMIT 5 OFFSET 10;
```

**Pagination formula:**

```text
OFFSET = (page_number - 1) * page_size
```

For example, page 3 with a page size of 5 has an offset of 10.

**Important:** Use `ORDER BY` for predictable pagination. Without a defined order, PostgreSQL does not guarantee which rows appear on each page.

---

## ✏️ Modifying Data: UPDATE and DELETE

So far we have inserted (`INSERT`) and retrieved (`SELECT`) data. These commands modify or remove existing records. Both belong to **DML** (Data Manipulation Language).

### UPDATE

The `UPDATE` statement changes existing data.

```sql
UPDATE students
SET email = 'alamin@example.com'
WHERE student_id = 1;
```

This updates the email of the student whose `student_id` is 1.

Update multiple columns:

```sql
UPDATE students
SET
    age = 22,
    country = 'Bangladesh'
WHERE student_id = 1;
```

**Warning:** Without a `WHERE` clause, an `UPDATE` statement modifies **every row** in the table.

### DELETE

The `DELETE` statement removes records from a table.

```sql
DELETE FROM students
WHERE student_id = 1;
```

This deletes the student whose ID is 1.

**Warning:** Without a `WHERE` clause, `DELETE FROM students;` removes **all rows** from the table.

---

## 📊 Grouping Data: GROUP BY and HAVING

### GROUP BY

The `GROUP BY` clause groups rows that share the same value in one or more columns. It is often used with aggregate functions such as `COUNT()`, `AVG()`, `SUM()`, `MIN()`, and `MAX()` (see the [Aggregate Functions](#14-aggregate-functions) section).

#### Count students by country

```sql
SELECT
    country,
    COUNT(*) AS total_students
FROM students
GROUP BY country;
```

This returns the number of students in each country.

#### Count students by grade

```sql
SELECT
    grade,
    COUNT(*) AS total_students
FROM students
GROUP BY grade
ORDER BY grade ASC;
```

This groups students by grade and sorts the results alphabetically or according to the column's data type.

#### GROUP BY with multiple columns

```sql
SELECT
    country,
    grade,
    COUNT(*) AS total_students
FROM students
GROUP BY country, grade
ORDER BY country, grade;
```

This counts students for each country-and-grade combination.

### HAVING

The `HAVING` clause filters groups created by `GROUP BY`. In contrast, `WHERE` filters individual rows **before** grouping.

#### Show courses with more than four students

```sql
SELECT
    course,
    COUNT(*) AS students_with_course
FROM students
GROUP BY course
HAVING COUNT(*) > 4;
```

Only courses with more than four students appear in the result.

#### Find countries where the average student age is greater than 21

```sql
SELECT
    country,
    AVG(age) AS average_age
FROM students
GROUP BY country
HAVING AVG(age) > 21;
```

This calculates each country's average student age and returns only countries whose average exceeds 21.

#### WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters groups |
| Applied before grouping | Applied after grouping |
| Commonly filters column values | Commonly filters aggregate results |

Example using both:

```sql
SELECT
    country,
    COUNT(*) AS total_students
FROM students
WHERE age >= 18
GROUP BY country
HAVING COUNT(*) > 4;
```

This first selects students aged 18 or older, groups them by country, and then displays countries with more than four qualifying students.

---

## 🔗 Working with Related Tables

We designed relationships in the [System Design](#-system-design-designing-a-database) section. Now let's create them in SQL: tables can be connected using primary keys and foreign keys. A foreign key references a key in another table and helps maintain referential integrity.

### Creating Related Tables

#### Create the users table

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(25) NOT NULL
);
```

#### Create the posts table

```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    user_id INT REFERENCES users(id)
);
```

**Explanation:**
- `id` is the primary key of each table.
- `posts.user_id` is a foreign key referencing `users.id`.
- A user can have multiple posts.
- Since `user_id` is not declared `NOT NULL`, a post can exist without an associated user.
- PostgreSQL enforces the foreign key when a non-NULL `user_id` is supplied.

The relationship is **one-to-many**: one user can be associated with multiple posts.

### Inserting Related Data

#### Insert users

```sql
INSERT INTO users (username)
VALUES
    ('akash'),
    ('alamin'),
    ('shakib'),
    ('jahangir');
```

#### Insert a post without a user

```sql
INSERT INTO posts (title)
VALUES ('Enjoying my holiday');
```

Because `user_id` is optional, this post has a `NULL` user ID.

#### Insert posts associated with users

```sql
INSERT INTO posts (title, user_id)
VALUES
    ('My first post', 1),
    ('Learning PostgreSQL', 2),
    ('Weekend activities', 1);
```

Here, user 1 has two posts, while user 2 has one.

**Note:** These example IDs assume the inserted users received IDs 1–4. In a database with existing records, verify the actual IDs rather than assuming their values.

---

## 🤝 Joins

Joins combine rows from two or more tables based on a related column. Using the `users` and `posts` tables above, these are the standard join types in PostgreSQL.

### INNER JOIN

An `INNER JOIN` returns only rows with matching values in **both** tables.

```sql
SELECT
    posts.id,
    posts.title,
    users.username
FROM posts
INNER JOIN users
    ON posts.user_id = users.id;
```

You can also write `JOIN` instead of `INNER JOIN`.

**Explanation:** Posts without a matching user are excluded from the results.

Example result:

| id | title | username |
|---:|---|---|
| 2 | My first post | akash |
| 3 | Learning PostgreSQL | alamin |
| 4 | Weekend activities | akash |

The IDs above are illustrative.

### LEFT JOIN

A `LEFT JOIN` returns **all rows from the left table** and matching rows from the right table. If no match exists, the right-side columns contain `NULL`.

```sql
SELECT
    posts.id,
    posts.title,
    users.username
FROM posts
LEFT JOIN users
    ON posts.user_id = users.id;
```

**Explanation:** All posts appear, including posts without an associated user.

Use a left join when you need every record from the main table, even if related data is missing.

### RIGHT JOIN

A `RIGHT JOIN` returns **all rows from the right table** and matching rows from the left table.

```sql
SELECT
    posts.id,
    posts.title,
    users.username
FROM posts
RIGHT JOIN users
    ON posts.user_id = users.id;
```

**Explanation:** Every user appears, including users who have not created any posts. For users without posts, the post columns contain `NULL`.

A `RIGHT JOIN` can often be rewritten as a `LEFT JOIN` by reversing the order of the tables.

### FULL JOIN

A `FULL JOIN`, also called a `FULL OUTER JOIN`, returns all matching and non-matching rows from **both** tables.

```sql
SELECT
    posts.id,
    posts.title,
    users.username
FROM posts
FULL JOIN users
    ON posts.user_id = users.id;
```

**Explanation:**
- Matching posts and users appear together.
- Posts without matching users still appear.
- Users without posts still appear.
- Missing columns on either side contain `NULL`.

This join is useful when you want a complete view of both tables, including unmatched records.

### CROSS JOIN

A `CROSS JOIN` returns the **Cartesian product** of two tables. Every row from the first table is combined with every row from the second table.

```sql
SELECT
    users.username,
    posts.title
FROM users
CROSS JOIN posts;
```

If the `users` table contains 4 rows and the `posts` table contains 3 rows, the result contains:

```text
4 × 3 = 12 rows
```

**Important:** A cross join does not require an `ON` condition. Use it when every possible combination is required.

### NATURAL JOIN

A `NATURAL JOIN` automatically joins two tables using **all columns that have the same name** in both tables.

Example:

```sql
SELECT *
FROM table_a
NATURAL JOIN table_b;
```

Suppose both tables have a column named `department_id`. PostgreSQL uses that column to match rows automatically.

**Important considerations:**
- A natural join uses every identically named column, not just one.
- If there are no identically named columns, it behaves like a cross join.
- Schema changes can unintentionally change the join conditions.

For these reasons, explicit joins with `ON` are generally easier to understand and maintain.

```sql
SELECT *
FROM table_a
JOIN table_b
    ON table_a.department_id = table_b.department_id;
```

The explicit form makes the relationship clear and avoids relying on matching column names.

### Joins Quick Reference

| Join | Returns |
|---|---|
| `INNER JOIN` | Only matching rows from both tables |
| `LEFT JOIN` | All rows from the left table + matches from the right |
| `RIGHT JOIN` | All rows from the right table + matches from the left |
| `FULL JOIN` | All rows from both tables, matched where possible |
| `CROSS JOIN` | Every possible row combination |
| `NATURAL JOIN` | Automatically matches identically named columns |

---

## 🧾 Master Quick Reference

| Topic | Purpose |
|---|---|
| `IS NULL` | Finds missing values |
| `IS NOT NULL` | Finds non-`NULL` values |
| `COALESCE()` | Returns the first non-`NULL` argument |
| `LIMIT` | Restricts the number of returned rows |
| `OFFSET` | Skips rows before returning results |
| `UPDATE` | Modifies existing records |
| `DELETE` | Removes records |
| `GROUP BY` | Groups rows with matching values |
| `HAVING` | Filters grouped results |
| `INNER JOIN` | Returns matching rows from both tables |
| `LEFT JOIN` | Keeps all rows from the left table |
| `RIGHT JOIN` | Keeps all rows from the right table |
| `FULL JOIN` | Keeps matching and unmatched rows from both tables |
| `CROSS JOIN` | Returns every possible row combination |
| `NATURAL JOIN` | Automatically matches columns with identical names |

---

## 🎓 Conclusion

Starting from data, keys, and normalization, we moved through the SDLC and ER design, then into practical PostgreSQL: data types, constraints, `ALTER TABLE`, and `SELECT`. The advanced topics — NULL handling, pagination, `UPDATE`/`DELETE`, grouping, related tables, and joins — complete the picture. Understanding these concepts is essential when building backend APIs and working with relational databases.

## 📚 Resources

- [PostgreSQL Official Documentation — Data Types](https://www.postgresql.org/docs/current/datatype.html)
- [PostgreSQL Official Documentation — Numeric Types](https://www.postgresql.org/docs/current/datatype-numeric.html)
- [PostgreSQL Official Documentation — Character Types](https://www.postgresql.org/docs/current/datatype-character.html)
- [PostgreSQL Official Documentation — Date/Time Types](https://www.postgresql.org/docs/current/datatype-datetime.html)
- [PostgreSQL Official Documentation — Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
- [PostgreSQL Official Documentation — CREATE TABLE](https://www.postgresql.org/docs/current/sql-createtable.html)
- [PostgreSQL Official Documentation — ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html)

---

**Author:** MD. Al-amin

**Topics:** Databases · Keys · Normalization · SDLC · ER Design · SQL / PostgreSQL · Joins & Advanced Queries