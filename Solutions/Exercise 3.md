# Exercise 3: Create Tables for Orders, Order Items, and Customers

## Step 1: Create Tables

Create three tables: orders, order_items, and customers.

```sql
-- Create Orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE
);

-- Create Order Items table
CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY,
    order_id INT,
    product_name VARCHAR(50),
    quantity INT,
    price DECIMAL(10, 2)
);

-- Create Customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50)
);
```

## Step 2: Insert Sample Data

Insert data into all three tables.

```sql
-- Insert data into Orders
INSERT INTO orders (order_id, customer_id, order_date) 
VALUES
(1, 1001, '2024-08-01'),
(2, 1002, '2024-08-02');

-- Insert data into Order Items
INSERT INTO order_items (order_item_id, order_id, product_name, quantity, price) 
VALUES
(1, 1, 'Monitor', 2, 300),
(2, 1, 'Mouse', 1, 50),
(3, 2, 'Laptop', 1, 1500);

-- Insert data into Customers
INSERT INTO customers (customer_id, customer_name) 
VALUES
(1001, 'Charlie Green'),
(1002, 'Diana White');
```

## Step 3: Query the Data

Run queries on each table.

```sql
-- Query Orders table
SELECT * FROM orders;

-- Query Order Items table
SELECT * FROM order_items;

-- Query Customers table
SELECT * FROM customers;
```

## Step 4: Create a View for Order Details

Create a view that joins the orders, order_items, and customers tables to show detailed order information.

```sql
-- Create a view for order details
CREATE VIEW order_details_view AS
SELECT o.order_id, c.customer_name, o.order_date, i.product_name, i.quantity, i.price, (i.quantity * i.price) AS total_price
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items i ON o.order_id = i.order_id;
```

## Step 5: Query the View

```sql
-- Query the view
SELECT * FROM order_details_view;
```
