# Exercise 1: Create Two Tables and Perform a Join in a View

## Step 1: Create Tables

Create two tables: employees and departments.

```sql
-- Create Employees table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10, 2)
);

-- Create Departments table
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);
```

## Step 2: Insert Sample Data

Insert data into both tables.

```sql
-- Insert data into Employees
INSERT INTO employees (employee_id, employee_name, department_id, salary) 
VALUES
(1, 'John Doe', 101, 60000),
(2, 'Jane Smith', 102, 72000),
(3, 'Mark Johnson', 101, 58000),
(4, 'Emily Davis', 103, 80000);

-- Insert data into Departments
INSERT INTO departments (department_id, department_name) 
VALUES
(101, 'Sales'),
(102, 'Engineering'),
(103, 'Marketing');
```

## Step 3: Query the Data

Run a simple query on each table to ensure the data is inserted correctly.

```sql
-- Query Employees table
SELECT * FROM employees;

-- Query Departments table
SELECT * FROM departments;
```

## Step 4: Create a View to Display Join Results

Perform a join between the employees and departments tables to show employee details along with their department names. Create a view to store this join.

## Step 5: Query the View
