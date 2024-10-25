# Exercise 2: Create Three Tables and Perform a Multi-table Join

## Step 1: Create Tables

Create three tables: products, sales, and customers.

```sql
-- Create Products table
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    price DECIMAL(10, 2)
);

-- Create Sales table
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT,
    sale_date DATE,
    quantity INT
);

-- Create Customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50),
    location VARCHAR(50)
);
```

## Step 2: Insert Sample Data

Insert data into all three tables.

```sql
-- Insert data into Products
INSERT INTO products (product_id, product_name, price) 
VALUES
(1, 'Laptop', 1500),
(2, 'Headphones', 200),
(3, 'Keyboard', 50);

-- Insert data into Sales
INSERT INTO sales (sale_id, product_id, customer_id, sale_date, quantity) 
VALUES
(1, 1, 101, '2024-01-15', 2),
(2, 2, 102, '2024-02-10', 3),
(3, 3, 101, '2024-03-05', 1);

-- Insert data into Customers
INSERT INTO customers (customer_id, customer_name, location) 
VALUES
(101, 'Alice Johnson', 'New York'),
(102, 'Bob Brown', 'California');
```

## Step 3: Query the Data

Run queries on each table.

```sql
-- Query Products table
SELECT * FROM products;

-- Query Sales table
SELECT * FROM sales;

-- Query Customers table
SELECT * FROM customers;
```

## Step 4: Create a View to Perform a Multi-table Join

Create a view that joins the products, sales, and customers tables to show detailed sales information.

```sql
-- Create a view for the multi-table join
CREATE VIEW sales_details_view AS
SELECT s.sale_id, c.customer_name, p.product_name, s.sale_date, s.quantity, p.price, (s.quantity * p.price) AS total_amount
FROM sales s
JOIN customers c ON s.customer_id = c.customer_id
JOIN products p ON s.product_id = p.product_id;
```

## Step 5: Query the View

```sql
-- Query the view
SELECT * FROM sales_details_view;
```
