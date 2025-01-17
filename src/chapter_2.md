# Chapter 2: Data Definition Language (DDL)

## 2.1 Introduction to DDL

Data Definition Language (DDL) is a subset of SQL that is used to create, modify, and delete database structures, such as tables, indexes, and constraints. DDL statements are used to define the structure of a database, including the relationships between tables and the data types of each column.

Some common DDL statements include:

- `CREATE`: Used to create a new database object, such as a table or index.
- `ALTER`: Used to modify an existing database object, such as adding or removing columns from a table.
- `DROP`: Used to delete a database object, such as a table or index.
- `TRUNCATE`: Used to delete all data from a table and reset the auto-incrementing ID.

## 2.2 PostgreSQL Data Types

Before creating tables, it's essential to understand the different data types available in PostgreSQL. Data types determine the type of data that can be stored in a column, and they play a crucial role in ensuring data consistency and accuracy.

Here are some of the most common data types in PostgreSQL:

- **Integer Types**:
  - `SMALLINT`: A small integer, typically 2 bytes in size.
  - `INTEGER`: A standard integer, typically 4 bytes in size.
  - `BIGINT`: A large integer, typically 8 bytes in size.
- **Numeric Types**:
  - `DECIMAL`: A decimal number with a specified precision and scale.
  - `NUMERIC`: A numeric value with a specified precision and scale.
  - `REAL`: A single-precision floating-point number.
  - `DOUBLE PRECISION`: A double-precision floating-point number.
- **Character Types**:
  - `CHAR`: A fixed-length character string.
  - `VARCHAR`: A variable-length character string.
  - `TEXT`: A character string with no specific length limit.
- **Date and Time Types**:
  - `DATE`: A date value.
  - `TIME`: A time value.
  - `TIMESTAMP`: A timestamp value, which includes both date and time.
  - `INTERVAL`: An interval value, which represents a duration of time.
- **Boolean Type**:
  - `BOOLEAN`: A boolean value, which can be either `TRUE` or `FALSE`.
- **Array Type**:
  - `ARRAY`: An array of values, which can be of any data type.
- **JSON Type**:
  - `JSON`: A JSON (JavaScript Object Notation) value.
  - `JSONB`: A binary JSON value.

## 2.3 Creating Databases and Tables

To create a new database, you can use the `CREATE DATABASE` statement:

```sql
CREATE DATABASE mydatabase;
```

To create a new table, you can use the `CREATE TABLE` statement:

```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100),
  phone VARCHAR(20)
);
```

In this example, we create a new table called `customers` with four columns: `id`, `name`, `email`, and `phone`. The `id` column is defined as a primary key, which means it will uniquely identify each row in the table.

## 2.4 Understanding Primary and Foreign Keys

A primary key is a column or set of columns that uniquely identifies each row in a table. A foreign key is a column or set of columns that references the primary key of another table.

For example, consider two tables: `orders` and `customers`. The `orders` table has a foreign key column called `customer_id` that references the `id` column in the `customers` table:

```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  email VARCHAR(100),
  phone VARCHAR(20)
);

CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INTEGER,
  order_date DATE,
  total DECIMAL(10, 2),
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

In this example, the `customer_id` column in the `orders` table is a foreign key that references the `id` column in the `customers` table.

## 2.5 Altering and Dropping Database Objects

To modify an existing database object, you can use the `ALTER` statement:

```sql
ALTER TABLE customers
ADD COLUMN address VARCHAR(100);
```

To delete a database object, you can use the `DROP` statement:

```sql
DROP TABLE customers;
```

Note that dropping a table will delete all data in the table and cannot be undone.

## 2.6 Best Practices for Database Design

Here are some best practices for DDL:

- Use meaningful and descriptive table and column names.
- Use foreign keys to establish relationships between tables.

## 2.7 Exercises

1. Create a new database and name it `<your_name>_shop`.

1. Connect to the database using the `psql` command-line tool.

1. Create a new table called `customers` with the following columns:

   - `id` (primary key)
   - `name`
   - `email`
   - `phone`

1. Alter the `customers` table to add a new column called `address`.

1. Create a new table called `orders` with the following columns:

   - `id` (primary key)
   - `customer_id` (foreign key)
   - `order_date`
   - `total`

1. Create a new table called `products` with the following columns:

   - `id` (primary key)
   - `name`
   - `description`
   - `price`

1. Alter the `products` table to add a new column called `category`.

1. Create a new table called `order_items` with the following columns:

   - `id` (primary key)
   - `order_id` (foreign key)
   - `product_id` (foreign key)
   - `quantity`

```

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

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________

_____________________________________________________________________________
```
