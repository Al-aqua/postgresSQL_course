# Chapter 5: Transaction Control Language (TCL)

## 5.1 Introduction to TCL

Transaction Control Language (TCL) manages transactions within a database, ensuring data consistency and integrity. A transaction is a sequence of one or more SQL operations treated as a single logical unit. PostgreSQL uses TCL commands to handle:

- **ACID Properties**:
  - **Atomicity**: All operations succeed or fail as a unit
  - **Consistency**: Transactions bring database from one valid state to another
  - **Isolation**: Concurrent transactions don't interfere
  - **Durability**: Committed changes survive system failures

Key TCL commands:

- `BEGIN`: Starts transaction block
- `COMMIT`: Saves changes permanently
- `ROLLBACK`: Discards changes since last commit
- `SAVEPOINT`: Creates nested transaction points

## 5.2 Importance of Transactions

Example scenario using Chapter 2 tables:

```sql
-- Dangerous partial update scenario
UPDATE products SET stock = stock - 5 WHERE id = 101;  -- Reduce inventory
INSERT INTO orders (customer_id, product_id, quantity)
VALUES (123, 101, 5);  -- Create order record
-- If second statement fails, inventory becomes inaccurate
```

Transactions prevent inventory mismatches:

```sql
BEGIN;
UPDATE products SET stock = stock - 5 WHERE id = 101;
INSERT INTO orders (customer_id, product_id, quantity)
VALUES (123, 101, 5)
RETURNING id;  -- Capture generated order ID
COMMIT;  -- Both operations succeed or fail together
```

## 5.3 TCL Statements

### 5.3.1 BEGIN Transaction

Explicit transaction blocks:

```sql
BEGIN;
INSERT INTO customers (name, email) VALUES ('John Doe', 'john@example.com');
-- Other operations...
```

### 5.3.2 COMMIT

Atomic update of related records:

```sql
BEGIN;
-- Transfer $100 between accounts
UPDATE accounts SET balance = balance - 100 WHERE id = 'ACC001';
UPDATE accounts SET balance = balance + 100 WHERE id = 'ACC002';
COMMIT;  -- Both updates succeed or fail together
```

### 5.3.3 ROLLBACK

Recover from pricing errors:

```sql
BEGIN;
-- Bulk price update with mistake
UPDATE products SET price = price * 1.1 WHERE category = 'electronics';
SELECT COUNT(*) FROM products WHERE price > 10000;  -- Oops, too expensive!
ROLLBACK;  -- Revert all price changes
```

### 5.3.4 SAVEPOINT

Complex order processing with error recovery:

```sql
BEGIN;
INSERT INTO orders (customer_id, total) VALUES (15, 0) RETURNING id;

SAVEPOINT order_created;
INSERT INTO order_items (order_id, product_id, quantity)
VALUES (currval('orders_id_seq'), 99, 2);

-- Discover discontinued product
ROLLBACK TO order_created;  -- Remove invalid order item
INSERT INTO order_items (order_id, product_id, quantity)
VALUES (currval('orders_id_seq'), 104, 2);

COMMIT;
```

## 5.4 Transaction Isolation Levels

PostgreSQL supports four isolation levels (default: READ COMMITTED):

| Level | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
| ---------------- | ----------- | -------------------- | ------------- |
| READ UNCOMMITTED | Allowed | Possible | Possible |
| READ COMMITTED | Prevented | Possible | Possible |
| REPEATABLE READ | Prevented | Prevented | Possible |
| SERIALIZABLE | Prevented | Prevented | Prevented |

Set isolation level for financial reporting:

```sql
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT SUM(balance) FROM accounts AS of_date;
SELECT * FROM transactions WHERE date <= CURRENT_DATE;
COMMIT;
```

## 5.5 Best Practices for Transaction Management

1. **Keep Transactions Focused**

   ```sql
   -- Good: Tight financial transaction
   BEGIN;
   UPDATE accounts SET balance = balance - 200 WHERE id = 'CHECKING';
   UPDATE accounts SET balance = balance + 200 WHERE id = 'SAVINGS';
   COMMIT;

   -- Bad: Mixing operational and reporting logic
   BEGIN;
   UPDATE inventory SET count = count - 10 WHERE id = 5;
   GENERATE inventory_report();  -- Long-running operation
   COMMIT;
   ```

1. **Use Savepoints for Multi-Step Processes**

   ```sql
   BEGIN;
   INSERT INTO patients (name, admission_date) VALUES ('John Doe', NOW());

   SAVEPOINT lab_work_started;
   INSERT INTO lab_orders (patient_id, test_type) VALUES (currval('patients_id_seq'), 'blood');
   -- If lab system unavailable:
   ROLLBACK TO lab_work_started;
   INSERT INTO lab_orders (patient_id, test_type) VALUES (currval('patients_id_seq'), 'urine');

   COMMIT;
   ```

1. **Avoid Empty Transactions**

   ```sql
   -- Anti-pattern: Transaction without data modification
   BEGIN;
   SELECT * FROM audit_log WHERE event_type = 'login';
   COMMIT;

   -- Proper usage: Read-only needs no transaction
   SELECT * FROM audit_log WHERE event_type = 'login';
   ```

## 5.6 Common TCL Scenarios

### Scenario 1: E-commerce Order Finalization

```sql
BEGIN;
-- Reserve inventory
WITH inventory_update AS (
    UPDATE products
    SET stock = stock - 2
    WHERE id = 305 AND stock >= 2
    RETURNING id
)
INSERT INTO orders (user_id, product_id, quantity)
SELECT 55, id, 2
FROM inventory_update;

-- Payment processing
INSERT INTO payments (order_id, amount, method)
VALUES (currval('orders_id_seq'), 199.98, 'credit_card');

COMMIT;
```

### Scenario 2: Batch Processing with Savepoints

```sql
BEGIN;
SAVEPOINT clean_start;

-- Update customer addresses
UPDATE customers
SET address = '123 Main St'
WHERE zip_code = '90210'
SAVEPOINT address_updated;

-- Handle potential constraint violations
BEGIN
    UPDATE customers
    SET phone = '+1-555-1234'
    WHERE zip_code = '90210';
EXCEPTION WHEN check_violation THEN
    ROLLBACK TO address_updated;
    UPDATE customers
    SET phone = '555-1234'
    WHERE zip_code = '90210';
END;

COMMIT;
```

### Scenario 3: Data Archiving Transaction

```sql
BEGIN;
-- Archive old orders
INSERT INTO orders_archive
SELECT * FROM orders WHERE created_at < '2023-01-01';

-- Delete archived records
DELETE FROM orders WHERE created_at < '2023-01-01';

-- Update audit trail
INSERT INTO system_events (description)
VALUES ('Archived pre-2023 orders');

COMMIT;
```

## 5.7 Exercises

1. **Basic Transaction**

   - Begin transaction
   - Insert new customer
   - Update email address
   - Commit transaction

1. **Savepoint Practice**

   - Begin transaction
   - Insert new order
   - Savepoint order creation
   - Update product price
   - Rollback to order creation
   - Commit transaction

```

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

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________
```
