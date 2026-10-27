# Database Management System (DBMS)
### MySQL Lab Work & Assignments

![MySQL](https://img.shields.io/badge/MySQL-8.0-blue?logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/Language-SQL-orange)
![Academic](https://img.shields.io/badge/Type-Academic%20Lab%20Work-green)
![Status](https://img.shields.io/badge/Status-Ongoing-brightgreen)

## 📌 About This Repository

Welcome to my **Database Management System (DBMS)** repository!

This repository contains my DBMS lab practicals, SQL queries, assignments, and classroom exercises completed as part of my Integrated MCA course at **IIPS, Devi Ahilya Vishwavidyalaya (DAVV), Indore**.

The main purpose of this repository is to organize my practical work, improve my understanding of SQL, and keep a record of what I learn during DBMS lab sessions.

## 👨‍🎓 Student Information

| Details | Information |
|---|---|
| **Name** | Ankesh Diwakar |
| **Course** | Integrated MCA |
| **Department** | International Institute of Professional Studies (IIPS) |
| **University** | Devi Ahilya Vishwavidyalaya (DAVV), Indore |
| **Subject** | Database Management System |
| **Roll Number** | IC-2K25-13 |
| **Database** | MySQL |

## 🎯 Learning Objectives

Through these practicals, I am learning how to:

- Understand the fundamentals of DBMS and relational databases.
- Create and manage databases using MySQL.
- Create tables with appropriate data types.
- Insert, update, delete, and retrieve records.
- Apply SQL constraints to maintain data integrity.
- Use SQL operators, functions, and conditions.
- Retrieve information using different SQL queries.
- Work with joins, subqueries, and aggregate functions.
- Practice SQL using sample databases and HR-related data.
- Improve my database design and query-writing skills.

## 📚 Topics Covered

### 1. DBMS Fundamentals
- Introduction to DBMS
- Relational Database Management System (RDBMS)
- Databases, tables, rows, and columns
- Primary keys and foreign keys

### 2. SQL Basics
- CREATE DATABASE
- CREATE TABLE
- INSERT INTO
- SELECT
- UPDATE
- DELETE
- ALTER TABLE
- DROP TABLE

### 3. SQL Constraints
- PRIMARY KEY
- FOREIGN KEY
- NOT NULL
- UNIQUE
- DEFAULT
- CHECK

### 4. SQL Queries
- WHERE clause
- ORDER BY
- GROUP BY
- HAVING clause
- DISTINCT
- Aggregate functions
- SQL operators

### 5. Advanced SQL Practice
- Joins
- Subqueries
- Nested queries
- AUTO_INCREMENT
- Date and time functions
- HR database queries

## 📂 Repository Contents

The repository includes the following lab work and assignments:

| Files / Folders | Description |
|---|---|
| `Assignment_1_mysql` | DBMS Assignment 1 |
| `Assignment_2_dbms_mysql_lab_05_work` | Assignment 2 and MySQL lab work |
| `Assignment-3` | DBMS Assignment 3 |
| `Assignment-4` | DBMS Assignment 4 |
| `Assignment-5` | DBMS Assignment 5 |
| `lab_1_dbms_mysql` | MySQL Lab 1 |
| `lab_2_Create_table_mysql` | Table creation practice |
| `lab_4_online_work_human_HR_data.mysql` | HR database exercises |
| `lab_5(24)_work.mysql` | MySQL Lab 5 |
| `Lab_6_online25dt_table_createbypdb.mysql` | Table creation exercises |
| `lab7_31_sep_labwork_dbms` | DBMS Lab 7 |
| `lab8_1sep_onlineclass_dbms` | DBMS Lab 8 |
| `lab_9_online(15sep)work` | DBMS Lab 9 |
| `constraints.dbms` | SQL constraints practice |
| `AUTO_INCREMENT & Dates` | Auto-increment and date-related exercises |
| `Lab_work_5_oct` | October lab work |

Additional SQL practice files are also included in the repository.

## 🛠️ Tools & Technologies

| Technology | Purpose |
|---|---|
| MySQL 8.0 | Database Management System |
| SQL | Database queries and operations |
| MySQL Command Line / Workbench | Writing and executing SQL queries |
| Git | Version control |
| GitHub | Storing and organizing practical work |

## 💻 Example SQL Queries

The following examples demonstrate some basic SQL operations.

### Create a Database

```sql
CREATE DATABASE college_db;

USE college_db;
```

### Create a Table

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    course VARCHAR(30),
    marks INT
);
```

### Insert Records

```sql
INSERT INTO students
VALUES
(1, 'Ankesh', 'MCA', 85),
(2, 'Rahul', 'MCA', 78),
(3, 'Aman', 'MCA', 90);
```

### Display Records

```sql
SELECT * FROM students;
```

### Apply a Condition

```sql
SELECT *
FROM students
WHERE marks > 80;
```

### Find Average Marks

```sql
SELECT AVG(marks) AS average_marks
FROM students;
```

These are simple examples for understanding SQL syntax. The repository contains additional lab exercises and assignments.

## ▶️ How to Run SQL Files

1. Install MySQL on your computer.
2. Open MySQL Workbench or the MySQL Command Line.
3. Open the required SQL file from this repository.
4. Create or select the database required for that exercise.
5. Execute the SQL queries.
6. Check the results and understand the operations.

**Note:** Some practical files may require specific tables or sample data to be created before executing the queries.

## 📈 Learning Progress

This repository is part of my academic learning journey.

I regularly use it to:

- Save classroom practicals and assignments.
- Revise SQL concepts.
- Practice database queries.
- Understand mistakes and improve query-writing skills.
- Track my progress in DBMS.

I will continue updating this repository as I complete more practicals and learn new concepts.

## 👨‍💻 About Me

**Ankesh Diwakar**

Integrated MCA Student  
International Institute of Professional Studies (IIPS)  
Devi Ahilya Vishwavidyalaya, Indore

I am a computer science student interested in programming, database management, and software development. I enjoy learning technical concepts through practical implementation and working on projects that help me improve my skills.

### 🔗 Connect With Me

- **GitHub:** [ankesh825](https://github.com/ankesh825)
- **LinkedIn:** [Ankesh Diwakar](https://www.linkedin.com/in/ankesh-diwakar-30b908382/)

---

### 📝 Note

This repository is maintained for **educational and academic purposes**.

All practicals and assignments are part of my learning and practice in Database Management Systems.

**Thank you for visiting my repository!**
