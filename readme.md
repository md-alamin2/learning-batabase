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

# SQL Data Types (PostgreSQL)

## 📌 Introduction

SQL data types define what kind of data a column can store in a database table. Choosing the correct data type helps maintain data accuracy, data integrity, and efficient storage.

For example:
- `INTEGER` stores whole numbers.
- `VARCHAR` stores variable-length text.
- `BOOLEAN` stores `TRUE` or `FALSE`.
- `DATE` stores dates.

---

## 1. Numeric Data Types

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

### Example

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

## 2. Character and String Data Types

These data types store text.

| Data Type | Description | Example |
|---|---|---|
| `CHAR(n)` | Fixed-length character string | `'ABC'` |
| `VARCHAR(n)` | Variable-length string with a maximum length | `'Alamin'` |
| `TEXT` | Variable-length text without a declared length limit | `'Hello World'` |

### Example

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

## 3. Boolean Data Type

The `BOOLEAN` data type stores logical values.

| Value | Meaning |
|---|---|
| `TRUE` | True |
| `FALSE` | False |
| `NULL` | Unknown or missing value |

### Example

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

## 4. Date and Time Data Types

These types store dates and times.

| Data Type | Description | Example |
|---|---|---|
| `DATE` | Date only | `'2026-10-10'` |
| `TIME` | Time without time zone | `'14:30:00'` |
| `TIME WITH TIME ZONE` | Time with time zone information | `'14:30:00+06'` |
| `TIMESTAMP` | Date and time without time zone | `'2026-10-10 14:30:00'` |
| `TIMESTAMPTZ` | Timestamp with time zone semantics | `'2026-10-10 14:30:00+06'` |
| `INTERVAL` | A duration of time | `'2 days'` |

### Example

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

## 5. Binary Data Type

### `BYTEA`

The `BYTEA` data type stores binary data as a sequence of bytes.

It can be used for:
- Binary files
- Image data
- Other raw binary content

### Example

```sql
CREATE TABLE files (
    id SERIAL PRIMARY KEY,
    file_name VARCHAR(100),
    file_data BYTEA
);
```

For many applications, files are stored in external storage, while their URLs or paths are saved in the database.

---

## 6. JSON Data Types

PostgreSQL supports JSON data.

| Data Type | Description |
|---|---|
| `JSON` | Stores JSON text and preserves its original formatting and key order |
| `JSONB` | Stores JSON in a decomposed binary format, generally better for querying and indexing |

### Example

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

## 7. UUID Data Type

`UUID` stands for Universally Unique Identifier. It stores a 128-bit identifier.

It is useful when unique IDs should not be simple sequential integers.

### Example

```sql
CREATE TABLE customers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL
);
```

PostgreSQL supports UUID generation through `gen_random_uuid()`.

---

## 8. Array Data Type

PostgreSQL allows columns to store arrays of values of the same element type.

### Example

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

## 9. Special Data Types

PostgreSQL also supports several specialized data types.

| Data Type | Description |
|---|---|
| `ENUM` | A predefined set of allowed values |
| `INET` | An IPv4 or IPv6 host address |
| `CIDR` | An IPv4 or IPv6 network address |
| `MACADDR` | A MAC address |
| `POINT` | A geometric point |
| `XML` | XML data |

### Example: ENUM

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

## 10. SQL Data Types Quick Reference

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

## 11. Complete Example

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

## 12. Important Notes

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

## 📚 Resources

