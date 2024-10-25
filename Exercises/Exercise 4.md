# Exercise: Advanced UDF for Insurance Risk Rating Calculation

This set of exercises will focus on creating a more complex UDF to calculate a risk rating for customers based on various policy attributes.

## Step 1: Create a Table for Insurance Claims

```sql
CREATE TABLE insurance_claims (
    claim_id INT PRIMARY KEY,
    customer_id INT,
    policy_id INT,
    claim_amount DECIMAL(10, 2),
    claim_date DATE,
    claim_status VARCHAR(20) -- e.g., 'Approved', 'Pending', 'Rejected'
);
```

## Step 2: Insert Sample Data

```sql
INSERT INTO insurance_claims (claim_id, customer_id, policy_id, claim_amount, claim_date, claim_status)
VALUES
(1, 1001, 1, 5000, '2024-01-15', 'Approved'),
(2, 1002, 2, 3000, '2024-02-10', 'Rejected'),
(3, 1003, 3, 10000, '2024-03-05', 'Approved'),
(4, 1004, 1, 2500, '2024-04-20', 'Pending');
```

## Step 3: Create a UDF for Risk Rating

Create a function that calculates a risk rating based on the following factors:

```
If the total claim amount exceeds 20% of the coverage amount, consider the customer high-risk (rating = 'High').
If the total claim amount is between 10% and 20% of the coverage amount, consider the customer medium-risk (rating = 'Medium').
Otherwise, the customer is low-risk (rating = 'Low').
```

## Step 4: Calculate Total Claim Amount for Each Customer

Before using the UDF, calculate the total claim amount for each customer by grouping the claims data.

## Step 5: Use the UDF to Calculate Risk Rating

Now use the calculate_risk_rating UDF to assign a risk rating to each customer based on their total claim amount and the coverage amount.
