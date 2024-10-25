# Creating a View for Insurance Data

## Step 1: Create Sample Tables

Let’s first create two simple tables: insurance_policies and insurance_claims.

### Insurance Policies Table:

```sql
CREATE TABLE insurance_policies (
    policy_id INT PRIMARY KEY,
    customer_id INT,
    policy_type VARCHAR(20),
    coverage_amount DECIMAL(10, 2)
);
```

### Insurance Claims Table:

```sql
CREATE TABLE insurance_claims (
    claim_id INT PRIMARY KEY,
    customer_id INT,
    policy_id INT,
    claim_amount DECIMAL(10, 2),
    claim_date DATE
);
```

## Step 2: Insert Sample Data

Insert some sample data into both tables.

```sql
INSERT INTO insurance_policies (policy_id, customer_id, policy_type, coverage_amount)
VALUES
(1, 1001, 'Health', 50000),
(2, 1002, 'Auto', 30000),
(3, 1003, 'Life', 100000);

INSERT INTO insurance_claims (claim_id, customer_id, policy_id, claim_amount, claim_date)
VALUES
(1, 1001, 1, 5000, '2024-01-15'),
(2, 1002, 2, 3000, '2024-02-10'),
(3, 1003, 3, 10000, '2024-03-05');
```

## Step 3: Create a View

Now, let’s create a view that joins the insurance_policies and insurance_claims tables to display policies and their associated claims. A view will give a consolidated view of the data.

```sql
CREATE VIEW insurance_policy_claims_view AS
SELECT 
    p.policy_id,
    p.customer_id,
    p.policy_type,
    p.coverage_amount,
    c.claim_id,
    c.claim_amount,
    c.claim_date
FROM insurance_policies p
LEFT JOIN insurance_claims c ON p.policy_id = c.policy_id;
```

## Step 4: Query the View

Now that the view is created, you can query it just like you would a regular table.

```sql
SELECT * FROM insurance_policy_claims_view;
```
