# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Syntax:**

```sql
-- Create table
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50)
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager');
INSERT INTO employees VALUES (2, 'Bob', 'Developer');
INSERT INTO employees VALUES (3, 'Charlie', 'Analyst');

COMMIT;


-- PL/SQL block with simple cursor and exception handling
DECLARE
  -- Cursor to fetch employee name and designation
  CURSOR emp_cursor IS
    SELECT emp_name, designation FROM employees;

  v_emp_name employees.emp_name%TYPE;
  v_designation employees.designation%TYPE;

  no_data EXCEPTION;  -- Custom exception for no data
  v_count NUMBER := 0;
BEGIN
  OPEN emp_cursor;
  LOOP
    FETCH emp_cursor INTO v_emp_name, v_designation;
    EXIT WHEN emp_cursor%NOTFOUND;
    v_count := v_count + 1;
    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_emp_name || ' | Designation: ' || v_designation);
  END LOOP;
  CLOSE emp_cursor;

  -- Raise custom exception if no rows fetched
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employee records found.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```

**Output:**  
The program should display the employee details or an error message.

<img width="423" height="167" alt="image" src="https://github.com/user-attachments/assets/881195d2-7ddb-4c9b-9f29-a62085b5bfed" />

---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.

**Syntax:**

```sql
-- Create table
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50)
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager');
INSERT INTO employees VALUES (2, 'Bob', 'Developer');
INSERT INTO employees VALUES (3, 'Charlie', 'Analyst');

COMMIT;


-- PL/SQL block with simple cursor and exception handling
DECLARE
  -- Cursor to fetch employee name and designation
  CURSOR emp_cursor IS
    SELECT emp_name, designation FROM employees;

  v_emp_name employees.emp_name%TYPE;
  v_designation employees.designation%TYPE;

  no_data EXCEPTION;  -- Custom exception for no data
  v_count NUMBER := 0;
BEGIN
  OPEN emp_cursor;
  LOOP
    FETCH emp_cursor INTO v_emp_name, v_designation;
    EXIT WHEN emp_cursor%NOTFOUND;
    v_count := v_count + 1;
    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_emp_name || ' | Designation: ' || v_designation);
  END LOOP;
  CLOSE emp_cursor;

  -- Raise custom exception if no rows fetched
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employee records found.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/


-- Modify the employees table to add a salary column
ALTER TABLE employees ADD (salary NUMBER);

-- Update employees with sample salaries
UPDATE employees SET salary = 75000 WHERE emp_id = 1;
UPDATE employees SET salary = 55000 WHERE emp_id = 2;
UPDATE employees SET salary = 40000 WHERE emp_id = 3;

COMMIT;


-- PL/SQL block using parameterized cursor with exception handling
DECLARE
  -- Declare parameterized cursor
  CURSOR emp_sal_cursor (p_min_sal NUMBER, p_max_sal NUMBER) IS
    SELECT emp_name, designation, salary
    FROM employees
    WHERE salary BETWEEN p_min_sal AND p_max_sal;

  -- Variables to hold fetched values
  v_emp_name employees.emp_name%TYPE;
  v_designation employees.designation%TYPE;
  v_salary employees.salary%TYPE;

  -- Salary range variables
  v_min_salary NUMBER := 45000;
  v_max_salary NUMBER := 80000;

  -- Custom exception
  no_data EXCEPTION;
  v_count NUMBER := 0;
BEGIN
  -- Open cursor with parameters
  OPEN emp_sal_cursor(v_min_salary, v_max_salary);
  LOOP
    FETCH emp_sal_cursor INTO v_emp_name, v_designation, v_salary;
    EXIT WHEN emp_sal_cursor%NOTFOUND;
    v_count := v_count + 1;
    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_emp_name ||
                         ' | Designation: ' || v_designation ||
                         ' | Salary: ' || v_salary);
  END LOOP;
  CLOSE emp_sal_cursor;

  -- Raise exception if no employees found
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employees found in the specified salary range.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```
**Output:**  
The program should display the employee details within the specified salary range or an error message if no data is found.

<img width="584" height="227" alt="image" src="https://github.com/user-attachments/assets/cb2bcd04-3423-46c9-8b48-50a50525f561" />

---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.

**Syntax:**

