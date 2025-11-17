# pl-sql-collections-and-records-GOTO-demonstration
Author:    KHADIDJA ADAM ABAKAR KAYAYE
Purpose:    Demonstrate PL/SQL Collections, Records, and GOTO statements.

 1.  Problem Statement

The goal is to create a PL/SQL script that processes monthly employee data. The script must:

1.  Fetch specific employees from the 'Shipping' (ID 50) and 'Sales' (ID 80) departments.
2.  Store their details (ID, name, salary, department) in a custom structure.
3.  Store their last three monthly performance scores.
4.  Use a lookup table (an Associative Array) to find the bonus multiplier for their specific department.
5.  Loop through the collected employees, calculate their bonus based on their salary and department rate, and print a report.
6.  Include an error-handling check that uses a GOTO statement to skip the process if no employees are found in the target departments.
 2. Solution Overview

To solve this problem, a single anonymous PL/SQL block was created. It uses a combination of Records and Collections to manage the data, and a GOTO statement for control flow.
 A. Records (Storing Data)

* **User-Defined Record (type_emp_bonus_rec):**
    A custom RECORD type was defined to hold all the data for one employee. This record contains a mix of data types: NUMBER for ID, VARCHAR2 for name, and a nested VARRAY collection for scores.
* **Cursor-Based Record (emp_row c_emp%ROWTYPE):**
    A cursor (c_emp) was declared to fetch data from the employees and departments tables. The emp_row variable, created using %ROWTYPE, is a record that automatically matches the cursor's structure. This is used to temporarily hold each row as it's fetched from the database.

### B. Collections (Organizing Data)

* **VARRAY (type_perf_scores):**
    A VARRAY(3) OF NUMBER is used to store the list of three performance scores. This collection is nested inside the type_emp_bonus_rec record, allowing each employee to have their own list of scores.

* **Nested Table (type_emp_bonus_list):**
    A TABLE OF type_emp_bonus_rec is used as the main collection. It holds the complete list of all employees being processed. This was chosen because it's flexible and allows us to store the list of custom records.

* **Associative Array (type_bonus_rates):**
    A TABLE OF NUMBER INDEX BY VARCHAR2(50) is used as a simple key-value lookup. It stores the bonus percentage (a NUMBER) indexed by the department name (a VARCHAR2). This makes it easy to find the bonus for 'Shipping' (0.10) or 'Sales' (0.15) without extra queriC. GOTO Statement (Control Flow)

* A GOTO statement was used as required by the assignment.
* *How it works:* Before any processing, a query checks if any employees exist in departments 50 or 80.
* If the count is zero (0), a GOTO end_of_script; command executes.
* This command forces the script to jump immediately to the <<end_of_script>> label at the very end of the BEGIN block.
* This efficiently skips all the data fetching and processing loops, preventing errors and unnecessary work.
CREATE TABLE departments (
    department_id    NUMBER PRIMARY KEY,
    department_name  VARCHAR2(50) NOT NULL
);
INSERT INTO departments (department_id, department_name)
VALUES (50, 'Shipping');

INSERT INTO departments (department_id, department_name)
VALUES (80, 'Sales');
CREATE TABLE employees (
    employee_id     NUMBER PRIMARY KEY,
    first_name      VARCHAR2(50),
    last_name       VARCHAR2(50),
    salary          NUMBER(10,2),
    department_id   NUMBER,
    
    CONSTRAINT fk_emp_dept
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);INSERT INTO employees (employee_id, first_name, last_name, salary, department_id)
VALUES (101, 'Alice', 'Brown', 4000, 50);

INSERT INTO employees (employee_id, first_name, last_name, salary, department_id)
VALUES (102, 'David', 'Smith', 4500, 50);

INSERT INTO employees (employee_id, first_name, last_name, salary, department_id)
VALUES (201, 'Maria', 'Lopez', 5000, 80);

INSERT INTO employees (employee_id, first_name, last_name, salary, department_id)
VALUES (202, 'Tom', 'Adams', 5200, 80);
