# Retail Data Analysis SQL Project

## Project Overview

**Project Title:** Retail Data Analysis SQL

**Level:** Beginner

**Database:** retail_data_db

This project demonstrates SQL skills used by data analysts to explore and analyze a retail dataset. It covers data setup, cleaning, and analysis to answer practical business questions.
The goal is to learn how to organize, query, and interpret data to uncover useful patterns and insights.

---

## Objectives

* Build and set up a database for retail transactions.
* Clean incomplete or incorrect data.
* Explore the dataset and extract useful metrics.
* Answer key business questions using SQL queries.

---

## Project Structure

### 1. Database Setup

**Database Creation**

```sql
CREATE DATABASE retail_data_db;
```

**Table Creation**

```sql
CREATE TABLE retail_records (
    transactions_id INT,
    record_date DATE,
    record_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_amount FLOAT
);
```

---

### 2. Data Exploration & Cleaning

**Check Total Records**

```sql
SELECT COUNT(*) FROM retail_records;
```

**Unique Customers**

```sql
SELECT COUNT(DISTINCT customer_id) FROM retail_records;
```

**Unique Categories**

```sql
SELECT DISTINCT category FROM retail_records;
```

**Check for Missing Data**

```sql
SELECT *
FROM retail_records
WHERE
    transactions_id IS NULL
    OR record_date IS NULL
    OR record_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR cogs IS NULL
    OR total_amount IS NULL;
```

**Delete Missing Records**

```sql
DELETE FROM retail_records
WHERE
    transactions_id IS NULL
    OR record_date IS NULL
    OR record_time IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR quantity IS NULL
    OR cogs IS NULL
    OR total_amount IS NULL;
```

---

### 3. Data Analysis & Insights

#### Q1. Retrieve all columns for a specific date

```sql
SELECT *
FROM retail_records
WHERE record_date = '2022-11-05';
```

#### Q2. Find all records from a specific category where quantity > 4 in Nov 2022

```sql
SELECT *
FROM retail_records
WHERE 
    category = 'Clothing'
    AND TO_CHAR(record_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;
```

#### Q3. Calculate total amount per category

```sql
SELECT 
    category,
    SUM(total_amount) AS net_amount,
    COUNT(*) AS total_orders
FROM retail_records
GROUP BY 1;
```

#### Q4. Average age of customers who bought from ‘Beauty’ category

```sql
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_records
WHERE category = 'Beauty';
```

#### Q5. Show transactions where total_amount > 1000

```sql
SELECT *
FROM retail_records
WHERE total_amount > 1000;
```

#### Q6. Count of records by gender and category

```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_count
FROM retail_records
GROUP BY category, gender
ORDER BY 1;
```

#### Q7. Average amount per month and find best performing month each year

```sql
SELECT 
       year,
       month,
       avg_amount
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM record_date) AS year,
    EXTRACT(MONTH FROM record_date) AS month,
    AVG(total_amount) AS avg_amount,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM record_date) ORDER BY AVG(total_amount) DESC) AS rank
FROM retail_records
GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```

#### Q8. Top 5 customers based on total amount

```sql
SELECT 
    customer_id,
    SUM(total_amount) AS total_value
FROM retail_records
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

#### Q9. Number of unique customers by category

```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_records
GROUP BY category;
```

#### Q10. Shift-wise order count (Morning, Afternoon, Evening)

```sql
WITH hourly_records AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM record_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM record_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_records
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_records
GROUP BY shift;
```

---

## Key Findings

* **Customer Demographics:** Includes different age groups and categories like Clothing, Beauty, etc.
* **High-Value Transactions:** Many customers made premium purchases (above 1000).
* **Monthly Trends:** Shows clear month-to-month variations in transaction amounts.
* **Top Customers:** Easy to identify top spenders and loyal buyers.

---

## Reports

* **Summary Report:** Overall transactions, demographics, and category-wise performance.
* **Trend Report:** Monthly and shift-based analysis.
* **Customer Insights:** Identifies unique and high-value customers.

---

## Conclusion

This project is a simple but powerful way to practice SQL for real-world analytics.
It walks through the whole process — from setting up a database to cleaning and analyzing data — using practical queries that mimic real business use cases.

---
