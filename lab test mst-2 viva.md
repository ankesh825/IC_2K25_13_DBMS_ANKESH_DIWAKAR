# College Student Management System (MySQL)

A simple database design project for managing college students, classes, departments, and login access. The main idea is to keep student records connected through keys and give different permissions to admins and students.

> **Project status:** Database design and SQL demo. This repository does **not** include a working website or login backend yet.

## 1. Problem statement

A college stores details like student ID, name, class, address, and email. Students may receive a college username and password to access their own information. An admin should be able to add student records, update details, and enable or disable accounts.

Instead of keeping everything in one large table, I have divided the information into related tables in MySQL.

## 2. Objectives

- Store student information in an organized way.
- Connect students with their class and department.
- Keep login accounts separate from personal information.
- Define roles such as `ADMIN`, `STUDENT`, and `FACULTY`.
- Assign permissions to roles.
- Keep disabled accounts in the database without allowing login.
- Practice primary keys, foreign keys, normalization, and JOINs.

## 3. Concepts used

| Concept | Purpose in this project |
| --- | --- |
| RDBMS | Store related data in separate tables |
| Primary Key | Identify each record uniquely |
| Foreign Key | Connect records between tables |
| Composite Key | Avoid duplicate user-role or role-permission pairs |
| `UNIQUE` / `NOT NULL` | Maintain data quality |
| JOIN | Retrieve data from multiple tables |
| Normalization | Reduce unnecessary repetition |
| CRUD | Add, read, update, and delete data |
| Authentication | Check who is logging in (future backend) |
| Authorization / RBAC | Decide what each user is allowed to do |
| Audit log | Record important changes |

## 4. Database structure

The database is named `college_management`.

| Table | Stores |
| --- | --- |
| `departments` | Department names |
| `classes` | Class names and their departments |
| `students` | Student details and class assignment |
| `users` | Usernames, password hashes, and account status |
| `roles` | Admin, student, and faculty roles |
| `user_roles` | Roles assigned to users |
| `permissions` | Actions that can be allowed |
| `role_permissions` | Permissions given to roles |
| `audit_logs` | History of admin actions |

### Relationship overview

```text
departments (1) ---- (many) classes
classes     (1) ---- (many) students
students    (1) ---- (0 or 1) users
users       (many) ---- (many) roles        [through user_roles]
roles       (many) ---- (many) permissions  [through role_permissions]
users       (1) ---- (many) audit_logs      [as the acting user]
```

The `users.student_id` field is nullable because an admin or faculty account does not always have a student record.

## 5. MySQL code

The following script can be run in **MySQL 8**. It creates the tables and inserts a small amount of sample data.

```sql
CREATE DATABASE IF NOT EXISTS college_management;
USE college_management;

-- Department details
CREATE TABLE departments (
    dept_id INT AUTO_INCREMENT PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL UNIQUE
);

-- Each class belongs to one department
CREATE TABLE classes (
    class_id INT AUTO_INCREMENT PRIMARY KEY,
    class_name VARCHAR(100) NOT NULL,
    dept_id INT NOT NULL,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- Each student is assigned to one class
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    class_id INT NOT NULL,
    address VARCHAR(200),
    email VARCHAR(100) NOT NULL UNIQUE,
    FOREIGN KEY (class_id) REFERENCES classes(class_id)
);

-- Login accounts (password_hash is populated by a secure backend)
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT UNIQUE,
    username VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    status ENUM('active', 'disabled') NOT NULL DEFAULT 'active',
    FOREIGN KEY (student_id) REFERENCES students(student_id)
);

CREATE TABLE roles (
    role_id INT AUTO_INCREMENT PRIMARY KEY,
    role_name VARCHAR(50) NOT NULL UNIQUE
);

-- A user may have one or more roles
CREATE TABLE user_roles (
    user_id INT NOT NULL,
    role_id INT NOT NULL,
    PRIMARY KEY (user_id, role_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (role_id) REFERENCES roles(role_id)
);

CREATE TABLE permissions (
    permission_id INT AUTO_INCREMENT PRIMARY KEY,
    permission_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE role_permissions (
    role_id INT NOT NULL,
    permission_id INT NOT NULL,
    PRIMARY KEY (role_id, permission_id),
    FOREIGN KEY (role_id) REFERENCES roles(role_id),
    FOREIGN KEY (permission_id) REFERENCES permissions(permission_id)
);

-- Log important actions performed by a user
CREATE TABLE audit_logs (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    actor_user_id INT NOT NULL,
    action_name VARCHAR(100) NOT NULL,
    target_user_id INT,
    action_time TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (actor_user_id) REFERENCES users(user_id),
    FOREIGN KEY (target_user_id) REFERENCES users(user_id)
);

-- Sample academic data
INSERT INTO departments (dept_name)
VALUES ('Computer Science'), ('Management');

INSERT INTO classes (class_name, dept_id)
VALUES ('MCA 2nd Year', 1), ('BBA 1st Year', 2);

INSERT INTO students (full_name, class_id, address, email)
VALUES
('Rahul Sharma', 1, 'Indore', 'rahul@example.com'),
('Aman Verma', 2, 'Bhopal', 'aman@example.com');

INSERT INTO roles (role_name)
VALUES ('ADMIN'), ('STUDENT'), ('FACULTY');

INSERT INTO permissions (permission_name)
VALUES
('VIEW_OWN_PROFILE'),
('VIEW_ALL_STUDENTS'),
('ADD_STUDENT'),
('UPDATE_STUDENT'),
('DISABLE_ACCOUNT'),
('ENABLE_ACCOUNT');

-- Admin gets all permissions
INSERT INTO role_permissions (role_id, permission_id)
SELECT 1, permission_id FROM permissions;

-- Student can only view their own profile
INSERT INTO role_permissions (role_id, permission_id)
SELECT 2, permission_id
FROM permissions
WHERE permission_name = 'VIEW_OWN_PROFILE';
```

