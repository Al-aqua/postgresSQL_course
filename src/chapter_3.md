# Chapter 3: Data Manipulation Language (DML)

## 3.1 Introduction to DML

Data Manipulation Language (DML) consists of SQL commands used to manage data within database objects. While DDL focuses on structure, DML operates on the data itself. Key DML operations include:

- `INSERT`: Adds new records to tables
- `UPDATE`: Modifies existing records
- `DELETE`: Removes records from tables
- `SELECT`: Retrieves data (covered in Chapter 5)

DML operations are transactional, meaning they can be rolled back if not committed. This chapter focuses on the first three operations.

______________________________________________________________________

## 3.2 INSERT Statements

### Basic INSERT Syntax

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

**Example: Inserting a new customer**

```sql
INSERT INTO customers (name, email, phone, address)
VALUES (
  'John Doe',
  'john.doe@example.com',
  '123-456-7890',
  '123 Main St, Springfield'
);
```

**Explanation:**

- `INSERT INTO` specifies the target table.
- Column list defines the data order (optional but recommended).
- `VALUES` clause provides corresponding data.
- String values use single quotes.

______________________________________________________________________

### Multiple Row Insertion

```sql
INSERT INTO products (name, description, price, category)
VALUES
  ('Laptop', 'High-performance laptop', 999.99, 'Electronics'),
  ('Mouse', 'Wireless mouse', 19.99, 'Accessories'),
  ('Keyboard', 'Mechanical keyboard', 49.99, 'Accessories');
```

**Explanation:**

- Comma-separated value groups.
- Efficient for bulk inserts.
- Maintains column order.

______________________________________________________________________

## 3.3 UPDATE Statements

### Basic Update Syntax

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**Example: Updating a product price**

```sql
UPDATE products
SET price = price * 0.9
WHERE category = 'Accessories';
```

**Explanation:**

- `SET` modifies specified columns.
- Mathematical expressions allowed.
- `WHERE` clause limits affected rows (crucial!).

______________________________________________________________________

## 3.4 DELETE Statements

### Basic Delete Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

**Example: Removing inactive customers**

```sql
DELETE FROM customers
WHERE email IS NULL;
```

**Explanation:**

- Always use `WHERE` to avoid full table deletion.
- Combines multiple conditions with `AND`.
- Use `IS NULL` to filter records with missing data.

______________________________________________________________________

## 3.5 Using Views

### Creating Views

```sql
CREATE VIEW active_customers AS
SELECT
  id,
  name,
  email,
  phone
FROM customers
WHERE email IS NOT NULL;
```

**Explanation:**

- Virtual table based on a query.
- Automatically updates with base data.
- Simplifies complex queries.

______________________________________________________________________

### Updating Through Views

```sql
UPDATE active_customers
SET phone = '987-654-3210'
WHERE id = 1;
```

**Limitations:**

- Only works for updateable views.
- Cannot modify columns from multiple tables.
- Must maintain underlying constraints.

______________________________________________________________________

## 3.6 Materialized Views

### Creating Materialized Views

```sql
CREATE MATERIALIZED VIEW product_sales_summary AS
SELECT
  p.name AS product_name,
  SUM(oi.quantity) AS total_quantity,
  SUM(oi.quantity * p.price) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.id
GROUP BY p.name;
```

**Key Characteristics:**

- Stores a physical copy of data.
- Requires manual refresh.
- Improves performance for complex queries.

______________________________________________________________________

### Refreshing Materialized Views

```sql
REFRESH MATERIALIZED VIEW product_sales_summary;
```

**Best Practice:**

- Schedule regular refreshes using cron jobs/pgAgent.
- Consider `CONCURRENTLY` option for zero downtime:
  ```sql
  REFRESH MATERIALIZED VIEW CONCURRENTLY product_sales_summary;
  ```

______________________________________________________________________

## 3.7 Data Validation and Constraints

### Common Constraints

```sql
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(id),
  order_date DATE NOT NULL,
  total DECIMAL(10,2) CHECK (total >= 0)
);
```

**Constraint Types:**

- `NOT NULL`: Mandatory field.
- `UNIQUE`: No duplicate values.
- `CHECK`: Custom validation rules.
- `REFERENCES`: Foreign key constraint.

______________________________________________________________________

### Adding Constraints Post-Creation

```sql
ALTER TABLE order_items
ADD CONSTRAINT positive_quantity
CHECK (quantity > 0);
```

**Validation:**

- Existing data must satisfy the constraint.
- Use `NOT VALID` to skip existing data check:
  ```sql
  ALTER TABLE order_items
  ADD CONSTRAINT positive_quantity
  CHECK (quantity > 0) NOT VALID;
  ```

______________________________________________________________________

## 3.8 Best Practices for DML Operations

1. Always use `WHERE` clauses with `UPDATE`/`DELETE`.
1. Validate data before insertion.
1. Regularly maintain materialized views.
1. Implement soft deletes using status flags.
1. Use batch operations for large datasets.
1. Test constraints and triggers thoroughly.

______________________________________________________________________

## 3.9 Exercises

1. Insert 3 new customers into the `customers` table.
1. Update the `phone` column for one of the customers.
1. Delete one of the customers.
1. Add a constraint to ensure the `total` column in the `orders` table is positive.
1. Add a constraint to make sure a customer's `email` column is unique.
1. Insert 10 orders into the `orders` table in one query.

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
