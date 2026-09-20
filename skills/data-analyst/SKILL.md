---
name: "data-analyst"
description: "Data analyst specializing in SQL queries, data exploration, statistical analysis, and business intelligence. Use when writing SQL queries, exploring datasets, calculating metrics, or analyzing business data. Covers SQL aggregation, window functions, CTEs, subqueries, and data visualization."
compatibility: pi
---

# Data Analyst

SQL expertise and data analysis workflows for business insights.

## Core Workflows

### 1. Basic Queries

```sql
-- Select with filtering
SELECT 
    user_id,
    COUNT(*) as order_count,
    SUM(amount) as total_spent,
    AVG(amount) as avg_order_value
FROM orders
WHERE created_at >= '2024-01-01'
GROUP BY user_id
HAVING COUNT(*) > 5
ORDER BY total_spent DESC
LIMIT 100;
```

### 2. Window Functions

```sql
-- Running total
SELECT 
    date,
    amount,
    SUM(amount) OVER (ORDER BY date) as running_total
FROM daily_sales;

-- Row number
SELECT 
    *,
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) as rank
FROM products;

-- Moving average
SELECT 
    date,
    price,
    AVG(price) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) as ma_7d
FROM stock_prices;
```

### 3. Common Table Expressions (CTEs)

```sql
WITH monthly_sales AS (
    SELECT 
        DATE_TRUNC(created_at, MONTH) as month,
        SUM(amount) as revenue
    FROM orders
    GROUP BY 1
),
customers AS (
    SELECT COUNT(DISTINCT user_id) as new_users, month
    FROM orders
    GROUP BY month
)
SELECT 
    m.month,
    m.revenue,
    c.new_users
FROM monthly_sales m
JOIN customers c ON m.month = c.month;
```

### 4. Subqueries

```sql
-- Find customers above average
SELECT *
FROM customers
WHERE lifetime_value > (
    SELECT AVG(lifetime_value) 
    FROM customers
);

-- Top product per category
SELECT *
FROM products p1
WHERE price = (
    SELECT MAX(price) 
    FROM products p2 
    WHERE p2.category = p1.category
);
```

### 5. Data Exploration

```python
import pandas as pd

# Quick analysis
df = pd.read_sql(query, connection)
df.head()
df.info()
df.describe()
df.isnull().sum()

# Distribution
df['column'].value_counts().head(10)
df.groupby('category')['value'].agg(['sum', 'mean', 'count'])
```

## Common Patterns

| Pattern | Query |
|---------|-------|
| Month-over-month | `LAG()` window function |
| Year-over-year | Self-join with date filter |
| Running totals | `SUM() OVER (ORDER BY)` |
| Percent of total | `SUM() OVER () / SUM() OVER ()` |
| Retention cohort | Self-join with timeline |

## Best Practices

1. **Use indexes** on filtering columns
2. **Avoid SELECT *** - specify columns
3. **Use CTEs** for readability
4. **EXPLAIN ANALYZE** before running large queries
5. **LIMIT results** during exploration

## Quick Reference

```sql
-- Count rows
SELECT COUNT(*) FROM table;

-- Distinct values
SELECT DISTINCT column FROM table;

-- Sample data
SELECT * FROM table LIMIT 10;

-- Date filters
WHERE date >= '2024-01-01' AND date < '2024-02-01'
```