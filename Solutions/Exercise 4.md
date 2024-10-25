# Exercise: Advanced UDF for Insurance Risk Rating Calculation

This set of exercises will focus on creating a more complex UDF to calculate a risk rating for customers based on various policy attributes.

## Step 1: Create a Table for Insurance Claims and Insurance Policies

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

```sql
CREATE TABLE insurance_policies (
    policy_id INT PRIMARY KEY,
    customer_id INT,
    policy_type VARCHAR(20),
    coverage_amount DOUBLE PRECISION,
    base_premium DOUBLE PRECISION,
    age_of_policyholder INT
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

```sql
INSERT INTO insurance_policies (policy_id, customer_id, policy_type, coverage_amount, base_premium, age_of_policyholder)
VALUES
(1, 1001, 'Health', 50000, 1200, 45),
(2, 1002, 'Auto', 30000, 800, 30),
(3, 1003, 'Life', 100000, 2000, 60),
(4, 1004, 'Health', 70000, 1500, 25);
```

## Step 3: Create a UDF for Risk Rating

Create a function that calculates a risk rating based on the following factors:

```
If the total claim amount exceeds 20% of the coverage amount, consider the customer high-risk (rating = 'High').
If the total claim amount is between 10% and 20% of the coverage amount, consider the customer medium-risk (rating = 'Medium').
Otherwise, the customer is low-risk (rating = 'Low').
```

```sql
CREATE OR REPLACE FUNCTION calculate_risk_rating(coverage_amount DECIMAL(10, 2), total_claim_amount DECIMAL(10, 2))
RETURNS VARCHAR(10)
IMMUTABLE
AS $$
BEGIN
    IF total_claim_amount > (coverage_amount * 0.20) THEN
        RETURN 'High';
    ELSIF total_claim_amount BETWEEN (coverage_amount * 0.10) AND (coverage_amount * 0.20) THEN
        RETURN 'Medium';
    ELSE
        RETURN 'Low';
    END IF;
END;
$$ LANGUAGE plpgsql;
```

## Step 4: Calculate Total Claim Amount for Each Customer

Before using the UDF, calculate the total claim amount for each customer by grouping the claims data.

```sql
WITH total_claims AS (
    SELECT customer_id, policy_id, SUM(claim_amount) AS total_claim_amount
    FROM insurance_claims
    WHERE claim_status = 'Approved'
    GROUP BY customer_id, policy_id
)
SELECT p.policy_id, p.customer_id, p.policy_type, p.coverage_amount, t.total_claim_amount
FROM insurance_policies p
JOIN total_claims t ON p.policy_id = t.policy_id;
```

## Step 5: Use the UDF to Calculate Risk Rating

Now use the calculate_risk_rating UDF to assign a risk rating to each customer based on their total claim amount and the coverage amount.

```sql
WITH total_claims AS (
    SELECT customer_id, policy_id, SUM(claim_amount) AS total_claim_amount
    FROM insurance_claims
    WHERE claim_status = 'Approved'
    GROUP BY customer_id, policy_id
)
SELECT 
    p.policy_id, 
    p.customer_id, 
    p.policy_type, 
    p.coverage_amount, 
    t.total_claim_amount,
    calculate_risk_rating(p.coverage_amount, t.total_claim_amount) AS risk_rating
FROM insurance_policies p
JOIN total_claims t ON p.policy_id = t.policy_id;
```
