<h1>Library Management System – SQL Project</h1>

<p>
This project is a comprehensive SQL-based Library Management System designed
to demonstrate database design, relational modeling, CRUD operations,
and advanced SQL concepts using PostgreSQL.
</p>

<h2>Project Objective</h2>

<p>
The objective of this project is to design and manage a relational database
for a library system and perform operational and analytical queries.
</p>

<p style="font-size:14px;">
The project covers table creation, entity relationships, data manipulation,
report generation, and automation using stored procedures.
</p>

<h2>Tools and Technologies</h2>

<p>
PostgreSQL (pgAdmin) is used for database creation, relationship modeling,
query execution, and procedural logic.
</p>

<h2>Database Design</h2>

<p>
A new database is created in pgAdmin to store all library-related data.
</p>

<p style="font-size:14px;">
The schema is designed using multiple normalized tables representing branches,
employees, books, members, book issuance, and returns.
</p>

<h2>Table Creation</h2>

<p>
The following tables are created to support library operations.
</p>

<pre><code>
CREATE TABLE branch (
    branch_id VARCHAR(10) PRIMARY KEY,
    manager_id VARCHAR(10),
    branch_address VARCHAR(55),
    contact_no VARCHAR(10)
);

CREATE TABLE employees (
    emp_id VARCHAR(10) PRIMARY KEY,
    emp_name VARCHAR(25),
    position VARCHAR(15),
    salary INT,
    branch_id VARCHAR(25)
);

CREATE TABLE books (
    isbn VARCHAR(20) PRIMARY KEY,
    book_title VARCHAR(75),
    category VARCHAR(15),
    rental_price FLOAT,
    status VARCHAR(15),
    author VARCHAR(35),
    publisher VARCHAR(55)
);

CREATE TABLE members (
    member_id VARCHAR(10) PRIMARY KEY,
    member_name VARCHAR(25),
    member_address VARCHAR(75),
    reg_date DATE
);

CREATE TABLE issued_status (
    issued_id VARCHAR(10) PRIMARY KEY,
    issued_member_id VARCHAR(15),
    issued_book_name VARCHAR(75),
    issued_date DATE,
    issued_book_isbn VARCHAR(35),
    issued_emp_id VARCHAR(10)
);

CREATE TABLE return_status (
    return_id VARCHAR(10) PRIMARY KEY,
    issued_id VARCHAR(10),
    return_book_name VARCHAR(75),
    return_date DATE,
    return_book_isbn VARCHAR(35)
);
</code></pre>

<h2>Entity Relationship Diagram (ERD)</h2>

<p>
An Entity Relationship Diagram (ERD) is created using pgAdmin to define
relationships between tables.
</p>

<p style="font-size:14px;">
Foreign key constraints are added to enforce referential integrity
between members, books, employees, branches, and transaction tables.
</p>

<h2>Foreign Key Constraints</h2>

<pre><code>
ALTER TABLE issued_status
ADD CONSTRAINT fk_members
FOREIGN KEY (issued_member_id)
REFERENCES members(member_id);

ALTER TABLE issued_status
ADD CONSTRAINT fk_books
FOREIGN KEY (issued_book_isbn)
REFERENCES books(isbn);

ALTER TABLE issued_status
ADD CONSTRAINT fk_emp
FOREIGN KEY (issued_emp_id)
REFERENCES employees(emp_id);

ALTER TABLE return_status
ADD CONSTRAINT fk_issued
FOREIGN KEY (issued_id)
REFERENCES issued_status(issued_id);

ALTER TABLE employees
ADD CONSTRAINT fk_branch
FOREIGN KEY (branch_id)
REFERENCES branch(branch_id);

ALTER TABLE return_status
ADD CONSTRAINT fk_return_book
FOREIGN KEY (return_book_isbn)
REFERENCES books(isbn);
</code></pre>

<h2>Data Population</h2>

<p>
All tables are populated with sample data to simulate real-world
library operations.
</p>

<h2>CRUD Operations</h2>

<p>
Basic Create, Read, Update, and Delete operations are performed
to manage library records.
</p>

<p style="font-size:14px;">
These operations include adding new books, updating member details,
deleting issued records, and retrieving transaction information.
</p>

<h2>Create Table As Select (CTAS)</h2>

<p>
CTAS operations are used to generate summary and reporting tables
from transactional data.
</p>

<p style="font-size:14px;">
Examples include book issue counts, high-value books,
branch performance reports, and active member identification.
</p>

<h2>Business Analysis Queries</h2>

<p>
The project answers several operational and analytical questions
using SQL joins and aggregations.
</p>

<p style="font-size:14px;">
These include rental income by category, recently registered members,
employees processing the most issues, and books not yet returned.
</p>

<h2>Advanced SQL Features</h2>

<p>
Advanced SQL techniques are implemented to extend functionality
and automate processes.
</p>

<p style="font-size:14px;">
These include Common Table Expressions (CTEs), window logic,
conditional aggregation, and stored procedures.
</p>

<h2>Stored Procedures</h2>

<p>
Stored procedures are created to manage book issuance and returns.
</p>

<p style="font-size:14px;">
They handle availability checks, automatic status updates,
and user-friendly system messages.
</p>

<h2>Overdue and Fine Management</h2>

<p>
The system identifies overdue books based on a 30-day return policy.
</p>

<p style="font-size:14px;">
CTAS queries are used to calculate overdue counts, total issued books,
and fines calculated at a fixed daily rate.
</p>

<h2>Project Outcome</h2>

<p>
This project demonstrates end-to-end database design and management
for a library system using SQL.
</p>

<p style="font-size:14px;">
It showcases practical use of relational modeling, CRUD operations,
reporting queries, and procedural SQL suitable for real-world applications.
</p>
