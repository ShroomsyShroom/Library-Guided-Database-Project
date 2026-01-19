<h1>Library Management System Database Project</h1>

<p>This project demonstrates the development of a comprehensive Library Management System using SQL. It covers database design, table creation, establishing entity relationships (ERD), data manipulation (CRUD), and advanced analytical techniques including Stored Procedures and CTAS.</p>

<h2>1. Database Schema Setup</h2> <p>The first phase involves creating the core tables to handle branches, employees, books, members, and transaction statuses.</p>

<pre> -- Creating the Branch table CREATE TABLE branch ( branch_id VARCHAR(10) PRIMARY KEY, manager_id VARCHAR(10), branch_address VARCHAR(55), contact_no VARCHAR(10) );

-- Creating the Employees table CREATE TABLE employees( emp_id VARCHAR(10) PRIMARY KEY, emp_name VARCHAR(25), position VARCHAR(15), salary INT, branch_id VARCHAR(25) );

-- Creating the Books table CREATE TABLE books( isbn VARCHAR(20) PRIMARY KEY, book_title VARCHAR(75), category VARCHAR(15), rental_price FLOAT, status VARCHAR(15), author VARCHAR(35), publisher VARCHAR(55) );

-- Creating the Members table CREATE TABLE members( member_id VARCHAR(10) PRIMARY KEY, member_name VARCHAR(25), member_address VARCHAR(75), reg_date DATE );

-- Creating the Issued Status table CREATE TABLE issued_status( issued_id VARCHAR(10) PRIMARY KEY, issued_member_id VARCHAR(15), issued_book_name VARCHAR(75), issued_date DATE, issued_book_isbn VARCHAR(35), issued_emp_id VARCHAR(10) );

-- Creating the Return Status table CREATE TABLE return_status( return_id VARCHAR(10) PRIMARY KEY, issued_id VARCHAR(10), return_book_name VARCHAR(75), return_date DATE, return_book_isbn VARCHAR(35) ); </pre>

<h2>2. Entity Relationship Framework (ERD)</h2> <p>To maintain data integrity, foreign key constraints are applied to link the tables together. This forms the structural backbone of the library system.</p>

<pre> -- Adding Foreign Key Constraints ALTER TABLE issued_status ADD CONSTRAINT fk_members FOREIGN KEY (issued_member_id) REFERENCES members(member_id); ALTER TABLE issued_status ADD CONSTRAINT fk_books FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn); ALTER TABLE issued_status ADD CONSTRAINT fk_empId FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id); ALTER TABLE return_status ADD CONSTRAINT fk_issued FOREIGN KEY (issued_id) REFERENCES issued_status(issued_id); ALTER TABLE employees ADD CONSTRAINT fk_branch FOREIGN KEY (branch_id) REFERENCES branch(branch_id); ALTER TABLE return_status ADD CONSTRAINT fk_book_isbn FOREIGN KEY (return_book_isbn) REFERENCES books(isbn); </pre>

<h2>3. CRUD Operations and Basic Analysis</h2> <p>These tasks demonstrate the ability to Create, Retrieve, Update, and Delete records within the system.</p>

<ul> <li><strong>Create:</strong> Adding a new book record.</li> <pre>INSERT INTO books VALUES ('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');</pre>

<li><strong>Update:</strong> Changing a member's address.</li>
<pre>UPDATE members SET member_address = '125 Bang Street' WHERE member_id='C101';</pre>

<li><strong>Delete:</strong> Removing an issued record.</li>
<pre>DELETE FROM issued_status WHERE issued_id = 'IS121';</pre>
</ul>

<h2>4. CTAS (Create Table As Select) Summary Tables</h2> <p>CTAS is used to generate analytical reports and separate specific data subsets.</p>

<pre> -- Task: Create a summary of total issues per book CREATE TABLE book_cnts AS SELECT b.isbn, b.book_title, COUNT(ist.issued_id) AS no_of_issued FROM books AS b JOIN issued_status AS ist ON ist.issued_book_isbn = b.isbn GROUP BY 1, 2; </pre>

<h2>5. Advanced SQL Logic & Stored Procedures</h2> <p>For more complex automation, such as managing book availability during issuance and return, Stored Procedures are utilized.</p>

<h3>Automating Book Issuance</h3> <p>This procedure checks if a book is available before allowing it to be issued. If available, it creates the record and updates the book status to 'no'.</p>

<pre> CREATE OR REPLACE PROCEDURE issue_a_book(p_issued_id VARCHAR(10), p_issued_member_id VARCHAR(30), p_issued_book_isbn VARCHAR(50), p_issued_emp_id VARCHAR(10)) LANGUAGE plpgsql AS $$ DECLARE v_status VARCHAR(10); BEGIN SELECT status INTO v_status FROM books WHERE isbn = p_issued_book_isbn;

IF v_status = &#39;yes&#39; THEN
    INSERT INTO issued_status(issued_id, issued_member_id, issued_date, issued_book_isbn, issued_emp_id)
    VALUES (p_issued_id, p_issued_member_id, CURRENT_DATE, p_issued_book_isbn, p_issued_emp_id);

    UPDATE books SET status = &#39;no&#39; WHERE isbn = p_issued_book_isbn;
    RAISE NOTICE &#39;Book issued successfully.&#39;;
ELSE
    RAISE NOTICE &#39;Book is currently unavailable.&#39;;
END IF;
END; $$; </pre>

<h2>6. Analytics and Reporting</h2>

<h3>Branch Performance Report</h3> <p>Generates a comprehensive view of activity per branch, including revenue and return rates.</p>

<pre> CREATE TABLE branch_reports AS SELECT br.branch_id, br.manager_id, COUNT(ist.issued_id) AS no_of_books_issued, COUNT(rs.return_id) AS no_of_books_returned, SUM(b.rental_price) AS total_revenue FROM issued_status AS ist INNER JOIN employees AS e ON ist.issued_emp_id = e.emp_id INNER JOIN branch AS br ON br.branch_id = e.branch_id LEFT JOIN return_status AS rs ON rs.issued_id = ist.issued_id INNER JOIN books AS b ON ist.issued_book_isbn = b.isbn GROUP BY 1, 2; </pre>

<h3>Overdue Fines Summary</h3> <p>Identifies members with books kept longer than 30 days and calculates a fine of $0.50 per day overdue.</p>

<pre> CREATE TABLE overdue_fines_summary AS SELECT ist.issued_member_id AS member_id, COUNT(*) FILTER (WHERE rs.return_id IS NULL AND CURRENT_DATE - ist.issued_date > 30) AS overdue_books, SUM(CASE WHEN rs.return_id IS NULL AND CURRENT_DATE - ist.issued_date > 30 THEN (CURRENT_DATE - ist.issued_date - 30) * 0.50 ELSE 0 END) AS total_fines FROM issued_status AS ist LEFT JOIN return_status AS rs ON rs.issued_id = ist.issued_id GROUP BY ist.issued_member_id; </pre>

<h2>Conclusion</h2> <p>This library system manages the complete lifecycle of book borrowing. By utilizing advanced SQL features like Stored Procedures and CTAS, the database not only stores data but also automates business logic and provides instant operational insights.</p>
