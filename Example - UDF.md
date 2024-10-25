# Example: Basic User-Defined Function (UDF) for Insurance Premium Calculation

In this exercise, you will create an insurance dataset and a UDF to calculate the insurance premium based on specific conditions.

## Step 1: Create a Table for Insurance Policies
```sql
CREATE TABLE insurance_policies (
    policy_id INT PRIMARY KEY,
    customer_id INT,
    policy_type VARCHAR(20),
    coverage_amount DECIMAL(10, 2),
    base_premium DECIMAL(10, 2),
    age_of_policyholder INT
);
```

## Step 2: Insert Sample Data
```sql
INSERT INTO insurance_policies (policy_id, customer_id, policy_type, coverage_amount, base_premium, age_of_policyholder)
VALUES
(1, 1001, 'Health', 50000, 1200, 45),
(2, 1002, 'Auto', 30000, 800, 30),
(3, 1003, 'Life', 100000, 2000, 60),
(4, 1004, 'Health', 70000, 1500, 25);
```

## Step 3: Create a UDF to Calculate Premium Based on Age

Create a function that adjusts the insurance premium based on the age of the policyholder. For this example, let's assume the following logic:

```
If the age is greater than 50, increase the premium by 20%.
If the age is less than or equal to 30, reduce the premium by 10%.
Otherwise, keep the premium unchanged.
```

```sql
CREATE OR REPLACE FUNCTION calculate_adjusted_premium(base_premium DECIMAL(10, 2), age_of_policyholder INT)
RETURNS DECIMAL(10, 2)
IMMUTABLE
AS $$
BEGIN
    IF age_of_policyholder > 50 THEN
        RETURN base_premium * 1.20;
    ELSIF age_of_policyholder <= 30 THEN
        RETURN base_premium * 0.90;
    ELSE
        RETURN base_premium;
    END IF;
END;
$$ LANGUAGE plpgsql;
```

## Step 4: Use the UDF in a Query

Query the insurance_policies table and use the calculate_adjusted_premium function to display the adjusted premium for each policy.

```sql
SELECT 
    policy_id, 
    customer_id, 
    policy_type, 
    coverage_amount, 
    base_premium,
    age_of_policyholder,
    calculate_adjusted_premium(base_premium, age_of_policyholder) AS adjusted_premium
FROM insurance_policies;
```
