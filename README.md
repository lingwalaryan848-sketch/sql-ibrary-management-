# sql-ibrary-management-
Project Overview

The Library Management System is a SQL-based database project designed to manage and organize library operations efficiently. The system maintains records of books, library members, employees, issued books, returned books, and branch information. It demonstrates database design concepts, table relationships, primary and foreign key constraints, and SQL query execution.

Objectives
Create and manage a relational database for a library.
Store and maintain book, member, employee, and branch records.
Track book issue and return transactions.
Implement primary and foreign key relationships.
Perform CRUD (Create, Read, Update, Delete) operations using SQL.
Execute analytical and reporting queries on library data.
Database Structure
Branch Table

Stores information about library branches including branch ID, manager ID, address, and contact details.

Employees Table

Maintains employee records such as employee ID, name, position, salary, and assigned branch.

Members Table

Contains member information including member ID, member name, address, and registration date.

Books Table

Stores book details such as ISBN, title, category, rental price, availability status, author, and publisher.

Issue Status Table

Tracks issued books by recording issue ID, member ID, book details, issue date, and employee responsible for issuing the book.

Return Status Table

Maintains records of returned books including return ID, issued ID, return date, and returned book details.

Key Features
Book inventory management
Member registration and management
Employee management
Book issue tracking
Book return tracking
Relational database design using foreign keys
Data retrieval using SQL queries
Library transaction monitoring
Technologies Used
MySQL
MySQL Workbench
SQL (DDL, DML, DQL)
Relational Database Management System (RDBMS)
Learning Outcomes
Database schema design
Primary and foreign key implementation
Table creation and normalization
Data insertion and manipulation
Query optimization and reporting
Real-world database project development

CREATE DATABASE library_db;
use library_db;
DROP TABLE IF EXISTS branch;
CREATE TABLE branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);


-- Create table "Employee"
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);


-- Create table "Members"
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE
);



-- Create table "Books"
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(10),
            author VARCHAR(30),
            publisher VARCHAR(30)
);



-- Create table "IssueStatus"
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);



-- Create table "ReturnStatus"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
SELECT * FROM books;
SELECT * FROM members;
SELECT * FROM employee;
SELECT * FROM issue_status;
SELECT * FROM return_status;
SELECT COUNT(*) FROM books;
SELECT COUNT(*) FROM members;
SELECT COUNT(*) FROM employee;
SELECT COUNT(*) FROM issue_status;
SELECT COUNT(*) FROM return_status;

-- Task 1. Create a New Book Record -- Task 1. Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"')"
insert into books ( isbn,book_title, category, retnal_prices , status , author , publisher)
values('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
select * From books;

-- task 2: Update an Existing Member's Address
update members
set member_address ='123-0ak st'
where member_id ='C106';
 -- Task 3: Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
delete from issued_status
where issued_id ='IS121';
-- Task 4: Retrieve All Books Issued by a Specific Employee 
-- Objective: Select all books issued by the employee with emp_id = 'E101'.
 SELECT *  from issued_status where issued_emp_id='E101';
 
 -- Task 5: List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.
 select issued_emp_id , count(*) from issued_status group by 1 having count(*)>1;
 
-- Task 6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
create table book_issued_cnt AS Select b.isbn , b.book_tittle, Count(ist.issued_id) As issue_count
from issued_status as ist 
Join Books as b
On ist.issued_book_isbn =b.isbn 
group by b.isbn ,b.book_tittle;

-- Task 7. Retrieve All Books in a Specific Category:
SELECT 
    *
FROM
    books
WHERE
    category = 'Classic';
   -- Task 8: Find Total Rental Income by Category:
   select B.category,
   SUM(B.rental_price),
   Count(*)
from
issued_status as ist
join books as b 
on b.isbn =ist.issued_book_isbn
group By 1;

-- task 10 .List Employees with Their Branch Manager's Name and their branch details:
SELECT 
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.*,
    e2.emp_name as manager
FROM employees as e1
JOIN 
branch as b
ON e1.branch_id = b.branch_id    
JOIN
employees as e2
ON e2.emp_id = b;

-- Task 11.Create a Table of Books with Rental Price Above a Certain Threshold:

CREATE TABLE expensive_books AS
SELECT * FROM books
WHERE rental_price > 7.00;
