# SQL Retail Sales Analysis

## Project Overview
This project analyzes a retail sales dataset using PostgreSQL.  
The main objective of this project is to perform data cleaning, data exploration, and business analysis using SQL queries.

This project demonstrates practical SQL skills used in real-world data analytics projects.

---

## Tools & Technologies
- PostgreSQL
- SQL

---

## Dataset Information
The dataset contains retail transaction data including:

- Transaction ID
- Sale Date
- Sale Time
- Customer ID
- Gender
- Age
- Product Category
- Quantity Sold
- Price Per Unit
- COGS
- Total Sale Amount

---

# Database Setup

## Create Table

```sql
DROP TABLE IF EXISTS retailsales;

CREATE TABLE retailsales(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(15),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

# Data Cleaning

## Checking NULL Values

```sql
SELECT * FROM retailsales
WHERE transactions_id IS NULL
OR sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR age IS NULL
OR category IS NULL
OR quantiy IS NULL
OR price_per_unit IS NULL
OR cogs IS NULL
OR total_sale IS NULL;
```

## Removing NULL Values

```sql
DELETE FROM retailsales
WHERE transactions_id IS NULL
OR sale_date IS NULL
OR sale_time IS NULL
OR customer_id IS NULL
OR gender IS NULL
OR age IS NULL
OR category IS NULL
OR quantiy IS NULL
OR price_per_unit IS NULL
OR cogs IS NULL
OR total_sale IS NULL;
```

---

# Data Exploration

## Total Sales Records

```sql
SELECT COUNT(*) AS total_sales
FROM retailsales;
```

## Total Unique Customers

```sql
SELECT COUNT(DISTINCT customer_id) AS total_customers
FROM retailsales;
```

## Unique Product Categories

```sql
SELECT DISTINCT category
FROM retailsales;
```

---

# Business Problems & Solutions

## Q1. Retrieve all sales made on '2022-11-05'

```sql
SELECT *
FROM retailsales
WHERE sale_date = '2022-11-05';
```

---

## Q2. Retrieve all Clothing transactions where quantity sold is more than 4 in Nov-2022

```sql
SELECT *
FROM retailsales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantiy >= 4;
```

---

## Q3. Calculate total sales for each category

```sql
SELECT
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retailsales
GROUP BY category;
```

---

## Q4. Find average age of customers who purchased from Beauty category

```sql
SELECT
    ROUND(AVG(age), 2) AS average_age
FROM retailsales
WHERE category = 'Beauty';
```

---

## Q5. Find transactions where total_sale is greater than 1000

```sql
SELECT *
FROM retailsales
WHERE total_sale > 1000;
```

---

## Q6. Find total number of transactions made by each gender in each category

```sql
SELECT
    gender,
    category,
    COUNT(transactions_id) AS total_transactions
FROM retailsales
GROUP BY gender, category
ORDER BY category;
```

---

## Q7. Find the best selling month in each year

```sql
WITH sales AS (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS average_sale,
        RANK() OVER(
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM retailsales
    GROUP BY 1,2
)

SELECT *
FROM sales
WHERE rnk = 1;
```

---

## Q8. Find top 5 customers based on highest total sales

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retailsales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

## Q9. Find number of unique customers from each category

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retailsales
GROUP BY category;
```

---

## Q10. Create sales shifts and count number of orders

### Shift Timing
- Morning → Hour <= 12
- Afternoon → Hour between 12 and 17
- Evening → Hour > 17

```sql
WITH shifts AS (
    SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) <= 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift
    FROM retailsales
)

SELECT
    shift,
    COUNT(*) AS number_of_orders
FROM shifts
GROUP BY shift;
```

---

# Key Insights
- Clothing and Beauty categories contribute significantly to sales.
- Customer purchase behavior changes throughout the day.
- Top customers generate a large portion of revenue.
- Monthly sales trends help identify peak business periods.

---

# SQL Concepts Used
- SELECT Statements
- WHERE Clause
- GROUP BY
- ORDER BY
- Aggregate Functions
- Window Functions
- Common Table Expressions (CTEs)
- Date & Time Functions
- Data Cleaning Techniques

---

# Future Improvements
- Build interactive dashboards using Power BI or Tableau
- Add customer segmentation analysis
- Perform profit analysis
- Create sales forecasting models

---

# Author
Abhishek khati  
BCA Student | Aspiring Data Analyst
# retail_sales_analysis
SQL Retail Sales Analysis Project using PostgreSQL with data cleaning, exploration, and business insights
