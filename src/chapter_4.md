# Chapter 4: Data Query Language (DQL)

## 4.1 Introduction to DQL

Data Query Language (DQL) is the SQL subset used to retrieve data from databases. While technically there's only one DQL command (`SELECT`), it offers extensive capabilities through various clauses and combinations. DQL forms the foundation of data analysis in PostgreSQL, enabling filtering, sorting, and transformation of stored data.

Key characteristics of DQL:

- Non-destructive (doesn't modify data)
- Supports complex data transformations
- Enables multi-table operations through joins
- Includes aggregation and statistical functions

## 4.2 Basic SELECT Statements

### 4.2.1 Simple Data Retrieval

Retrieve all columns from the `customers` table:

```sql
SELECT * FROM customers;
```

- `SELECT *`: Selects all columns
- `FROM customers`: Specifies the source table

### 4.2.2 Column Selection

Get specific columns from `products`:

```sql
SELECT name, price FROM products;
```

- Explicit column selection improves performance
- Maintains column order as specified

### 4.2.3 Filtering with WHERE

Find orders over $100:

```sql
SELECT id, order_date, total
FROM orders
WHERE total > 100.00;
```

- `WHERE` clause filters rows before aggregation
- Supports comparison operators (`=`, `<>`, `>`, `<`, etc.)

### 4.2.4 Sorting with ORDER BY

Get recent orders first:

```sql
SELECT *
FROM orders
ORDER BY order_date DESC;
```

- `DESC` for descending order (default is `ASC`)
- Can sort by multiple columns

### 4.2.5 Limiting Results

Get top 5 expensive products:

```sql
SELECT name, price
FROM products
ORDER BY price DESC
LIMIT 5;
```

- `LIMIT` constrains the number of returned rows
- Often used with `ORDER BY` for meaningful results

## 4.3 Advanced SELECT Statements

### 4.3.1 Grouping with GROUP BY

Count orders per customer:

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id;
```

- `GROUP BY` creates groups for aggregation
- `COUNT(*)` counts rows in each group
- Column aliases (`AS order_count`) improve readability

### 4.3.2 Filtering Groups with HAVING

Find customers with 3+ orders:

```sql
SELECT customer_id, COUNT(*)
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 3;
```

- `HAVING` filters after aggregation
- Can use aggregate functions in conditions

### 4.3.3 Combining WHERE and HAVING

Find high-value frequent customers:

```sql
SELECT customer_id, AVG(total) AS avg_order
FROM orders
WHERE order_date >= '2024-01-01'
GROUP BY customer_id
HAVING AVG(total) > 150.00;
```

- `WHERE` filters rows before aggregation
- `HAVING` filters groups after aggregation

## 4.4 Joining Tables

### 4.4.1 INNER JOIN

Get order details with customer information:

```sql
SELECT o.id, o.order_date, c.name, c.email
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```

- Matches records from both tables
- Table aliases (`o` and `c`) simplify notation
- Join condition specifies relationship

### 4.4.2 LEFT JOIN

List all customers with their orders (including those without orders):

```sql
SELECT c.name, o.order_date, o.total
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;
```

- Returns all customers regardless of order existence
- NULL values in order columns for customers without orders

### 4.4.3 Multi-Table JOIN

Get complete order details:

```sql
SELECT o.id, c.name, p.name AS product, oi.quantity
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
JOIN customers c ON o.customer_id = c.id;
```

- Combines data from four tables
- Demonstrates chained joins
- Uses column aliases for clarity

## 4.5 Aggregate Functions and Subqueries

### 4.5.1 Common Aggregate Functions

Calculate product statistics:

```sql
SELECT
  COUNT(*) AS total_products,
  AVG(price) AS average_price,
  MAX(price) AS most_expensive,
  MIN(price) AS cheapest
FROM products;
```

- Multiple aggregates in single query
- Works across all table rows

### 4.5.2 Subquery in WHERE Clause

Find customers who placed orders:

```sql
SELECT name, email
FROM customers
WHERE id IN (SELECT DISTINCT customer_id FROM orders);
```

- Subquery returns list of customer IDs with orders
- `IN` operator checks for membership

### 4.5.3 Correlated Subquery

Get customers' last order date:

```sql
SELECT c.name,
  (SELECT MAX(order_date)
   FROM orders
   WHERE customer_id = c.id) AS last_order
FROM customers c;
```

- Subquery references outer query's `c.id`
- Executed once per customer

## 4.6 Best Practices for DQL

1. **Explicit Column Selection**: Always specify columns instead of using `SELECT *`
1. **Use Standard Joins**: Prefer `JOIN` syntax over comma-separated tables
1. **Limit Early**: Use `WHERE` before `GROUP BY` to reduce processing
1. **Format Consistently**: Use clear indentation and capitalization
1. **Test Components**: Build complex queries incrementally
1. **Analyze Performance**: Use `EXPLAIN` for slow queries (preview of Chapter 8)

## 4.7 Exercises

1. Basic Queries:

   - List all products priced between $10 and $50
   - Show customers from New York sorted alphabetically
   - Display 3 most recent orders

1. Joins and Aggregation:

   - Calculate total revenue per customer
   - Find products never ordered
   - Show average order value by month

1. Subquery Challenges:

   - List customers who spent more than average
   - Find products with above-average price in their category

```

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________
```

```

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________
```
