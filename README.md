# 📚 Library Management System

## Project Overview

The Library Management System is a SQL-based database project designed to manage library operations efficiently. It stores information about books, members, employees, issued books, and returned books using relational database concepts.

## Objectives

- Manage book inventory
- Track issued and returned books
- Store member records
- Manage employee information
- Implement SQL relationships using foreign keys
- Perform CRUD operations

## Database Structure

### Branch Table
Stores branch information including branch ID, manager ID, address, and contact details.

### Employee Table
Maintains employee records such as employee ID, name, position, salary, and assigned branch.

### Members Table
Stores member details including registration information.

### Books Table
Contains book information such as ISBN, title, category, author, publisher, and rental price.

### Issue Status Table
Tracks books issued to members.

### Return Status Table
Maintains records of returned books.

## ER Diagram

![ER Diagram](library_erd.png)

## CRUD Operations

## Task 1: Create a New Book Record

**Objective:** Insert a new book into the `books` table.

```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES ('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');

SELECT * FROM books;
```

---

## Task 2: Update an Existing Member's Address

**Objective:** Update the address of member **C103**.

```sql
UPDATE members
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
```

---

## Task 3: Delete a Record from the Issued Status Table

**Objective:** Delete the record with `issued_id = 'IS121'`.

```sql
DELETE FROM issued_status
WHERE issued_id = 'IS121';
```

---

## Task 4: Retrieve All Books Issued by a Specific Employee

**Objective:** Display all books issued by employee **E101**.

```sql
SELECT *
FROM issued_status
WHERE issued_emp_id = 'E101';
```

---

## Task 5: List Members Who Have Issued More Than One Book

**Objective:** Find members who have issued more than one book.

```sql
SELECT
    issued_member_id,
    COUNT(*) AS books_issued
FROM issued_status
GROUP BY issued_member_id
HAVING COUNT(*) > 1;
```

---

# 3. CTAS (Create Table As Select)

## Task 6: Create a Summary Table

**Objective:** Create a summary table showing each book and the number of times it has been issued.

```sql
CREATE TABLE book_issued_cnt AS
SELECT
    b.isbn,
    b.book_title,
    COUNT(ist.issued_id) AS issue_count
FROM issued_status AS ist
JOIN books AS b
ON ist.issued_book_isbn = b.isbn
GROUP BY b.isbn, b.book_title;
```

---

# 4. Data Analysis & Findings

## Task 7: Retrieve All Books in a Specific Category

**Objective:** Display all books in the **Classic** category.

```sql
SELECT *
FROM books
WHERE category = 'Classic';
```

---

## Task 8: Find Total Rental Income by Category

**Objective:** Calculate total rental income generated from each book category.

```sql
SELECT
    b.category,
    SUM(b.rental_price) AS total_rental_income,
    COUNT(*) AS books_issued
FROM issued_status AS ist
JOIN books AS b
ON ist.issued_book_isbn = b.isbn
GROUP BY b.category;
```

---

## Task 9: List Members Registered in the Last 180 Days

**Objective:** Display members who registered within the last **180 days**.

```sql
SELECT *
FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL 180 DAY;
```

---

## Task 10: List Employees with Their Branch Manager and Branch Details

**Objective:** Display employee details along with their branch information and manager name.

```sql
SELECT
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.branch_id,
    b.branch_address,
    b.contact_no,
    e2.emp_name AS manager_name
FROM employees AS e1
JOIN branch AS b
ON e1.branch_id = b.branch_id
JOIN employees AS e2
ON e2.emp_id = b.manager_id;
```

> **Note:** If your table name is `employee` instead of `employees`, replace `employees` with `employee` in the above query.

---

## Task 11: Create a Table of Expensive Books

**Objective:** Create a new table containing books with a rental price greater than **7.00**.

```sql
CREATE TABLE expensive_books AS
SELECT *
FROM books
WHERE rental_price > 7.00;
```