**Note:** I have intentionally not inserted example passwords or password hashes into the `users` table. In a real application, the backend should create a secure password hash before inserting an account. Never store a password in plain text.

## 6. SQL queries and JOINs

### A. View students with their class

```sql
SELECT s.student_id, s.full_name, c.class_name
FROM students AS s
INNER JOIN classes AS c ON s.class_id = c.class_id;
```

`INNER JOIN` shows students who have a matching class record.

### B. View students, classes, and departments together

```sql
SELECT s.student_id, s.full_name, c.class_name, d.dept_name
FROM students AS s
JOIN classes AS c ON s.class_id = c.class_id
JOIN departments AS d ON c.dept_id = d.dept_id;
```

This is an example of joining three related tables.

### C. Show all students, including students without a user account

```sql
SELECT s.student_id, s.full_name, u.username, u.status
FROM students AS s
LEFT JOIN users AS u ON s.student_id = u.student_id;
```

`LEFT JOIN` is useful here because a student record may exist before a login account is created.

### D. Find permissions assigned to a role

```sql
SELECT r.role_name, p.permission_name
FROM roles AS r
JOIN role_permissions AS rp ON r.role_id = rp.role_id
JOIN permissions AS p ON rp.permission_id = p.permission_id
WHERE r.role_name = 'ADMIN';
```

### E. Add a student (admin operation)

```sql
INSERT INTO students (full_name, class_id, address, email)
VALUES ('Neha Patel', 1, 'Indore', 'neha@example.com');
```

### F. Update a student's address (admin operation)

```sql
UPDATE students
SET address = 'Ujjain'
WHERE student_id = 1;
```

### G. Disable or enable an account (admin operation)

```sql
-- Run only after a real account with user_id = 1 exists
UPDATE users SET status = 'disabled' WHERE user_id = 1;
UPDATE users SET status = 'active' WHERE user_id = 1;
```

These are **example SQL operations**, not security checks. In a working app, the backend must verify the logged-in user's permissions **before** running them. Do not let a student execute these statements directly.

## 7. Login and admin-control logic

```text
User opens college portal
         |
Enters username + password
         |
Backend finds user account
         |
Verifies stored password hash
         |
Checks account status
         |
  +------+------+
  |             |
Disabled      Active
  |             |
Deny login    Load roles and permissions
                |
       +--------+--------+
       |                 |
      Admin            Student
       |                 |
Manage allowed      View only their
student records     own profile
```

For example, to check whether a user has the `DISABLE_ACCOUNT` permission, a backend can run a parameterized query like this:

```sql
SELECT 1
FROM users AS u
JOIN user_roles AS ur ON u.user_id = ur.user_id
JOIN role_permissions AS rp ON ur.role_id = rp.role_id
JOIN permissions AS p ON rp.permission_id = p.permission_id
WHERE u.user_id = ?
  AND u.status = 'active'
  AND p.permission_name = 'DISABLE_ACCOUNT'
LIMIT 1;
```

Here `?` represents a bound parameter; the exact placeholder depends on the database library. This query alone is not a complete login system. A real backend must also manage sessions securely, use parameterized queries, and prevent students from accessing someone else's records by changing an ID in a URL.

## 8. Keys used

- **Primary Key:** `students.student_id`, `classes.class_id`, etc.
- **Foreign Key:** `students.class_id` references `classes.class_id`.
- **Unique Key:** `students.email` and `users.username` cannot be repeated.
- **Composite Primary Key:** `(user_id, role_id)` in `user_roles`.

## 9. Normalization

I have separated student, class, and department details to avoid storing the same department name again for every student. The tables are designed around **Third Normal Form (3NF)** principles:

1. **1NF:** Each column stores a single value.
2. **2NF:** Non-key columns depend on the complete key.
3. **3NF:** Non-key columns should not depend on other non-key columns.

This makes updates easier and reduces repeated data.

## 10. Security points

- Store only **hashed passwords**, preferably using Argon2id or bcrypt in the backend.
- Check permissions on the **server**, not only by hiding buttons on a webpage.
- Use a database account with limited privileges for the application; do not use MySQL `root` for normal web requests.
- Keep database credentials in environment variables, never in GitHub.
- Use parameterized queries to avoid SQL injection.
- Disable login access without deleting academic records.
- Revoke active sessions when an account is disabled.
- Record sensitive admin changes in `audit_logs` from the backend, ideally in the same transaction as the change.

## 11. Tools required

- MySQL 8.0+
- MySQL Workbench or MySQL Command Line
- Optional for a future website: HTML, CSS, JavaScript, and Python Flask

### How to try the SQL

1. Open MySQL Workbench.
2. Create a new SQL tab.
3. Copy the script from **Section 5** and execute it once.
4. Run the SELECT queries from **Section 6**.
5. Use `SHOW TABLES;` and `DESCRIBE students;` to inspect the structure.

## 12. Future improvements

- Create a login page and backend authentication.
- Build separate admin and student dashboards.
- Add student search and class filters.
- Connect the database to a Flask application.
- Show admin action history using `audit_logs`.
- Add an ER diagram and screenshots from MySQL Workbench.

## 13. What I learned

While planning this project, I understood how multiple tables can be connected using primary and foreign keys. I also learned why student information and login details should be stored separately, how JOINs help retrieve related data, and why admin permissions must be checked before allowing changes.

---

**Academic DBMS project | MySQL | College Student Management System**
