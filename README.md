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

### Create
Inserted sample records into the books table.

```sql
INSERT INTO books(isbn, book_title, category)
VALUES ('978-1-60129-456-2','To Kill a Mockingbird','Classic');
