# MODULE: RELATIONAL MODEL, ER & EER MODELING
## Comprehensive Expert-Level Notes

---

# TABLE OF CONTENTS

1. [Relational Model Fundamentals](#1-relational-model-fundamentals)
2. [Keys in Relational Model](#2-keys-in-relational-model)
3. [Integrity Constraints](#3-integrity-constraints)
4. [Handling of NULLs](#4-handling-of-nulls)
5. [Entity-Relationship Model Basics](#5-entity-relationship-model-basics)
6. [Attributes: Types & Properties](#6-attributes-types--properties)
7. [Relationships in ER Model](#7-relationships-in-er-model)
8. [Structural Constraints](#8-structural-constraints)
9. [Weak Entities](#9-weak-entities)
10. [Mapping ER to Relational Schema](#10-mapping-er-to-relational-schema)
11. [Advanced ER Mapping Scenarios](#11-advanced-er-mapping-scenarios)
12. [Extended ER Model (EER)](#12-extended-er-model-eer)
13. [Mapping EER to Relational Schema](#13-mapping-eer-to-relational-schema)
14. [Database-Level Constraints & Triggers](#14-database-level-constraints--triggers)
15. [Update Operations & Constraint Violations](#15-update-operations--constraint-violations)

---

# 1. RELATIONAL MODEL FUNDAMENTALS

## 1.1 What is the Relational Model?

The **relational model** is a **logical representation** of data organized into tables (relations). Proposed by **Dr. E.F. Codd (IBM)** in 1970, it revolutionized database management.

**Key Characteristics:**
- Data stored in tables (two-dimensional relations)
- Each row represents an entity/fact
- Each column represents an attribute
- Simple, flexible, standardized
- Easy sorting and access
- Foundation of RDBMS (Relational Database Management System)

### Informal Analogy
Think of a relation as a spreadsheet:
- **Row** = one instance/record (e.g., a student)
- **Column** = attribute/property (e.g., name, age)
- **Cell** = specific value

## 1.2 Fundamental Concepts

### Relation (Table)
A two-dimensional collection of data organized in rows and columns. Each relation has:
- **Name**: identifies the table
- **Attributes**: column names describing properties
- **Tuples**: rows containing data
- **Schema**: formal structure definition

**Relational Schema Notation:**
```
RELATION_NAME(Attribute1 Type1, Attribute2 Type2, ..., AttributeN TypeN)

Example:
STUDENT(RollNo INT, Name VARCHAR(30), CGPA FLOAT, DOB DATE)
```

### Tuple (Row)
An ordered set of values representing one entity instance. A tuple is atomic (indivisible).

Example tuple from STUDENT:
```
(101, "Alice Johnson", 3.85, "2004-05-15")
```

### Attribute (Column)
A named property of an entity type. Each attribute has:
- **Name**: role it plays (e.g., StudentName)
- **Domain**: set of valid values (type, range, format)

**Example domains:**
- StudentID: 5-digit positive integers
- Name: VARCHAR(50) – character strings up to 50 chars
- Salary: DECIMAL(10,2) – monetary values
- Status: {Active, Inactive, On-Leave}

### Domain
Logical definition + data type of valid values for an attribute.

**Example:**
- Domain of PhoneNumber: 10-digit numeric strings (format: ddd-ddd-dddd)
- Domain of GPA: Real numbers between 0.0 and 4.0
- Domain of Gender: {M, F, Others}

### Degree
Total number of attributes in a relation.

**Example:** STUDENT(RollNo, Name, CGPA) has **degree = 3**

### Cardinality
Number of tuples (rows) currently in a relation. Changes as data is inserted/deleted.

**Example:** If STUDENT table has 150 students, **cardinality = 150**

### Relational Instance (State)
The actual data (current set of rows) in a relation at a given point in time.

### Relational Database Schema
Complete design blueprint of all relations in a database:

```
DATABASE_NAME = {R1, R2, ..., Rn} + Integrity Constraints (IC)

Example:
COMPANY_DB = {EMPLOYEE, DEPARTMENT, PROJECT, WORKS_ON} + {constraints}
```

### Relational Database State (Instance)
Union of all individual relation states satisfying all integrity constraints.

---

# 2. KEYS IN RELATIONAL MODEL

## 2.1 Purpose of Keys

Keys uniquely identify tuples in a relation and maintain relationships between tables. They are **essential** for:
- Preventing duplicate data
- Establishing relationships between tables
- Enabling efficient searching

## 2.2 Types of Keys

### Super Key (SK)
**Definition:** Any set of attributes that can uniquely identify a tuple.

**Properties:**
- May contain extra (unnecessary) attributes
- Every key is a super key, but not vice versa

**Example:**
```
EMPLOYEE(EmpID, SSN, LicenseNo, Name, Salary, DeptNo)

Super keys:
- {EmpID}
- {SSN}
- {LicenseNo}
- {EmpID, Name}  ← contains extra attribute Name
- {SSN, DeptNo}
- {EmpID, SSN, LicenseNo, Name}
- Any set containing a candidate key
```

⭐ **Key Point:** A super key has the uniqueness property but may not be minimal.

### Candidate Key (CK)
**Definition:** A **minimal** super key—a super key with no redundant attributes.

**Properties:**
- No proper subset is a super key
- Multiple candidate keys can exist
- Every relation has at least one candidate key
- All candidate keys have equal strength

**Example:**
```
EMPLOYEE table:
- CK1: {EmpID} ✓ minimal
- CK2: {SSN} ✓ minimal
- CK3: {LicenseNo} ✓ minimal
- {EmpID, Name} ✗ not a candidate key (EmpID alone is minimal)
```

**Question:** Why is {EmpID, Name} not a candidate key?
**Answer:** Because {EmpID} alone can uniquely identify a tuple; Name is redundant.

### Primary Key (PK)
**Definition:** The candidate key **chosen by the database designer** to uniquely identify tuples operationally.

**Characteristics:**
- **Uniqueness**: No two tuples have the same PK value
- **Entity Integrity**: Cannot contain NULL values
- **Single choice**: Only one PK per relation
- **Operational use**: Used for indexing, referencing, tuple identity
- **Notation**: Underlined in schema diagram

**Example:**
```
EMPLOYEE(EmpID, SSN, LicenseNo, Name, Salary)
         ^^^^^^
Underline indicates PK choice
```

### Alternate Key (AK)
**Definition:** Any candidate key **not chosen as the primary key**.

**Characteristics:**
- Has the same strength as primary key
- May not exist if only one candidate key
- Often enforced with UNIQUE constraint

**Example:**
```
EMPLOYEE(EmpID PK, SSN AK, LicenseNo AK, Name, Salary)
          ^^^^^^         ^^^           ^^^
           PK          Alternate Keys
```

### Composite Key (CK) / Concatenated Key
**Definition:** A key consisting of **two or more attributes**.

**Use Cases:**
- Relationships don't naturally reduce to single attribute
- Representing many-to-many relationships
- Weak entities

**Example:**
```
EMPLOYEE_PROJECT(EmpID, ProjectID, RoleID, DateAssigned)
                  ^^^^^^  ^^^^^^^^^^
                  Composite Primary Key

Valid composite PKs:
- {EmpID, ProjectID}  ← identifies one assignment
- {EmpID, ProjectID, RoleID}
```

⚠️ **Important:** If an employee works on same project in different roles, need all three attributes.

### Artificial (Surrogate) Key
**Definition:** System-generated key created when natural/composite keys are **large, complex, or problematic**.

**Characteristics:**
- Not derived from real-world attributes
- Usually auto-increment integer
- No semantic meaning
- Improves performance

**Example:**
```
❌ Without surrogate key:
EMPLOYEE_PROJECT(EmpID, ProjectID, RoleID, StartDate, EndDate, ...)
                  ^^^^^^  ^^^^^^^^^^  ^^^^^^
                  Large composite key

✓ With surrogate key:
EMPLOYEE_PROJECT(AssignmentID, EmpID, ProjectID, RoleID, StartDate, ...)
                  ^^^^^^^^^^^^^  ^^^^^^  ^^^^^^^^^^  ^^^^^^
                  Surrogate PK   Foreign Keys
```

### Foreign Key (FK)
**Definition:** Attribute(s) in one relation that **reference the primary key** of another relation.

**Purpose:**
- Represent relationships between entities
- Maintain referential integrity
- Link tables together

**Notation:** FK attributes shown with arrow pointing to referenced PK.

**Example:**
```
EMPLOYEE(EmpID PK, Name, Salary, DeptID FK)
                                  ^^^^^^↓
DEPARTMENT(DeptID PK, DeptName, Location)
           ^^^^^^↑
```

**Rules:**
- FK value must exist in referenced PK or be NULL (no orphan records)
- FK can be NULL if relationship is optional
- FK can be composite (multiple attributes)
- One tuple can have multiple FKs

---

## 2.3 Key Selection Guidelines

When choosing a Primary Key from candidate keys, prefer:

1. **Smallest in size** (fewer bytes per row → faster comparisons, less storage)
2. **Stable values** (don't change frequently)
3. **Simple attribute** (avoid composite if possible)
4. **Meaningful to domain** (for usability, though not required)

**Example:**
```
PERSON(PersonID, SSN, PassportNo, DrivingLicense)

Candidate Keys:
- {PersonID} - 4 bytes, unique, stable → ✓ BEST CHOICE
- {SSN} - 11 bytes (with format), stable
- {PassportNo} - variable bytes
- {DrivingLicense} - variable bytes, may change
```

---

# 3. INTEGRITY CONSTRAINTS

## 3.1 Classification of Constraints

```
DATABASE CONSTRAINTS
│
├── Implicit / Inherent
│   └─ Based on data model itself
│
├── Explicit / Schema-Based
│   ├─ Domain Constraint
│   ├─ Key Constraint
│   ├─ Entity Integrity Constraint
│   └─ Referential Integrity Constraint
│
└── Application / Semantic
    └─ Beyond database model; requires application logic
```

## 3.2 Domain Constraint

**Definition:** Restricts attribute values to a **defined set of valid values** (domain).

**Specification:**
- Data type (INT, VARCHAR, DATE, etc.)
- Size/length constraints
- Range constraints
- Format constraints
- Allowed value set

**Examples:**
```
STUDENT(
  RollNo INT NOT NULL,           ← Integer type, no nulls
  Name VARCHAR(50) NOT NULL,     ← Max 50 characters
  Age INT CHECK (Age >= 18),     ← Range constraint
  Status VARCHAR(20) DEFAULT 'Active',  ← Only specific values
  Gender CHAR(1) CHECK (Gender IN ('M','F','O'))  ← Enumeration
)
```

**Violation Scenario:**
```
INSERT INTO STUDENT VALUES (101, "Alice", 15, 'Active', 'M');
← ERROR: Age 15 violates constraint Age >= 18
```

## 3.3 Key Constraint (Uniqueness)

**Definition:** All values in a **key attribute** must be unique; no duplicates allowed.

**Application:**
- Primary key values
- Candidate key values (via UNIQUE constraint)
- Composite keys (combination must be unique)

**SQL Syntax:**
```sql
CREATE TABLE EMPLOYEE (
  EmpID INT PRIMARY KEY,           ← Unique + Not Null
  SSN CHAR(11) UNIQUE,             ← Unique (can be NULL)
  Name VARCHAR(50),
  UNIQUE(FirstName, LastName)      ← Composite unique
);
```

**Violation Scenario:**
```
INSERT INTO EMPLOYEE VALUES (101, '123-45-6789', 'Alice');
INSERT INTO EMPLOYEE VALUES (101, '987-65-4321', 'Bob');
← ERROR: Duplicate EmpID violates primary key
```

## 3.4 Entity Integrity Constraint

**Definition:** **Primary key attributes cannot contain NULL values**.

**Why?**
- PK is used to identify individual tuples uniquely
- NULL means "value unknown" → cannot identify tuple
- Violates the fundamental purpose of a primary key

**Formal Rule:**
```
For any relation R and any tuple t in R:
t[PK] ≠ NULL (for all attributes in PK)

If PK is composite (A, B, C):
- t[A] ≠ NULL AND t[B] ≠ NULL AND t[C] ≠ NULL
```

**SQL Enforcement:**
```sql
CREATE TABLE STUDENT (
  RollNo INT NOT NULL PRIMARY KEY,  ← NOT NULL is automatic with PK
  Name VARCHAR(50) NOT NULL,
  Email VARCHAR(100)
);

INSERT INTO STUDENT VALUES (NULL, 'Alice', 'alice@mail.com');
← ERROR: Cannot insert NULL into primary key column RollNo
```

**Important Note:**
- Other attributes CAN contain NULL values (unless separately constrained)
- Only PK attributes are protected by entity integrity

```sql
CREATE TABLE STUDENT (
  RollNo INT NOT NULL PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  Email VARCHAR(100),  ← Can be NULL (optional)
  Age INT              ← Can be NULL (unknown)
);
```

## 3.5 Referential Integrity Constraint

**Definition:** Maintains consistency between **related tables**; every foreign key value must match a primary key value or be NULL.

**Formal Rule:**
```
If attribute FK in relation R1 references primary key PK in relation R2:

For any tuple t1 in R1:
  t1[FK] = NULL OR t1[FK] matches some t2[PK] in R2

(No "orphan" records allowed)
```

**Relationship Terminology:**
- **Referencing Relation (Child Table):** Contains the FK
- **Referenced Relation (Parent Table):** Contains the PK

**Visual Example:**
```
DEPARTMENT (Parent)          EMPLOYEE (Child)
+--------+----------+        +-------+-------+-------+
| DeptID | DeptName |        | EmpID | Name  | DeptID|
+--------+----------+        +-------+-------+-------+
| D1     | HR       |        | E1    | Alice | D1 ✓  |
| D2     | IT       |        | E2    | Bob   | D2 ✓  |
| D3     | Finance  |        | E3    | Carol | D1 ✓  |
+--------+----------+        | E4    | Dave  | D5 ✗ ERROR
                             +-------+-------+-------+
                              DeptID D5 doesn't exist
                              in DEPARTMENT table!
```

**SQL Syntax:**
```sql
CREATE TABLE DEPARTMENT (
  DeptID INT PRIMARY KEY,
  DeptName VARCHAR(50)
);

CREATE TABLE EMPLOYEE (
  EmpID INT PRIMARY KEY,
  Name VARCHAR(50),
  DeptID INT,
  FOREIGN KEY (DeptID) REFERENCES DEPARTMENT(DeptID)
);
```

**Violation Scenarios:**

```
✓ Valid Insertions:
INSERT INTO EMPLOYEE VALUES (1, 'Alice', 1);    ← DeptID 1 exists
INSERT INTO EMPLOYEE VALUES (2, 'Bob', NULL);   ← NULL is allowed

✗ Invalid Insertions:
INSERT INTO EMPLOYEE VALUES (3, 'Carol', 99);
← ERROR: No Department with DeptID=99 exists
```

**Cascading Actions for Referential Integrity:**

When a parent record is deleted or modified, DBMS can take actions:

| Action | Description | Example |
|--------|-------------|---------|
| **RESTRICT/NO ACTION** | Reject delete/update if child exists | Can't delete department if employees work there |
| **CASCADE** | Propagate changes to child records | Delete dept → all employees get DeptID=NULL or deleted |
| **SET NULL** | Set FK to NULL in child records | Delete dept → employee records still exist but DeptID=NULL |
| **SET DEFAULT** | Set FK to default value | Delete dept → employee records get predefined DeptID |

**SQL Syntax:**
```sql
CREATE TABLE EMPLOYEE (
  EmpID INT PRIMARY KEY,
  Name VARCHAR(50),
  DeptID INT,
  FOREIGN KEY (DeptID) REFERENCES DEPARTMENT(DeptID)
    ON DELETE CASCADE
    ON UPDATE RESTRICT
);
```

---

## 3.6 Summary of Integrity Constraints

| Constraint | Scope | Rule | Example |
|-----------|-------|------|---------|
| **Domain** | Single attribute | Values must be in domain | Age must be 0-150 |
| **Key** | Single relation | Key values unique | SSN unique in PERSON |
| **Entity Integrity** | Single relation | PK ≠ NULL | EmpID required |
| **Referential Integrity** | Two relations | FK = PK or NULL | DeptID in EMPLOYEE must exist in DEPARTMENT |

---

# 4. HANDLING OF NULLs

## 4.1 Understanding NULL

**NULL** is a special marker representing:
- **Value unknown**: "Date of Birth exists but we don't know it yet"
- **Value not available**: "Salary exists but is confidential"
- **Attribute not applicable**: "Mobile number not applicable for corporate account"

⚠️ **Critical Distinction:**
```
NULL ≠ 0
NULL ≠ "" (empty string)
NULL ≠ False
NULL ≠ Any specific value
```

## 4.2 NULL in SQL Operations

### Comparison with NULL

**Key Rule:** Comparisons with NULL always result in **UNKNOWN** (neither TRUE nor FALSE).

```sql
emp_table:
+-------+-------+
| EmpID | Comm  |
+-------+-------+
| 1     | 500   |
| 2     | NULL  |
| 3     | 200   |
+-------+-------+

Query: SELECT * FROM emp WHERE Comm = 500;
Result: EmpID=1 only ✓

Query: SELECT * FROM emp WHERE Comm = NULL;
Result: (empty) ✗ WRONG! This never finds NULL values!

Query: SELECT * FROM emp WHERE Comm IS NULL;
Result: EmpID=2 ✓ CORRECT way to find NULLs
```

### Logical Operations with NULL

```
NULL AND TRUE  = UNKNOWN
NULL AND FALSE = FALSE
NULL OR TRUE   = TRUE
NULL OR FALSE  = UNKNOWN
NOT NULL       = UNKNOWN
```

## 4.3 SQL Operators for NULLs

### IS NULL
Returns TRUE if value is NULL.

```sql
SELECT * FROM emp WHERE comm IS NULL;
```

### IS NOT NULL
Returns TRUE if value is NOT NULL.

```sql
SELECT * FROM emp WHERE comm IS NOT NULL;
```

## 4.4 NVL / COALESCE Functions

**Purpose:** Replace NULL with a specified value **in query results only** (doesn't change table data).

**Syntax:**
```sql
NVL(expression, replacement_value)
COALESCE(expr1, expr2, expr3, ...)
```

**Important:** Data types of expression and replacement must match.

**Examples:**

```sql
emp_table:
+-------+-------+-------+
| EmpID | Name  | Comm  |
+-------+-------+-------+
| 1     | Alice | 500   |
| 2     | Bob   | NULL  |
| 3     | Carol | 200   |
+-------+-------+-------+

Query 1: SELECT Name, NVL(Comm, 0) AS Commission FROM emp;
Result:
+-------+------------+
| Name  | Commission |
+-------+------------+
| Alice | 500        |
| Bob   | 0          | ← NULL replaced with 0
| Carol | 200        |
+-------+------------+

Query 2: SELECT Name, NVL(Comm, 100) + 50 AS TotalIncentive FROM emp;
Result:
+-------+----------------+
| Name  | TotalIncentive |
+-------+----------------+
| Alice | 550 (500+50)   |
| Bob   | 150 (100+50)   | ← NULL treated as 100
| Carol | 250 (200+50)   |
+-------+----------------+

Query 3: SELECT COALESCE(Comm, Bonus, 0) FROM emp;
Uses Comm; if NULL, uses Bonus; if both NULL, uses 0
```

## 4.5 NOT NULL Constraint

**Purpose:** Explicitly forbid NULL values for specific attributes.

**Syntax:**
```sql
CREATE TABLE STUDENT (
  RollNo INT NOT NULL PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  Email VARCHAR(100),       ← Can be NULL
  Age INT NOT NULL,
  Address VARCHAR(200)      ← Can be NULL
);
```

**Difference from Domain Constraint:**
- **Domain constraint**: Validates VALUE (type, range, format)
- **NOT NULL constraint**: Validates PRESENCE (existence required)

## 4.6 Best Practices with NULLs

| Scenario | Best Practice |
|----------|--------------|
| **Mandatory data** | Use NOT NULL constraint; require value at insertion |
| **Optional attributes** | Allow NULLs; test with IS NULL/IS NOT NULL |
| **Calculations** | Use NVL/COALESCE to handle NULLs; don't mix with = |
| **Foreign keys** | Allow NULL only if relationship is optional |
| **Aggregates** | Remember NULL is ignored by COUNT(*), SUM(), AVG() |
| **Design** | Minimize NULLs; use separate tables for optional data |

---

# 5. ENTITY-RELATIONSHIP MODEL BASICS

## 5.1 Introduction to ER Model

**ER Model** (proposed by Peter Chen, 1976) is a **conceptual data model** for database design.

**Purpose:**
- Provides high-level, easy-to-understand representation
- Independent of any specific DBMS or relational structure
- Bridges gap between real-world requirements and database implementation
- Foundation for mapping to relational schemas

**Levels in Database Design:**
```
Real-World Requirements
        ↓
Conceptual Design (ER/EER Diagrams)
        ↓
Logical Design (Relational Schemas)
        ↓
Physical Design (Indexes, File Organization)
        ↓
Implementation (SQL, DBMS)
```

## 5.2 Core Components: Entity, Attribute, Relationship

### Entity
**Definition:** A distinct object or thing in the real world with independent existence.

**Types:**
- **Physical entity**: Person, Car, Building (tangible)
- **Conceptual entity**: Company, Course, Job (abstract)

**ER Notation:** Rectangle with entity name (singular, capitalized)

```
┌──────────┐
│ STUDENT  │
└──────────┘
```

### Entity Type
**Definition:** A description/schema of an entity; defines the structure.

**Example:**
- Entity Type: STUDENT
- Real instance: "Alice Johnson, Roll 101, Age 20"

### Entity Set
**Definition:** Collection of all entities of the same entity type.

**Example:**
- Entity Set: All students enrolled in university (Alice, Bob, Carol, ...)

### Attribute
**Definition:** Property or characteristic describing an entity type.

**ER Notation:** Oval (ellipse) with attribute name

```
       ┌────────┐
       │  Name  │
       └────────┘
          /
    ┌──────────┐
    │ STUDENT  │
    └──────────┘
```

**Examples of attributes for STUDENT:**
- RollNo, Name, Age, DOB, Email, Address, PhoneNo, CGPA

---

# 6. ATTRIBUTES: TYPES & PROPERTIES

## 6.1 Simple (Atomic) Attribute

**Definition:** Cannot be divided further; has a single, indivisible value.

**Examples:**
- RollNo (single number)
- Age (single number)
- Email (single string)
- Name (single string)

**ER Notation:** Simple oval

```
┌─────────┐
│ RollNo  │
└─────────┘
```

## 6.2 Composite Attribute

**Definition:** Can be decomposed into smaller subattributes.

**Examples:**
- **Address** → Street, City, State, PIN
- **Name** → FirstName, LastName, MiddleName
- **DateTime** → Date + Time
- **Phone** → CountryCode, AreaCode, Number

**ER Notation:** Oval containing smaller ovals

```
          ┌──────────┐
          │ Address  │
          └──────────┘
           /  |  |  \
      Street City State PIN
```

**Relational Mapping:**
- **Option 1 (Flatten):** Store components as separate columns
  ```
  STUDENT(RollNo, Name, AddressStreet, AddressCity, AddressState, AddressPIN)
  ```
- **Option 2 (Normalize):** Create separate table for complex attribute
  ```
  STUDENT_ADDRESS(RollNo FK, AddressStreet, City, State, PIN)
  ```

## 6.3 Multivalued Attribute

**Definition:** Can hold **multiple values** for a single entity.

**Examples:**
- **PhoneNo**: Person may have mobile, home, office phones
- **Email**: May have multiple email addresses
- **Language**: Person speaks English, Spanish, French
- **Degree**: Person has Bachelor's, Master's, PhD

**ER Notation:** Double oval (double lines)

```
    ┌─────────────┐
    ║ PhoneNumber ║  ← Double oval
    └─────────────┘
         /
    ┌──────────┐
    │ PERSON   │
    └──────────┘
```

**Relational Mapping:**
Create separate table with owner's PK + multivalued attribute.

```
PERSON(PersonID, Name, Address)
PHONE(PersonID FK, PhoneNo)

Primary key of PHONE: (PersonID, PhoneNo) [composite]

Example:
PERSON:
+----------+-------+----------+
| PersonID | Name  | Address  |
+----------+-------+----------+
| 1        | Alice | 123 Main |
| 2        | Bob   | 456 Oak  |
+----------+-------+----------+

PHONE:
+----------+--------------+
| PersonID | PhoneNo      |
+----------+--------------+
| 1        | 555-0101     |
| 1        | 555-0102     |
| 2        | 555-0201     |
+----------+--------------+
```

## 6.4 Derived Attribute

**Definition:** Value can be **computed** from other attributes; not stored.

**Examples:**
- **Age** (derived from DOB)
- **Tenure** (derived from HireDate)
- **GPA** (derived from grades)
- **Balance** (sum of credits and debits)

**ER Notation:** Dashed oval

```
       ┌─────────┐
       │ DOB     │
       └─────────┘
          |
          |
       (derived)
          |
    ┌──────────────┐
    │    Age      │ ← Dashed oval
    └──────────────┘ (computed)
```

**Characteristics:**
- Not physically stored in table
- Calculated at query time
- Saves storage space
- Always up-to-date

**SQL Implementation:**
```sql
CREATE TABLE STUDENT (
  RollNo INT PRIMARY KEY,
  Name VARCHAR(50),
  DOB DATE,
  CGPA FLOAT
);

-- Age is NOT stored; calculated when needed:
SELECT Name, YEAR(CURRENT_DATE) - YEAR(DOB) AS Age FROM STUDENT;

-- Or create computed column (some DBs):
ALTER TABLE STUDENT ADD Age AS (YEAR(CURRENT_DATE) - YEAR(DOB));
```

## 6.5 Key Attribute

**Definition:** Attribute (or set) that **uniquely identifies** an entity in its entity set.

**ER Notation:** Oval with underlined name

```
    ┌─────────────┐
    │  RollNo     │ ← Underlined
    └─────────────┘
         /
    ┌──────────┐
    │ STUDENT  │
    └──────────┘
```

**Examples:**
- **STUDENT**: RollNo, StudentID
- **PERSON**: SSN, PassportNo
- **PRODUCT**: ProductCode, GTIN
- **CAR**: VIN (Vehicle Identification Number), RegistrationNo

**Relational Mapping:**
- Becomes **primary key** of the relation
- Inherits NOT NULL constraint

```sql
CREATE TABLE STUDENT (
  RollNo INT NOT NULL PRIMARY KEY,  ← Key attribute
  Name VARCHAR(50),
  CGPA FLOAT
);
```

## 6.6 Summary Table: Attribute Types

| Type | Single Value | Multiple Values | Derived | Decomposable | ER Symbol |
|------|:----------:|:---:|:---:|:---:|:---:|
| Simple | ✓ | ✗ | ✗ | ✗ | ○ |
| Composite | - | - | ✗ | ✓ | ○(○○) |
| Multivalued | ✗ | ✓ | ✗ | ✗ | ◎ |
| Derived | ✓ | ✗ | ✓ | ✗ | ◊ |
| Key | ✓ | ✗ | ✗ | ✗ | ○ underlined |

---

# 7. RELATIONSHIPS IN ER MODEL

## 7.1 Relationship Type & Relationship Set

**Relationship Type:** Association/link between entity types (defined in schema).

**Relationship Set:** Specific instances of that relationship (actual links between entity instances).

**ER Notation:** Diamond connecting entity rectangles

```
   STUDENT ◇─────── COURSE
            Enrolled_In
```

**Example:**
- Relationship Type: ENROLLED_IN (Student enrolls in Course)
- Relationship Set:
  - (Alice, CSC101) – Alice enrolled in CSC101
  - (Alice, MTH202) – Alice enrolled in MTH202
  - (Bob, CSC101) – Bob enrolled in CSC101

## 7.2 Degree of Relationship

**Degree:** Number of distinct entity types participating in a relationship.

### Unary (Recursive) Relationship
**Degree 1:** Single entity type relates to itself.

**Examples:**
- EMPLOYEE supervises EMPLOYEE (manager-worker relationship)
- PERSON married_to PERSON
- PREREQUISITE: COURSE is prerequisite for COURSE

**ER Diagram:**
```
       EMPLOYEE
         /  \
        /    \
   supervises
      /        \
     /          \
   (worker)    (manager)
```

**Relational Mapping - 1:1 or 1:N:**
```
EMPLOYEE(EmpID PK, Name, SupervisorID FK→EmpID)

Example:
+-------+-------+---------------+
| EmpID | Name  | SupervisorID  |
+-------+-------+---------------+
| 101   | Alice | NULL          | ← CEO has no supervisor
| 102   | Bob   | 101           | ← Bob reports to Alice
| 103   | Carol | 101           | ← Carol reports to Alice
| 104   | Dave  | 102           | ← Dave reports to Bob
+-------+-------+---------------+
```

**Relational Mapping - M:N:**
```
COURSE(CourseID PK, CourseName)
PREREQUISITE(CourseID PK/FK, PrerequisiteID PK/FK)

Example:
PREREQUISITE:
+----------+------------------+
| CourseID | PrerequisiteID   |
+----------+------------------+
| CSC201   | CSC101           | ← CSC201 requires CSC101
| CSC201   | MTH101           | ← CSC201 requires MTH101
| CSC301   | CSC201           | ← CSC301 requires CSC201
+----------+------------------+
```

### Binary Relationship
**Degree 2:** Two different entity types participate (most common).

**Examples:**
- STUDENT enrolls_in COURSE
- EMPLOYEE works_for DEPARTMENT
- PERSON owns CAR

**ER Diagram:**
```
STUDENT ─────── COURSE
  |              |
  └─ enrolls_in ─┘
```

### N-ary Relationship
**Degree > 2:** More than two entity types participate.

**Example:**
```
   SUPPLIER ──\
              ├── supplies
   PART ──────┤
              │
   WAREHOUSE ─/

Meaning: Supplier supplies Part to Warehouse
(not just Supplier-Part or Part-Warehouse)
```

**Relational Mapping:**
```
SUPPLIER(SupplierID PK, Name, Location)
PART(PartID PK, PartName, Price)
WAREHOUSE(WarehouseID PK, Location, Capacity)

SUPPLIES(SupplierID FK, PartID FK, WarehouseID FK, Quantity, DeliveryDate)
         ^^^^^^^^^^     ^^^^^^     ^^^^^^^^^^^^^
         All are PK/FK; together form composite PK
```

---

# 8. STRUCTURAL CONSTRAINTS

## 8.1 Overview

**Structural constraints** specify **how many** entities can participate and **whether** participation is mandatory.

They consist of two components:
1. **Cardinality ratio** (one-to-one, one-to-many, etc.)
2. **Participation constraint** (total or partial)

## 8.2 Cardinality Ratios (Relationship Cardinalities)

Cardinality describes the **maximum number** of entity instances that can be related.

### One-to-One (1:1)
**Definition:** One entity in A relates to **at most one** entity in B, and vice versa.

**Real-world examples:**
- PERSON has ONE PASSPORT, PASSPORT issued to ONE PERSON
- EMPLOYEE manages ONE DEPARTMENT, DEPARTMENT managed by ONE EMPLOYEE
- CITIZEN of ONE COUNTRY, COUNTRY has ONE capital

**ER Notation:**
```
PERSON ─── 1:1 ─── PASSPORT
          Owns
```

**Relational Mapping:**
```
PERSON(PersonID PK, Name, PassportNo FK)
PASSPORT(PassportNo PK, IssuedDate, ExpiryDate)

OR

PERSON(PersonID PK, Name)
PASSPORT(PassportNo PK, IssuedDate, ExpiryDate, PersonID FK)
```

**Visual Example:**
```
PERSON              PASSPORT
+-------+---+       +-------+---+
| PID | Name       | PassNo| PID|
+-------+---+       +-------+---+
| 1   | Alice  ←→   | P101  | 1  |
| 2   | Bob    ←→   | P102  | 2  |
| 3   | Carol  ←→   | P103  | 3  |
+-------+---+       +-------+---+
Each Person has ONE Passport
```

### One-to-Many (1:N)
**Definition:** One entity in A relates to **zero, one, or many** entities in B; but one entity in B relates to **exactly one** entity in A.

**Real-world examples:**
- DEPARTMENT has MANY EMPLOYEES, but EMPLOYEE works for ONE DEPARTMENT
- BANK has MANY BRANCHES, BRANCH belongs to ONE BANK
- TEACHER teaches MANY COURSES, COURSE taught by ONE TEACHER

**ER Notation:**
```
DEPARTMENT ─── 1:N ─── EMPLOYEE
    │         Works_For    │
    └─ 1 (one dept) ─ N (many emp) ─┘
```

**Relational Mapping:**
```
DEPARTMENT(DeptID PK, DeptName, Location)
EMPLOYEE(EmpID PK, Name, Salary, DeptID FK)
                                  ^^^^^^
                            Points to ONE department
```

**Visual Example:**
```
DEPARTMENT              EMPLOYEE
+-------+-------+       +-------+-------+------+
| DID | DName  |       | EID | Name  | DID |
+-------+-------+       +-------+-------+------+
| D1  | Sales  |       | E1  | Alice | D1  |
|     |        |  →→→  | E2  | Bob   | D1  |
|     |        |  →→→  | E3  | Carol | D1  |
| D2  | IT     |       | E4  | Dave  | D2  |
|     |        |  →→→  | E5  | Eve   | D2  |
+-------+-------+       +-------+-------+------+

Many Employees per Department
One Department per Employee
```

### Many-to-One (N:1)
**Definition:** Same as 1:N but notation reverses perspectives.

```
EMPLOYEE ─── N:1 ─── DEPARTMENT
```

**Functionally identical to 1:N; just different notation.**

### Many-to-Many (M:N)
**Definition:** One entity in A can relate to **many** entities in B, and one entity in B can relate to **many** entities in A.

**Real-world examples:**
- STUDENT enrolls in MANY COURSES, COURSE has MANY STUDENTS
- DOCTOR examines MANY PATIENTS, PATIENT sees MANY DOCTORS
- PROJECT assigned MANY EMPLOYEES, EMPLOYEE works MANY PROJECTS

**ER Notation:**
```
STUDENT ─── M:N ─── COURSE
            Enrolls_In
```

**Relational Mapping:**
```
STUDENT(StudentID PK, Name, Email)
COURSE(CourseID PK, CourseName, Credits)
ENROLLMENT(StudentID FK, CourseID FK, Semester, Grade)
           ^^^^^^^^^^^  ^^^^^^^^^^
           Together form composite PK
```

**Visual Example:**
```
STUDENT                 ENROLLMENT           COURSE
+---+-------+          +---+----+---+       +-----+--------+
|SID|Name   |          |SID|CID|Gr |       |CID  |CName    |
+---+-------+          +---+----+---+       +-----+--------+
| 1 | Alice |  ←→→→    | 1 | C1 | A | ←→   | C1  | Java    |
|   |       |  ←→→→    | 1 | C2 | B | ←→   | C2  | Database|
| 2 | Bob   |  ←→→→    | 2 | C1 | B | ←→   | C3  | Python  |
|   |       |  ←→→→    | 2 | C3 | A |      +-----+--------+
+---+-------+          +---+----+---+

Alice enrolled in C1, C2
Bob enrolled in C1, C3
C1 has students Alice, Bob
```

---

## 8.3 Participation Constraints

Participation describes **whether** an entity **must** or **can** participate in a relationship.

### Total Participation (Mandatory)
**Definition:** Every entity in an entity set **must participate** in at least one relationship instance.

**Interpretation:**
- "Each [entity] must [relationship]"
- No entity can exist without the relationship

**ER Notation:** Double line between entity and relationship

```
STUDENT ══════ ENROLLMENT ═══════ COURSE
         (double line = total)

Means: Every student must be enrolled in at least one course
```

**Relational Effect:**
- Foreign key can be NULL only if relationship is optional
- If total participation, FK typically NOT NULL (or design differently)

**Real-world Examples:**
- Every EMPLOYEE must work for a DEPARTMENT (total)
- Every COURSE must be taught by a TEACHER (total)

### Partial Participation (Optional)
**Definition:** Some entities in an entity set **may not participate** in the relationship.

**Interpretation:**
- "An entity can [relationship] or not"
- Entity can exist without the relationship

**ER Notation:** Single line between entity and relationship

```
PERSON ───── OWNS ────── CAR
       (single line = partial)

Means: A Person may or may not own a Car
```

**Real-world Examples:**
- A PERSON may or may not have a DRIVER_LICENSE (partial)
- A STUDENT may or may not have a SCHOLARSHIP (partial)

---

## 8.4 Min-Max Notation (Alternative)

Modern ER diagrams use (min, max) notation on lines:

```
STUDENT ──(0,1)──── SCHOLARSHIP ──(1,1)──── AMOUNT
  ↑                     ↑
  min=0 (partial)   min=1 (total)
  max=1              max=1

Interpretation:
- Student: min=0, max=1 → optional, can have at most 1
- Scholarship: min=1, max=1 → mandatory, must have exactly 1
```

**General Rule:**
- **min = 0** → Partial participation
- **min ≥ 1** → Total participation
- **max = 1** → One-to-one on that side
- **max = N** → One-to-many on that side

**Examples:**
```
DEPARTMENT ──(1,1)──── MANAGER ──(0,1)──── PERSON
│                                          │
Every Dept has exactly 1 Mgr    Person may manage at most 1 Dept

STUDENT ──(1,N)──── ENROLLMENT ──(0,1)──── COURSE
│
Every Student enrolled ≥ 1 course    But Course may have 0 Students
```

---

## 8.5 Combined Structural Constraints

**Cardinality + Participation together define the relationship:**

| Constraint | 1:1, Both Partial | 1:1, One Total | 1:1, Both Total | 1:N, N Total | M:N |
|-----------|:---:|:---:|:---:|:---:|:---:|
| **Meaning** | Optional on both sides | Required on one side | Required both sides | Required on many | Required both |
| **Mapping** | FK on either side (or separate table) | FK on total side | Often merge tables | FK on N-side | New table required |

**Real Example:**
```
EMPLOYEE ════ (1:1) ════ OFFICE
│            double line
└─ Total: Every employee has office; Every office has employee

MAPPING: Merge into single table OR
         EMPLOYEE(EmpID PK, Name, OfficeID FK UNIQUE)
         OFFICE(OfficeID PK, Building, Floor)
```

---

# 9. WEAK ENTITIES

## 9.1 Definition & Characteristics

**Weak Entity Type:** An entity type that **cannot be uniquely identified** by its own attributes alone; depends on another entity type (owner/parent).

**Characteristics:**
- No key attribute of its own
- Has a **partial key** (discriminator) that provides uniqueness within context of owner
- Existence depends on owner entity (existence dependency)
- Participation with owner is always **total**
- Cannot exist without the owner

**ER Notation:**
- Double rectangle (not single rectangle)
- Relationship with owner is double diamond (identifying relationship)
- Partial key is double-underlined

```
         EMPLOYEE
    ┌──────────────┐
    │ (rectangle)  │
    └──────────────┘
           ║
      (double line)
         ║║
          ║
    ┌──────────────┐
    ║ (double rect)║
    └──────────────┘
      DEPENDENT

Partial key: DependentName (not unique alone)
Identifying relationship: Employee has Dependent
```

**Real-world Examples:**

```
Example 1: DEPENDENT
━━━━━━━━━━
EMPLOYEE has DEPENDENT
- Dependent Name alone doesn't uniquely identify
- Need EmpID + DependentName together
- If Employee deleted, Dependent record must also be deleted

Example 2: PAYMENT
━━━━━━━━━━
LOAN has PAYMENT
- Payment Number alone doesn't uniquely identify
- Need LoanNo + PaymentNo together
- Payments only exist if Loan exists

Example 3: ACCOUNT_DETAIL
━━━━━━━━━━
BANK_ACCOUNT has ACCOUNT_DETAIL
- Detail sequence alone doesn't identify
- Need AccountNo + DetailSeq together
```

## 9.2 Weak Entity vs Strong Entity

| Aspect | Strong Entity | Weak Entity |
|--------|---|---|
| **Key** | Has own key (key attribute) | No key; has partial key |
| **Uniqueness** | Identified by own attributes | Identified by owner + partial key |
| **Existence** | Independent | Dependent on owner |
| **Relationship** | Regular diamond | Identifying (double diamond) |
| **ER Notation** | Single rectangle | Double rectangle |
| **Participation** | Can be partial or total | Always total with owner |

---

## 9.3 Relational Mapping of Weak Entities

**Rule:** Weak entity table includes:
1. All attributes of weak entity
2. Primary key of owner entity (as foreign key)
3. Partial key attributes
4. Composite primary key = owner's PK + partial key

**Example:**

```
ER Model:
EMPLOYEE (EmpID, Name, Salary)
    ║
    ║ has (identifying relationship)
    ║
DEPENDENT (DependentName, Relation, DOB) ← DependentName is partial key

Relational Schema:
EMPLOYEE(EmpID PK, Name, Salary)

DEPENDENT(EmpID FK→EMPLOYEE(EmpID), DependentName, Relation, DOB)
          ^^^^^^ from owner           ^^^ partial key
          Primary Key: (EmpID, DependentName) ← composite

OR with better formatting:
DEPENDENT(EmpID PK/FK, DependentName PK, Relation, DOB)
```

**Table Contents:**
```
EMPLOYEE:
+-------+-------+--------+
| EmpID | Name  | Salary |
+-------+-------+--------+
| 101   | Alice | 50000  |
| 102   | Bob   | 55000  |
+-------+-------+--------+

DEPENDENT:
+-------+------------------+---------+------------+
| EmpID | DependentName    | Relation| DOB        |
+-------+------------------+---------+------------+
| 101   | John (son)       | Son     | 2010-05-10 |
| 101   | Mary (wife)      | Spouse  | 1985-03-20 |
| 102   | Tom (son)        | Son     | 2012-07-15 |
+-------+------------------+---------+------------+

Primary Keys:
- EMPLOYEE: EmpID (unique; identifies employee)
- DEPENDENT: (EmpID, DependentName) (composite; uniquely identifies dependents)
```

**Why (EmpID, DependentName) is necessary:**
- Alice's employee ID is 101
- Alice has two children: John and Mary
- DependentName alone (John, Mary) is NOT unique across all employees
- But (101, John) uniquely identifies John as Alice's child

---

# 10. MAPPING ER TO RELATIONAL SCHEMA

## 10.1 Systematic Conversion Algorithm

Convert ER diagrams to relational schemas step by step:

```
Step 1: Map strong entities
Step 2: Map weak entities
Step 3: Map binary 1:1 relationships
Step 4: Map binary 1:N relationships
Step 5: Map binary M:N relationships
Step 6: Map multivalued attributes
Step 7: Map unary/recursive relationships
Step 8: Map n-ary relationships (n > 2)
Step 9: Handle composite attributes
Step 10: Map derived attributes (usually skip—computed at query time)
```

## 10.2 Step 1: Map Strong Entities

**Rule:** Create a relation for each strong entity set.
- Relation name = entity set name
- Attributes = all simple attributes
- Primary key = key attribute(s)
- Composite attributes: flatten into component attributes
- Derived attributes: usually omit (calculate when needed)

**Example:**

```
ER Model:
┌──────────────┐
│ STUDENT      │
├──────────────┤
│ RollNo (PK)  │
│ Name         │
│ Age          │
│ Email        │
└──────────────┘

Relational Schema:
STUDENT(RollNo PK, Name, Age, Email)

Or with domain info:
STUDENT(RollNo INT NOT NULL PRIMARY KEY,
        Name VARCHAR(50),
        Age INT,
        Email VARCHAR(100))
```

**With Composite Attributes:**

```
ER Model:
┌────────────────────┐
│ STUDENT            │
├────────────────────┤
│ RollNo (PK)        │
│ Name               │
│ Address:           │ ← Composite
│   └─Street         │
│   └─City           │
│   └─PIN            │
└────────────────────┘

Relational Schema (flatten composite):
STUDENT(RollNo PK, Name, AddressStreet, AddressCity, AddressPIN)
```

---

## 10.3 Step 2: Map Weak Entities

**Rule:** Create a relation for weak entity with:
- All weak entity attributes
- Primary key of owner entity (as FK)
- Partial key attributes
- Composite PK = owner's PK + partial key

**Example:**

```
ER Model:
EMPLOYEE (EmpID, Name) ─────has────── DEPENDENT (DepName, Relation)
           (strong)        (identifying)   (weak)
                           (double diamond)

Relational Schema:
EMPLOYEE(EmpID PK, Name)
DEPENDENT(EmpID PK/FK→EMPLOYEE, DepName PK, Relation)
```

---

## 10.4 Step 3: Map 1:1 Relationships

**Three approaches depending on participation:**

### 3a. Both Partial (Neither Side Required)

**Choice:** Add FK to either side OR create separate relationship table.

```
PERSON ───── (0,1) ───── (0,1) ───── PASSPORT
           Owns

Option 1 - FK on PERSON side:
PERSON(PersonID PK, Name, PassportNo FK UNIQUE)
PASSPORT(PassportNo PK, IssuedDate, ExpiryDate)

Option 2 - FK on PASSPORT side:
PERSON(PersonID PK, Name)
PASSPORT(PassportNo PK, IssuedDate, ExpiryDate, PersonID FK UNIQUE)

Option 3 - Separate junction table:
PERSON(PersonID PK, Name)
PASSPORT(PassportNo PK, IssuedDate, ExpiryDate)
OWNS(PersonID FK UNIQUE, PassportNo FK UNIQUE)
```

### 3b. One Side Total (Mandatory)

**Rule:** Add FK to the total participation side.

```
EMPLOYEE ───── (1,1) ───── (0,1) ───── OFFICE
          Works_In
       (total)     (partial)

MAPPING:
EMPLOYEE(EmpID PK, Name, Salary, OfficeID FK)
         ◁─ FK added because EMPLOYEE is total
OFFICE(OfficeID PK, Building, Floor)

Interpretation: Every employee must have an office (not NULL)
                Some offices may not be assigned to an employee (optional)
```

### 3c. Both Total (Both Mandatory)

**Rule:** Merge into single relation OR create junction table.

```
MANAGER ───── (1,1) ───── (1,1) ───── DEPARTMENT
            Manages
        (total)       (total)

Option 1 - Merge (simple):
DEPARTMENT(DeptID PK, DeptName, ManagerID FK UNIQUE)
MANAGER(ManagerID PK, Name, Experience)
(Implies: every department has a manager, every manager manages a department)

Option 2 - Keep separate with junction (complex):
DEPARTMENT(DeptID PK, DeptName)
MANAGER(ManagerID PK, Name, Experience)
MANAGES(ManagerID FK UNIQUE, DeptID FK UNIQUE)
```

---

## 10.5 Step 4: Map 1:N Relationships

**Rule:** Add FK to the **N-side** (many-side). Include relationship attributes on N-side.

```
DEPARTMENT ───── (1) ───── (N) ───── EMPLOYEE
              Works_For
             (one dept)  (many emp)

MAPPING:
DEPARTMENT(DeptID PK, DeptName, Location)
EMPLOYEE(EmpID PK, Name, Salary, DeptID FK)
                                  ◁─ FK on N-side

Example:
+--------+----------+
| DeptID | DeptName |
+--------+----------+
| D1     | Sales    |
| D2     | IT       |
+--------+----------+

+-------+-------+--------+-------+
| EmpID | Name  | Salary | DeptID|
+-------+-------+--------+-------+
| 1     | Alice | 50000  | D1    | ← Points to D1
| 2     | Bob   | 55000  | D1    | ← Points to D1
| 3     | Carol | 60000  | D2    | ← Points to D2
+-------+-------+--------+-------+

FK DeptID ensures:
- Every employee's DeptID points to a valid department
- One department can be referenced by many employees
```

**With Relationship Attributes:**

```
ER:
DEPARTMENT ─── (1) ─── WORKS_FOR ─── (N) ─── EMPLOYEE
                   (HireDate)
                   (attributes of relationship)

MAPPING:
EMPLOYEE(EmpID PK, Name, Salary, DeptID FK, HireDate)
                                  ◁─ include relationship attributes
```

---

## 10.6 Step 5: Map M:N Relationships

**Rule:** Create a new **junction/bridge table** with:
- PKs of both entity sets (as FKs)
- All relationship attributes
- Combined PKs usually form composite PK of junction table

```
STUDENT ──────────── M:N ───────────── COURSE
          Enrolls_In
      (many students)  (many courses)

MAPPING:
STUDENT(StudentID PK, Name, Email, Major)
COURSE(CourseID PK, CourseName, Credits)
ENROLLMENT(StudentID PK/FK, CourseID PK/FK, Semester, Grade)
           ^^^^^^^^^^^     ^^^^^^^^^
           Together form composite PK

Why separate table?
- Cannot add FK to just STUDENT (would imply one course per student)
- Cannot add FK to just COURSE (would imply one student per course)
- Need separate table to represent many-to-many association
```

**Table Example:**

```
STUDENT:
+----------+-------+
| StudentID| Name  |
+----------+-------+
| S1       | Alice |
| S2       | Bob   |
+----------+-------+

COURSE:
+----------+--------+
| CourseID | CName  |
+----------+--------+
| C1       | Java   |
| C2       | Python |
+----------+--------+

ENROLLMENT:
+----------+----------+----------+-------+
| StudentID| CourseID | Semester | Grade |
+----------+----------+----------+-------+
| S1       | C1       | Fall2024 | A     |
| S1       | C2       | Fall2024 | B     |
| S2       | C1       | Fall2024 | B     |
+----------+----------+----------+-------+

Composite PK: (StudentID, CourseID, Semester)
or just (StudentID, CourseID) if only one enrollment per pair per lifetime
```

---

## 10.7 Step 6: Map Multivalued Attributes

**Rule:** Create a separate relation with:
- Owner entity's PK (as FK)
- The multivalued attribute
- Composite PK = owner's PK + multivalued attribute

```
ER Model:
┌──────────────┐
│ PERSON       │
├──────────────┤
│ PersonID (PK)│
│ Name         │
│ ║PhoneNo║    │ ← Double oval (multivalued)
└──────────────┘

Relational Schema:
PERSON(PersonID PK, Name)
PHONE(PersonID FK, PhoneNo)
      ◁─ Composite key needed

Primary key of PHONE: (PersonID, PhoneNo)

Example:
PERSON:
+----------+-------+
| PersonID | Name  |
+----------+-------+
| 1        | Alice |
| 2        | Bob   |
+----------+-------+

PHONE:
+----------+--------+
| PersonID | PhoneNo|
+----------+--------+
| 1        | 555001 |
| 1        | 555002 |
| 2        | 555201 |
+----------+--------+

Each phone number is unique per person;
Same number not assigned to two persons
```

---

## 10.8 Step 7: Map Recursive (Unary) Relationships

### 7a. Unary 1:1 or 1:N

**Rule:** Add self-referencing FK in the same relation.

```
ER Model:
   EMPLOYEE
    /  \
   /    \
  supervises
   /      \
(worker) (manager)
  |        |
  └────────┘
Same entity type

Relational Schema:
EMPLOYEE(EmpID PK, Name, Salary, SupervisorID FK→EMPLOYEE)
                                   ◁─ Points to same table

Example:
+-------+-------+--------+---------------+
| EmpID | Name  | Salary | SupervisorID  |
+-------+-------+--------+---------------+
| 101   | Alice | 50000  | NULL          | ← CEO (no supervisor)
| 102   | Bob   | 45000  | 101           | ← Bob's supervisor is Alice
| 103   | Carol | 48000  | 101           | ← Carol's supervisor is Alice
| 104   | Dave  | 43000  | 102           | ← Dave's supervisor is Bob
+-------+-------+--------+---------------+
```

### 7b. Unary M:N

**Rule:** Create separate junction table with two FKs both pointing to same entity.

```
ER Model:
COURSE
 /  \
/    \
 Prerequisite
 /        \
(course) (prereq)

Relational Schema:
COURSE(CourseID PK, CourseName, Credits)
PREREQUISITE(CourseID PK/FK→COURSE, PrereqID PK/FK→COURSE)
             ◁─ Both FKs point to COURSE

Example:
COURSE:
+----------+----------+
| CourseID | CourseName|
+----------+----------+
| C1       | Java      |
| C2       | OOP       |
| C3       | Advanced  |
+----------+----------+

PREREQUISITE:
+----------+----------+
| CourseID | PrereqID |
+----------+----------+
| C2       | C1       | ← C2 requires C1
| C3       | C2       | ← C3 requires C2
+----------+----------+
```

---

## 10.9 Step 8: Map N-ary Relationships (n > 2)

**Rule:** Create junction table with PKs of all participating entities (as FKs) + relationship attributes. Combined PKs usually form composite PK.

```
ER Model:
   SUPPLIER──\
             ├── Supplies
   PART ─────┤
             │
   WAREHOUSE─/

(Meaning: Supplier provides Part to Warehouse)

Relational Schema:
SUPPLIER(SupplierID PK, Name, Address)
PART(PartID PK, PartName, Price)
WAREHOUSE(WarehouseID PK, Location, Capacity)

SUPPLIES(SupplierID FK, PartID FK, WarehouseID FK, Quantity, DeliveryDate)
         ^^^^^^^^^^     ^^^^^^     ^^^^^^^^^^^^^ 
         All form composite PK together

Example:
SUPPLIER:
+-----------+-------+
| SupplierID| Name  |
+-----------+-------+
| SP1       | ABC   |
| SP2       | XYZ   |
+-----------+-------+

PART:
+-------+--------+
| PartID| Name   |
+-------+--------+
| P1    | Bolt   |
| P2    | Screw  |
+-------+--------+

WAREHOUSE:
+----+----------+
| WID| Location |
+----+----------+
| W1 | NYC      |
| W2 | Boston   |
+----+----------+

SUPPLIES:
+---+----+----+----------+-----------+
|SID|PID |WID |Quantity  | DelivDate |
+---+----+----+----------+-----------+
|SP1| P1 | W1 | 100      | 2024-01-15|
|SP1| P2 | W1 | 50       | 2024-01-15|
|SP2| P1 | W2 | 75       | 2024-01-20|
+---+----+----+----------+-----------+

Composite PK: (SupplierID, PartID, WarehouseID)
```

---

## 10.10 Step 9: Composite Attributes

**Rule:** Flatten composite attributes into separate columns.

```
ER Model:
┌────────────────────┐
│ STUDENT            │
├────────────────────┤
│ RollNo (PK)        │
│ Name               │
│ Address:           │
│   └─Street         │
│   └─City           │
│   └─State          │
│   └─PIN            │
└────────────────────┘

Relational Schema:
STUDENT(RollNo PK, Name, AddressStreet, AddressCity, AddressState, AddressPIN)

OR with better naming:
STUDENT(RollNo PK, Name, Street, City, State, PIN)
```

---

# 11. ADVANCED ER MAPPING SCENARIOS

## 11.1 Combined: Weak Entities + Relationships

```
ER Model:
HOSPITAL (HospID, Name)
  ║
  ║ has (identifying relationship)
  ║
DEPARTMENT (DeptName, ...weak entity...)
  │
  │ has (regular 1:N relationship)
  │
DOCTOR (DoctorID, Name, Specialty)

Mapping:
HOSPITAL(HospID PK, Name)

DEPARTMENT(HospID PK/FK→HOSPITAL, DeptName PK, Budget, Location)
           ◁─ Weak entity with owner FK as part of PK

DOCTOR(DoctorID PK, Name, Specialty, HospID FK→HOSPITAL)
           ◁─ Points directly to hospital
           ◁─ OR to specific department (HospID, DeptName) FK
```

---

## 11.2 Multiple Relationships Between Same Entities

```
ER Model:
         MANAGER        SUPERVISOR
            ↙              ↖
        EMPLOYEE ◇─ manages ◇─ supervises ◇─ EMPLOYEE
                      (relationships)

Mapping:
EMPLOYEE(EmpID PK, Name, Salary, ManagerID FK1, SupervisorID FK2)
                           ◁─────────────┘       ◁──────────┘
                         FK to different roles

OR with roles explicit:
EMPLOYEE(EmpID PK, Name, Salary, ManagerEmpID FK1→EMPLOYEE, 
                                  SupervisorEmpID FK2→EMPLOYEE)
```

---

# 12. EXTENDED ER MODEL (EER)

## 12.1 Introduction to EER

**EER (Extended ER)** extends ER with:
- Superclass/subclass relationships
- Generalization
- Specialization
- Inheritance of attributes
- Disjointness and participation constraints
- Aggregation

**Purpose:** Better represent complex real-world structures with classification hierarchies.

## 12.2 Generalization & Specialization

### Generalization (Bottom-Up)
**Definition:** Process of combining similar entity types into a higher-level superclass.

**Logic:**
- Identify common attributes and relationships
- Create superclass containing these commons
- Subclasses inherit all superclass attributes

**Example:**
```
Real entities: STUDENT, FACULTY, STAFF (all are people in university)
              ↓ Generalization
Common superclass: PERSON (attributes: PersonID, Name, Age, Address)
Specific subclasses: STUDENT (additional: RollNo, CGPA)
                     FACULTY (additional: Salary, Department)
                     STAFF (additional: Position, Salary)
```

**ER Notation:**
```
            PERSON
       (superclass)
       │ Generalization
       │ or
       └─ Specialization
            │
       ┌────┼────┐
       │    │    │
    STUDENT FACULTY STAFF
    (subclasses)
```

### Specialization (Top-Down)
**Definition:** Process of dividing an entity type into more specific subtypes.

**Logic:**
- Start with general entity
- Identify specific variations
- Create subclasses for each variation
- Subclasses inherit superclass attributes + add own

**Example:**
```
VEHICLE (general type)
   ↓ Specialization (based on usage)
   ├─ CAR
   ├─ TRUCK
   ├─ MOTORCYCLE
   └─ BICYCLE

Attributes:
VEHICLE: VehicleID, Make, Model, Year, Color
CAR: + NumDoors, Transmission, FuelType
TRUCK: + LoadCapacity, GrossDrivingWeight
MOTORCYCLE: + EngineCC, HandlebarType
BICYCLE: + GearCount, FrameSize
```

**ER Notation:**
```
           VEHICLE
        (superclass)
          │
          ├─ specialization
          │
    ┌─────┼─────┬────────┐
    │     │     │        │
   CAR  TRUCK MOTORCYCLE BICYCLE
   (subclasses)
```

---

## 12.3 Specialization Constraints

### Total Specialization
**Definition:** Every superclass entity **must** belong to at least one subclass.

**Meaning:** No "generic" entity exists; all must be specialized.

**ER Notation:** Double lines from superclass to specialization triangle

```
           PERSON
            ║
         (double line = total)
         ║ │ ║
       ┌─┴─┴─┘
       │
    ◇───◇  ← Specialization arrow/triangle
    │   │
STUDENT FACULTY
    
Every PERSON must be either a STUDENT or FACULTY (or both)
```

**Example:**
```
In a university database:
PERSON(PersonID, Name, Age)
Every person in the database is a STUDENT, FACULTY, or both
No one is just a generic "person" with no role
```

### Partial Specialization
**Definition:** Some superclass entities **may not** belong to any subclass.

**Meaning:** Generic entities can exist.

**ER Notation:** Single line from superclass to specialization triangle

```
            PERSON
             │
         (single line = partial)
             │
          ◇─┼─◇
          │ │
       STUDENT EMPLOYEE
    
Some PERSON entities are neither STUDENT nor EMPLOYEE
```

**Example:**
```
PERSON(PersonID, Name, Age)
STUDENT(PersonID FK, RollNo)
EMPLOYEE(PersonID FK, EmpID)

Person with PersonID=100, Name="John", Age=50 exists
but is neither a student nor employee (partial specialization allows this)
```

---

### Disjoint Specialization
**Definition:** Subclasses are **mutually exclusive**; a superclass entity can belong to **at most one** subclass.

**Meaning:** No overlap; cannot be two things at once.

**ER Notation:** Symbol "d" on specialization triangle

```
           ACCOUNT
             │
             ◇ d  ← d means disjoint
          / | \
    SAVINGS CHECKING MONEY_MARKET
    
Account is EITHER Savings OR Checking OR Money Market
(not multiple simultaneously)
```

**Example:**
```
VEHICLE(VehicleID, Make, Model)
CAR(VehicleID FK, NumDoors)
TRUCK(VehicleID FK, LoadCapacity)
MOTORCYCLE(VehicleID FK, EngineCC)

Vehicle with ID=V1 can be CAR OR TRUCK OR MOTORCYCLE
But NOT both CAR and TRUCK simultaneously
```

---

### Overlapping Specialization
**Definition:** Subclasses are **not mutually exclusive**; a superclass entity can belong to **multiple** subclasses.

**Meaning:** Overlap allowed; can be multiple things.

**ER Notation:** Symbol "o" on specialization triangle

```
            PERSON
             │
             ◇ o  ← o means overlap
          / | \
      STUDENT EMPLOYEE VOLUNTEER
    
A PERSON can be:
- STUDENT only
- EMPLOYEE only
- STUDENT AND EMPLOYEE (part-time student working full-time)
- STUDENT AND VOLUNTEER
- EMPLOYEE AND VOLUNTEER
- All three: STUDENT, EMPLOYEE, VOLUNTEER
```

**Example:**
```
PERSON(PersonID, Name, Age)
STUDENT(PersonID FK, RollNo, CGPA)
EMPLOYEE(PersonID FK, EmpID, Salary)

Person ID=1: John (STUDENT only; RollNo=101, EmpID=NULL)
Person ID=2: Alice (EMPLOYEE only; RollNo=NULL, EmpID=E001)
Person ID=3: Bob (STUDENT AND EMPLOYEE; RollNo=102, EmpID=E002)
                   ◁─ Overlapping specialization allows this
```

---

## 12.4 Constraints Summary

| Type | Meaning | ER Symbol | Relational Implication |
|------|---------|-----------|------------------------|
| **Total** | Must be in subclass | Double line | Superclass may be empty |
| **Partial** | May be in subclass | Single line | Superclass has orphan entities |
| **Disjoint** | At most one subclass | d | No overlap in FK mappings |
| **Overlapping** | Multiple subclasses | o | Can be in multiple tables |

---

## 12.5 Aggregation

**Definition:** Treating a relationship (association between entities) as a **higher-level entity** so it can participate in additional relationships.

**When Used:**
- When a relationship itself needs to be related to another entity
- Relationship is not just a simple link but has significance

**ER Notation:** Rectangle enclosing a relationship diamond

```
ER without aggregation (incomplete):
EMPLOYEE ─── works_on ─── PROJECT
             │
             └─── manages ─── DEPARTMENT
                    ↑
                    Cannot specify "manages which work_on relationship"

ER with aggregation (correct):
       ┌──────────────────────────┐
       │ EMPLOYEE ─ works_on - PROJECT │
       │ (aggregated as single entity) │
       └──────────────────────────┘
                    │
                    │ managed_by
                    │
              DEPARTMENT
```

**Real Example:**
```
EMPLOYEE ─── WORKS_ON ─── PROJECT
   │                          │
   │                          │
   │                          │
   └──── managed_by ──────────┘

Meaning:
- Employees work on projects (many-to-many)
- A SPECIFIC WORK_ON (assignment of employee to project) is managed by a department
- Example: Bob's assignment to Database project is managed by IT department
          Alice's assignment to same Database project is managed by HR

Without aggregation: Cannot express this
With aggregation: Treat (EMPLOYEE, WORKS_ON, PROJECT) as aggregate entity
                  that relates to DEPARTMENT
```

**Mapping:**
```
EMPLOYEE(EmpID PK, Name, Salary)
PROJECT(ProjectID PK, ProjectName, Budget)
WORKS_ON(EmpID FK, ProjectID FK, HoursPerWeek)
         ◁─ Primary key is (EmpID, ProjectID)

DEPARTMENT(DeptID PK, DeptName)

MANAGES(EmpID FK, ProjectID FK, DeptID FK→DEPARTMENT)
        ◁─ Both FKs refer to (EmpID, ProjectID) in WORKS_ON
        ◁─ DeptID refers to DEPARTMENT
```

---

# 13. MAPPING EER TO RELATIONAL SCHEMA

## 13.1 Overview

EER specialization/generalization can be mapped using four strategies. Choice depends on:
- How many attributes in subclasses
- Total vs partial specialization
- Disjoint vs overlapping specialization

---

## 13.2 Strategy 1: Superclass + Separate Subclass Tables

**When to use:**
- Subclasses have **many specific attributes**
- Clear local attributes in each subclass
- Works for both total and partial, disjoint and overlapping

**Mapping:**
- One table for superclass (with PK only)
- One table for each subclass
- Subclass PK = FK to superclass (foreign key that is also primary key)
- Subclass also contains own attributes

**Example:**

```
ER Model:
                PERSON
           (PersonID, Name, Age)
              │ specialization
         ┌────┼─────┐
         │    │     │
     STUDENT FACULTY STAFF

Relational Schema:
PERSON(PersonID PK, Name, Age)

STUDENT(PersonID PK/FK→PERSON, RollNo, CGPA, MajorDept)
        ◁─ PersonID is both PK and FK

FACULTY(PersonID PK/FK→PERSON, FacultyID, Department, Salary)
        ◁─ PersonID is both PK and FK

STAFF(PersonID PK/FK→PERSON, StaffID, Position, Salary)
      ◁─ PersonID is both PK and FK

Example Data:
PERSON:
+----------+-------+-----+
| PersonID | Name  | Age |
+----------+-------+-----+
| 1        | Alice | 20  |
| 2        | Bob   | 45  |
| 3        | Carol | 25  |
+----------+-------+-----+

STUDENT:
+----------+--------+------+-----------+
| PersonID | RollNo | CGPA | MajorDept |
+----------+--------+------+-----------+
| 1        | S001   | 3.8  | CS        |
| 3        | S002   | 3.6  | Math      |
+----------+--------+------+-----------+

FACULTY:
+----------+-----------+------------+--------+
| PersonID | FacultyID | Department | Salary |
+----------+-----------+------------+--------+
| 2        | F001      | CS         | 80000  |
+----------+-----------+------------+--------+

STAFF:
+----------+--------+---------+--------+
| PersonID | StaffID| Position| Salary |
+----------+--------+---------+--------+
(empty or other staff members)
+----------+--------+---------+--------+

Characteristics:
- PersonID=1,3 are students (exist in STUDENT table)
- PersonID=2 is faculty (exists in FACULTY table)
- Partial specialization: Person 1,2,3 exist; 2 not in STUDENT, 1,3 not in FACULTY
- Overlapping possible: Person can exist in multiple subclass tables
```

---

## 13.3 Strategy 2: Subclass Tables Only (Total Specialization)

**When to use:**
- **Total specialization** guaranteed (every superclass entity in some subclass)
- Subclasses have **many attributes**
- No need to store generic superclass records

**Mapping:**
- No table for superclass
- One table for each subclass
- Subclass table includes:
  - All superclass attributes
  - All own attributes
- Subclass PK = primary key of superclass (not FK)

**Example:**

```
ER Model:
Same as before but with TOTAL specialization
(every PERSON must be STUDENT, FACULTY, or STAFF)

Relational Schema:
(NO PERSON table)

STUDENT(PersonID PK, Name, Age, RollNo, CGPA, MajorDept)
        ◁─ Includes all PERSON attributes (Name, Age)
        ◁─ Plus own attributes

FACULTY(PersonID PK, Name, Age, FacultyID, Department, Salary)
        ◁─ Includes all PERSON attributes
        ◁─ Plus own attributes

STAFF(PersonID PK, Name, Age, StaffID, Position, Salary)
      ◁─ Includes all PERSON attributes
      ◁─ Plus own attributes

Example Data:
STUDENT:
+----------+-------+-----+--------+------+-----------+
| PersonID | Name  | Age | RollNo | CGPA | MajorDept |
+----------+-------+-----+--------+------+-----------+
| 1        | Alice | 20  | S001   | 3.8  | CS        |
| 3        | Carol | 25  | S002   | 3.6  | Math      |
+----------+-------+-----+--------+------+-----------+

FACULTY:
+----------+-----+-----+-----------+--------+--------+
| PersonID | Name| Age | FacultyID | Dept   | Salary |
+----------+-----+-----+-----------+--------+--------+
| 2        | Bob | 45  | F001      | CS     | 80000  |
+----------+-----+-----+-----------+--------+--------+

STAFF:
+----------+-----+-----+--------+---------+--------+
| PersonID | Name| Age | StaffID| Position| Salary |
+----------+-----+-----+--------+---------+--------+
(empty or other staff)
+----------+-----+-----+--------+---------+--------+

Redundancy: Name and Age repeated in each subclass table
Trade-off: Avoids join with superclass table, simpler queries
```

---

## 13.4 Strategy 3: Single Table with Type Attribute (Disjoint)

**When to use:**
- Subclasses have **few additional attributes**
- **Disjoint specialization** (mutually exclusive)
- Want to avoid joining multiple tables

**Mapping:**
- Single table for both superclass and subclasses
- Add discriminator/type attribute
- Subclass-specific attributes nullable

**Example:**

```
ER Model:
Same PERSON with disjoint specialization
(no overlapping: PERSON is STUDENT OR FACULTY OR STAFF, but not multiple)

Relational Schema:
PERSON(PersonID PK, Name, Age, PersonType, RollNo, CGPA, FacultyID, Dept,  
       StaffID, Position, Salary)
                   ◁─ Discriminator

PersonType values: 'STUDENT', 'FACULTY', 'STAFF'

Example Data:
+----------+-------+-----+----------+--------+------+-----------+-----------+--------+-----------+--------+
| PersonID | Name  | Age | PersonType| RollNo| CGPA| FacultyID | Dept | StaffID | Position| Salary |
+----------+-------+-----+----------+--------+------+-----------+-----------+--------+-----------+--------+
| 1        | Alice | 20  | STUDENT  | S001  | 3.8 | NULL      | NULL | NULL    | NULL    | NULL   |
| 2        | Bob   | 45  | FACULTY  | NULL  |NULL | F001      | CS   | NULL    | NULL    | 80000  |
| 3        | Carol | 25  | STUDENT  | S002  | 3.6 | NULL      | NULL | NULL    | NULL    | NULL   |
+----------+-------+-----+----------+--------+------+-----------+-----------+--------+-----------+--------+

Characteristics:
- All in one table
- PersonType indicates subclass
- Subclass attributes are NULL for other types
- Disjoint: PersonType has single value
```

---

## 13.5 Strategy 4: Single Table with Multiple Boolean Flags (Overlapping)

**When to use:**
- Subclasses have **few additional attributes**
- **Overlapping specialization** (can be in multiple subclasses)
- Need to identify multiple roles per entity

**Mapping:**
- Single table for superclass and all subclasses
- Add boolean flag columns for each subclass
- Subclass attributes nullable

**Example:**

```
ER Model:
PERSON with overlapping specialization
(PERSON can be STUDENT, EMPLOYEE, VOLUNTEER, or combinations)

Relational Schema:
PERSON(PersonID PK, Name, Age, IsStudent, IsEmployee, IsVolunteer,  
       StudentRollNo, StudentCGPA, EmployeeID, EmployeeSalary, VolunteerHours)

IsStudent, IsEmployee, IsVolunteer: TRUE/FALSE or 0/1

Example Data:
+----------+-------+-----+-----------+------------+-----------+----------+--------+----------+-----------+-----------+
| PersonID | Name  | Age | IsStudent | IsEmployee | IsVolunteer| RollNo  | CGPA   | EmpID    | Salary    | VolHours  |
+----------+-------+-----+-----------+------------+-----------+----------+--------+----------+-----------+-----------+
| 1        | Alice | 20  | TRUE      | TRUE       | FALSE      | S001    | 3.8    | E001     | 25000     | NULL      |
| 2        | Bob   | 45  | FALSE     | TRUE       | TRUE       | NULL    | NULL   | E002     | 60000     | 5         |
| 3        | Carol | 25  | TRUE      | FALSE      | TRUE       | S002    | 3.6    | NULL     | NULL      | 10        |
+----------+-------+-----+-----------+------------+-----------+----------+--------+----------+-----------+-----------+

Characteristics:
- Alice: STUDENT and EMPLOYEE (both TRUE)
- Bob: EMPLOYEE and VOLUNTEER (both TRUE)
- Carol: STUDENT and VOLUNTEER (both TRUE)
- Overlapping: Multiple flags can be TRUE
- Nullable fields for attributes of roles person doesn't have
```

---

## 13.6 Decision Matrix: Which Strategy to Use?

| Situation | Strategy | Rationale |
|-----------|----------|-----------|
| **Many subclass-specific attrs + total + disjoint** | 2 (subclass-only) | Best normalization; no NULL columns |
| **Many subclass-specific attrs + partial + disjoint** | 1 (super + sub) | Handles orphan superclass records |
| **Many subclass-specific attrs + total + overlapping** | 1 (super + sub) | Handles multiple subclass membership |
| **Few subclass-specific attrs + total + disjoint** | 3 (type attribute) | Avoids joins; simple queries |
| **Few subclass-specific attrs + partial + disjoint** | 3 (type attribute) | Type=NULL for orphans |
| **Few subclass-specific attrs + overlapping** | 4 (boolean flags) | Indicates multiple roles |
| **Complex multi-level hierarchy** | 1 (super + sub) | Most flexible |

---

# 14. DATABASE-LEVEL CONSTRAINTS & TRIGGERS

## 14.1 Check Constraint

**Purpose:** Validate that a column or set of columns satisfies a condition before INSERT/UPDATE.

**Syntax:**
```sql
ALTER TABLE TableName
ADD CONSTRAINT ConstraintName CHECK (condition);

OR

CREATE TABLE TableName (
  col1 INT CHECK (col1 > 0),
  col2 VARCHAR(50) CHECK (col2 LIKE '[A-Z]%'),
  age INT CHECK (age >= 18 AND age <= 120)
);
```

**Examples:**

```sql
-- Minimum balance
ALTER TABLE ACCOUNTS
ADD CONSTRAINT CKMinimumBalance CHECK (balance >= 100);

-- Only specific values
ALTER TABLE STUDENT
ADD CONSTRAINT CKGender CHECK (gender IN ('M', 'F', 'O'));

-- Date constraints
ALTER TABLE PROJECT
ADD CONSTRAINT CKDates CHECK (EndDate >= StartDate);

-- Age range
ALTER TABLE EMPLOYEE
ADD CONSTRAINT CKAge CHECK (Age >= 18 AND Age <= 65);
```

**Violation Scenario:**
```sql
INSERT INTO ACCOUNTS(AccountNo, Balance) VALUES(1001, 50);
← ERROR: 50 < 100 violates CKMinimumBalance constraint

INSERT INTO STUDENT VALUES(101, 'Alice', 'X');
← ERROR: 'X' not in ('M','F','O') violates CKGender constraint
```

---

## 14.2 Triggers

**Definition:** Stored SQL code that executes **automatically** before or after an INSERT, UPDATE, or DELETE operation.

**Purpose:**
- Enforce complex business rules
- Maintain derived attributes automatically
- Prevent invalid state transitions
- Log changes for audit trails
- Maintain referential integrity beyond foreign keys

**Basic Syntax:**
```sql
CREATE TRIGGER TriggerName
AFTER/BEFORE INSERT/UPDATE/DELETE ON TableName
FOR EACH ROW
BEGIN
  -- SQL statements to execute
  IF (condition) THEN
    INSERT/UPDATE/DELETE statement
  END IF;
END;
```

### Example 1: Update Derived Attribute

```sql
CREATE TRIGGER UpdateGPA
AFTER UPDATE OF marks ON GRADES
FOR EACH ROW
BEGIN
  UPDATE STUDENT
  SET CGPA = (SELECT AVG(marks) FROM GRADES WHERE StudentID = NEW.StudentID)
  WHERE StudentID = NEW.StudentID;
END;

Scenario:
- When a student's grade is updated, automatically recalculate CGPA
- No manual update needed
```

### Example 2: Audit Log

```sql
CREATE TRIGGER AuditEmployeeChanges
AFTER UPDATE ON EMPLOYEE
FOR EACH ROW
BEGIN
  INSERT INTO EMPLOYEE_LOG(EmpID, OldSalary, NewSalary, ChangeDate)
  VALUES(NEW.EmpID, OLD.Salary, NEW.Salary, NOW());
END;

Scenario:
- Every time an employee's salary changes, log the change
- Maintains history of all modifications
```

### Example 3: Prevent Invalid Transitions

```sql
CREATE TRIGGER PreventPastDateBooking
BEFORE INSERT ON BOOKING
FOR EACH ROW
BEGIN
  IF NEW.BookingDate < CURDATE() THEN
    RAISE EXCEPTION 'Cannot book in the past';
  END IF;
END;

Scenario:
- Prevent booking dates earlier than today
- Enforce business rule at database level
```

### Example 4: Cascading Updates

```sql
CREATE TRIGGER UpdateDependents
AFTER UPDATE OF EmpID ON EMPLOYEE
FOR EACH ROW
BEGIN
  UPDATE DEPENDENT
  SET EmpID = NEW.EmpID
  WHERE EmpID = OLD.EmpID;
END;

Scenario:
- When employee ID changes, update all dependent records
- Automatically maintain referential integrity
```

---

## 14.3 Assertion

**Definition:** Database-level constraint on one or more relations.

**Purpose:** Enforce complex constraints involving multiple tables.

**Syntax:**
```sql
CREATE ASSERTION AssertionName
CHECK (condition);
```

**Example:**

```sql
-- Maximum enrollment
CREATE ASSERTION MaxCourseEnrollment
CHECK (
  NOT EXISTS (
    SELECT CourseID FROM ENROLLMENT
    GROUP BY CourseID
    HAVING COUNT(*) > 100
  )
);

Meaning: No course can have more than 100 enrolled students
```

---

# 15. UPDATE OPERATIONS & CONSTRAINT VIOLATIONS

## 15.1 Basic Update Operations

Three fundamental operations modify database state:

### INSERT
Adds new tuple to relation.

**Possible violations:**
- **Domain constraint**: Value doesn't match attribute domain
- **Key constraint**: Duplicate key value
- **Entity integrity**: NULL in primary key
- **Referential integrity**: FK references non-existent PK

```sql
INSERT INTO EMPLOYEE VALUES(101, 'Alice', 'invalid_salary', 10);
         ↑                   ↑      ↑        ↑
      Violates             domain  key      referential
                         constraint constraint
```

### DELETE
Removes tuple from relation.

**Possible violations:**
- **Referential integrity**: Deleting parent record that is referenced by child

```sql
DELETE FROM DEPARTMENT WHERE DeptID = 10;
Violation: EMPLOYEE table has records with DeptID = 10
          → Orphan records would be created
```

### UPDATE
Modifies attribute values in existing tuple.

**Possible violations:**
- **Domain constraint**: Invalid value type/range
- **Not NULL constraint**: Setting PK or constrained column to NULL
- **Key constraint**: Duplicate key after update
- **Referential integrity**: Changing FK to non-existent PK

```sql
UPDATE EMPLOYEE
SET Salary = 'invalid'        ← Domain violation
WHERE EmpID = 101;

UPDATE EMPLOYEE
SET EmpID = NULL              ← Entity integrity violation (EmpID is PK)
WHERE EmpID = 101;

UPDATE EMPLOYEE
SET DeptID = 999              ← Referential integrity violation
WHERE EmpID = 101;            (DeptID 999 doesn't exist)
```

---

## 15.2 Handling Constraint Violations

When a violating operation is attempted, DBMS takes one of several actions:

### RESTRICT / NO ACTION (Default)
Reject the operation that causes violation.

```sql
DELETE FROM DEPARTMENT WHERE DeptID = 10;
↓ (if employee exists with DeptID=10)
ERROR: Cannot delete; referential integrity violated
(Entire DELETE operation is canceled)
```

### CASCADE
Propagate changes to dependent records.

```sql
-- Define constraint:
CREATE TABLE EMPLOYEE (
  EmpID INT PRIMARY KEY,
  Name VARCHAR(50),
  DeptID INT FOREIGN KEY REFERENCES DEPARTMENT(DeptID)
    ON DELETE CASCADE
);

-- Execute:
DELETE FROM DEPARTMENT WHERE DeptID = 10;
↓
DBMS also deletes all EMPLOYEE records with DeptID = 10
(Cascades the deletion down)
```

### SET NULL
Set FK to NULL in dependent records.

```sql
CREATE TABLE EMPLOYEE (
  ...
  DeptID INT FOREIGN KEY REFERENCES DEPARTMENT(DeptID)
    ON DELETE SET NULL
);

DELETE FROM DEPARTMENT WHERE DeptID = 10;
↓
DBMS sets DeptID = NULL for all EMPLOYEE records with DeptID = 10
(Employees exist but department is unknown)
```

### SET DEFAULT
Set FK to default value in dependent records.

```sql
CREATE TABLE EMPLOYEE (
  ...
  DeptID INT DEFAULT 1 FOREIGN KEY REFERENCES DEPARTMENT(DeptID)
    ON DELETE SET DEFAULT
);

DELETE FROM DEPARTMENT WHERE DeptID = 10;
↓
DBMS sets DeptID = 1 (default) for all EMPLOYEE records with DeptID = 10
```

---

## 15.3 Constraint Violation Example Scenarios

### Scenario 1: INSERT Violation

```sql
EMPLOYEE table:
+-------+-------+----------+-------+
| EmpID | Name  | Salary   | DeptID|
+-------+-------+----------+-------+
| 1     | Alice | 50000    | 10    |
+-------+-------+----------+-------+

DEPARTMENT table:
+-------+--------+
| DeptID| DeptNam|
+-------+--------+
| 10    | Sales  |
| 20    | IT     |
+-------+--------+

Query: INSERT INTO EMPLOYEE VALUES(1, 'Bob', 55000, 10);
Error: Duplicate EmpID = 1 (violates key constraint)

Query: INSERT INTO EMPLOYEE VALUES(2, 'Carol', -5000, 10);
Error: Salary -5000 (violates domain constraint)

Query: INSERT INTO EMPLOYEE VALUES(2, 'Dave', 60000, 99);
Error: DeptID 99 doesn't exist (violates referential integrity)
```

### Scenario 2: DELETE Violation

```sql
Query: DELETE FROM DEPARTMENT WHERE DeptID = 10;

Option 1 (RESTRICT):
Error: Cannot delete; EMPLOYEE records exist with DeptID = 10

Option 2 (CASCADE):
Success: Deletes department AND all employees in that department

Option 3 (SET NULL):
Success: Deletes department; EMPLOYEE records have DeptID = NULL

Option 4 (SET DEFAULT):
Success: Deletes department; EMPLOYEE records have DeptID = 1 (default)
```

### Scenario 3: UPDATE Violation

```sql
Query: UPDATE EMPLOYEE SET EmpID = 1 WHERE EmpID = 2;
Error: Duplicate EmpID = 1 (key constraint violation)

Query: UPDATE EMPLOYEE SET Salary = 'abc' WHERE EmpID = 1;
Error: 'abc' is not a valid salary type (domain violation)

Query: UPDATE EMPLOYEE SET DeptID = 99 WHERE EmpID = 1;
Error: DeptID 99 doesn't exist (referential integrity violation)
```

---

## 15.4 Transaction Concepts (Brief)

Update operations often grouped into **transactions** (all-or-nothing units):

```sql
BEGIN TRANSACTION
  INSERT INTO ACCOUNT VALUES(100, 'Savings', 1000);
  INSERT INTO CUSTOMER VALUES(1, 'Alice', 100);
  -- If any INSERT fails, ENTIRE transaction is rolled back
  -- Both statements succeed or both fail (atomicity)
COMMIT;
```

---

# SUMMARY & KEY TAKEAWAYS

| Topic | Key Concept |
|-------|------------|
| **Relational Model** | Data in tables; tuples (rows), attributes (columns); simple, flexible, standardized |
| **Keys** | Super key, Candidate key, Primary key, Foreign key; all enforce uniqueness and relationships |
| **Integrity Constraints** | Domain, Key, Entity Integrity, Referential Integrity; protect data quality |
| **NULLs** | Unknown/missing values; not equal to 0 or empty string; test with IS NULL / IS NOT NULL |
| **ER Model** | Conceptual design tool; entities, attributes, relationships; bridges real-world to database |
| **Attributes** | Simple, Composite, Multivalued, Derived, Key; describe entity properties |
| **Relationships** | Unary, Binary, N-ary; Cardinality (1:1, 1:N, M:N); Participation (total/partial) |
| **Structural Constraints** | Combine cardinality + participation; guide mapping decisions |
| **Weak Entities** | No own key; depend on owner; partial key + owner PK = full key |
| **ER to Relational** | Systematic mapping: strong entities → tables; weak → includes owner PK; 1:N → FK on N-side; M:N → junction table |
| **EER** | Extends ER with specialization/generalization; four mapping strategies depending on attributes & constraints |
| **Constraints & Triggers** | Check constraints, triggers, assertions enforce complex rules at database level |
| **Update Operations** | INSERT, DELETE, UPDATE; can violate constraints; DBMS responds with RESTRICT, CASCADE, SET NULL, SET DEFAULT |

---

# QUICK REVISION SHEET

**Keys:**
- Super key: any set of unique attributes
- Candidate key: minimal super key
- Primary key: chosen candidate key (not null)
- Foreign key: references PK in another table
- Composite key: multiple attributes forming key

**Integrity:**
- Domain: valid value ranges/types
- Entity: PK ≠ NULL
- Referential: FK = PK or NULL
- Key: uniqueness

**NULL Handling:**
- IS NULL / IS NOT NULL for testing
- NVL/COALESCE for substitution
- NOT NULL constraint to forbid NULLs

**ER Components:**
- Entity: real-world object (rectangle)
- Attribute: property (oval); types: simple, composite, multivalued, derived, key
- Relationship: association (diamond); degree: unary, binary, n-ary
- Cardinality: 1:1, 1:N, M:N
- Participation: total (double line), partial (single line)
- Weak entity: double rectangle; no own key

**ER Mapping:**
- Strong entity → table
- Weak entity → table with owner PK as FK
- 1:N → FK on N-side
- 1:1 → FK on one side (or merge)
- M:N → separate junction table
- Multivalued → separate table

**EER Specialization Mapping:**
1. Superclass + subclass tables (most flexible)
2. Subclass-only (total specialization)
3. Type attribute (disjoint, few attributes)
4. Boolean flags (overlapping, few attributes)

---