```sql
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  salary      NUMBER,
  dept_no     NUMBER
);

INSERT INTO employees VALUES (1, 'Alice', 'Manager', 75000, 10);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 55000, 20);
INSERT INTO employees VALUES (3, 'Charlie', 'Analyst', 40000, 30);
COMMIT;


-- PL/SQL block using a Cursor FOR Loop with exception handling
DECLARE
  v_count NUMBER := 0;
  no_data EXCEPTION;
BEGIN
  -- Cursor FOR loop automatically handles open, fetch, and close
  FOR emp_rec IN (SELECT emp_name, dept_no FROM employees) LOOP
    v_count := v_count + 1;
    DBMS_OUTPUT.PUT_LINE('Employee: ' || emp_rec.emp_name ||
                         ' | Department No: ' || emp_rec.dept_no);
  END LOOP;

  -- Raise custom exception if no employees found
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employees found in the employees table.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```

**Output:**  
The program should display employee names with their department numbers or the appropriate error message if no data is found.

<img width="396" height="165" alt="image" src="https://github.com/user-attachments/assets/bed5900c-dcb8-4b25-bfb4-07bc236ea945" />

---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Syntax:**

```sql
-- Create or replace the employees table (optional if already exists)
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  salary      NUMBER,
  dept_no     NUMBER
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager', 75000, 10);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 55000, 20);
INSERT INTO employees VALUES (3, 'Charlie', 'Analyst', 40000, 30);
COMMIT;


-- PL/SQL program using cursor with %ROWTYPE and exception handling
DECLARE
  -- Declare cursor
  CURSOR emp_cursor IS
    SELECT emp_id, emp_name, designation, salary FROM employees;

  -- Record variable based on the cursor’s row structure
  emp_record emp_cursor%ROWTYPE;

  -- Counter to check if any rows are fetched
  v_count NUMBER := 0;

  -- Custom exception
  no_data EXCEPTION;
BEGIN
  OPEN emp_cursor;
  LOOP
    FETCH emp_cursor INTO emp_record;
    EXIT WHEN emp_cursor%NOTFOUND;

    v_count := v_count + 1;
    DBMS_OUTPUT.PUT_LINE(
      'Emp_ID: ' || emp_record.emp_id ||
      ' | Name: ' || emp_record.emp_name ||
      ' | Designation: ' || emp_record.designation ||
      ' | Salary: ' || emp_record.salary
    );
  END LOOP;
  CLOSE emp_cursor;

  -- Raise custom exception if no data found
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employee records found.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```

**Output:**  
The program should display employee records or the appropriate error message if no data is found.

<img width="662" height="177" alt="image" src="https://github.com/user-attachments/assets/be1496ea-e545-4687-ab5f-66051d68802b" />

---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.

**Syntax:**

```sql
-- Create or replace the employees table (if not already exists)
CREATE TABLE employees (
  emp_id      NUMBER PRIMARY KEY,
  emp_name    VARCHAR2(50),
  designation VARCHAR2(50),
  salary      NUMBER,
  dept_no     NUMBER
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager', 75000, 10);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 55000, 20);
INSERT INTO employees VALUES (3, 'Charlie', 'Analyst', 40000, 10);
INSERT INTO employees VALUES (4, 'David', 'Tester', 42000, 30);
COMMIT;


-- PL/SQL block using cursor with FOR UPDATE and exception handling
DECLARE
  -- Cursor to select employees of a specific department for update
  CURSOR emp_cursor (p_dept NUMBER) IS
    SELECT emp_id, emp_name, salary
    FROM employees
    WHERE dept_no = p_dept
    FOR UPDATE;  -- Locks the selected rows

  -- Record variable to store each row
  emp_record emp_cursor%ROWTYPE;

  -- Variables
  v_dept_no NUMBER := 10;     -- Department to update
  v_count   NUMBER := 0;      -- Row counter
  no_data   EXCEPTION;        -- Custom exception

BEGIN
  -- Open cursor
  OPEN emp_cursor(v_dept_no);
  LOOP
    FETCH emp_cursor INTO emp_record;
    EXIT WHEN emp_cursor%NOTFOUND;
    v_count := v_count + 1;

    -- Update salary for each fetched record
    UPDATE employees
    SET salary = salary + 5000
    WHERE CURRENT OF emp_cursor;

    DBMS_OUTPUT.PUT_LINE('Updated salary for ' || emp_record.emp_name ||
                         ' (Emp_ID: ' || emp_record.emp_id || ')');
  END LOOP;
  CLOSE emp_cursor;

  -- Raise exception if no rows were updated
  IF v_count = 0 THEN
    RAISE no_data;
  END IF;

  COMMIT;

EXCEPTION
  WHEN no_data THEN
    DBMS_OUTPUT.PUT_LINE('No employees found in the specified department.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
    ROLLBACK;
END;
/
```
**Output:**  
The program should update employee salaries and display a message, or it should display an error message if no data is found.

<img width="403" height="145" alt="image" src="https://github.com/user-attachments/assets/f88b362f-02a1-4f6e-a7d0-c2098b709a83" />

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