- [PostgreSQL Official Documentation — Data Types](https://www.postgresql.org/docs/current/datatype.html)
- [PostgreSQL Official Documentation — Numeric Types](https://www.postgresql.org/docs/current/datatype-numeric.html)
- [PostgreSQL Official Documentation — Character Types](https://www.postgresql.org/docs/current/datatype-character.html)
- [PostgreSQL Official Documentation — Date/Time Types](https://www.postgresql.org/docs/current/datatype-datetime.html)

---

**Author:** MD. Al-amin

**Topic:** SQL / PostgreSQL Data Types

# SQL Constraints (PostgreSQL)

## 📌 Introduction

SQL constraints are rules applied to table columns or entire tables to ensure the accuracy, validity, and integrity of data stored in a database.

Constraints prevent invalid data from being inserted, updated, or maintained in a table.

For example:
- A username must be unique.
- An email address cannot be empty.
- A student's age must be at least 18.
- Every student must have a unique ID.

PostgreSQL supports several types of constraints to enforce these rules.

---

## 1. NOT NULL Constraint

The `NOT NULL` constraint ensures that a column cannot store a `NULL` value.

### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100)
);
```

Here, `username` must contain a non-`NULL` value, but `email` may be `NULL`.

### Invalid Example

```sql
INSERT INTO students (username, email)
VALUES (NULL, 'alamin@example.com');
```

**Result:** PostgreSQL rejects the insertion because `username` cannot be `NULL`.

**Remember:** `NULL` is different from an empty string (`''`) or zero (`0`).

---

## 2. UNIQUE Constraint

The `UNIQUE` constraint ensures that values in a column or combination of columns do not duplicate one another.

### Example

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

### Invalid Example

```sql
INSERT INTO users (username, email)
VALUES ('alamin', 'alamin@example.com');

INSERT INTO users (username, email)
VALUES ('alamin', 'another@example.com');
```

**Result:** The second insertion fails because the username already exists.

**Remember:** Use `NOT NULL UNIQUE` when a value must be both present and unique.

---

## 3. PRIMARY KEY Constraint

The `PRIMARY KEY` constraint uniquely identifies every row in a table.

A primary key:
- Must contain unique values.
- Cannot contain `NULL`.
- Can consist of one column or multiple columns.
- Allows only one primary key constraint per table, although that key can contain multiple columns.

### Example

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

Here, `id` uniquely identifies each student.

### Composite Primary Key Example

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

## 4. FOREIGN KEY Constraint

The `FOREIGN KEY` constraint maintains referential integrity between related tables.

It ensures that a referenced value exists in the related table, subject to the foreign key's rules.

### Example

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

### How It Works

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

## 5. CHECK Constraint

The `CHECK` constraint ensures that a value satisfies a specified condition.

### Example

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

### Invalid Example

```sql
INSERT INTO students (username, age, cgpa)
VALUES ('Alamin', 16, 3.75);
```

**Result:** The insertion fails because the age violates the check constraint.

**Important:** A `CHECK` constraint passes when its expression evaluates to `TRUE` or `NULL`. If the value is required, combine `CHECK` with `NOT NULL`.

---

## 6. DEFAULT Constraint

The `DEFAULT` constraint provides a value automatically when an `INSERT` statement omits that column or explicitly uses the `DEFAULT` keyword.

### Example

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

## 7. Combining Multiple Constraints

You can apply multiple constraints to the same column.

### Example

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

## 8. Column-Level vs. Table-Level Constraints

Constraints can be defined at the column level or the table level.

### Column-Level Constraint

Defined directly after a column's data type.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    price NUMERIC(10, 2) CHECK (price >= 0)
);
```

### Table-Level Constraint

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

### Example: Composite UNIQUE Constraint

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

## 9. Naming Constraints

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

## 10. Adding and Removing Constraints

PostgreSQL allows you to modify constraints on an existing table.

### Add a Constraint

```sql
ALTER TABLE students
ADD CONSTRAINT check_student_age
CHECK (age >= 18);
```

### Add a Foreign Key

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_department
FOREIGN KEY (department_id)
REFERENCES departments(id);
```

### Remove a Constraint

```sql
ALTER TABLE students
DROP CONSTRAINT check_student_age;
```

**Note:** PostgreSQL generally requires the constraint's name when dropping a constraint. The automatically generated name may differ from the name you expect, so check the actual name if necessary.

---

## 11. SQL Constraints Quick Reference

| Constraint | Purpose |
|---|---|
| `NOT NULL` | Prevents `NULL` values |
| `UNIQUE` | Prevents duplicate non-`NULL` values by default |
| `PRIMARY KEY` | Uniquely identifies each row and disallows `NULL` |
| `FOREIGN KEY` | Enforces relationships between tables |
| `CHECK` | Enforces a condition on values |
| `DEFAULT` | Supplies a default value when a value is omitted |

---

## 12. Practice Example

Try creating a `books` table with the following requirements:

1. `id` must be an auto-incrementing primary key.
2. `title` must not be `NULL`.
3. `isbn` must be unique and required.
4. `price` must not be negative.
5. `available` must default to `TRUE`.
6. `published_year` must be between 1450 and 2100 when provided.

### Solution

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

## 📚 Resources

- [PostgreSQL Official Documentation — Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
- [PostgreSQL Official Documentation — CREATE TABLE](https://www.postgresql.org/docs/current/sql-createtable.html)
- [PostgreSQL Official Documentation — ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html)

---

**Author:** MD. Al-amin

**Topic:** SQL / PostgreSQL Constraints
