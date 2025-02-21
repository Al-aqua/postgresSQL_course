# Databases Management Systems Midterm Exam

<div lang="ar" dir="rtl">

# امتحان نظام إدارة قواعد البيانات منتصف الفصل

</div>

by [Mohammad Al-aqua](https://github.com/al-aqua)

### Task Overview

You will create a PostgreSQL database with four tables.  
You are required to define validations and constraints for each table,  
insert sample data, and then display the contents of the tables.

<div lang="ar" dir="rtl">

ستقوم بإنشاء قاعدة بيانات PostgreSQL تحتوي على أربعة جداول.
مطلوب منك تعريف التحقق والقيود لكل جدول،  
إدخال بيانات عينة، ثم عرض محتويات الجداول.

</div>

---

### 1. Database Creation

<div lang="ar" dir="rtl">

#### 1. إنشاء القاعدة البيانية

</div>

- **Step 1:** Create a database named `library`.
- **Step 2:** Connect to the `library` database.

<div lang="ar" dir="rtl">

- **الخطوة 1:** أنشئ قاعدة بيانات باسم `library`.
- **الخطوة 2:** اتصل بقاعدة البيانات `library`.

</div>

---

### 2. Table Definitions and Constraints

<div lang="ar" dir="rtl">

#### 2. تعريفات الجداول والقيود

</div>

Create the following tables with appropriate validations and constraints:

<div lang="ar" dir="rtl">
أنشئ الجداول التالية مع التحقق والقيود المناسبة:
</div>

#### a. `publishers` Table

- **Columns:**
  - `id` – INTEGER, PRIMARY KEY
  - `name` – VARCHAR(255)

#### b. `books` Table

- **Columns:**
  - `id` – INTEGER, PRIMARY KEY
  - `publisher_id` – INTEGER, FOREIGN KEY referencing `publishers(id)`
  - `title` – VARCHAR(255)
  - `publication_year` – INTEGER

#### c. `authors` Table

- **Columns:**
  - `id` – INTEGER, PRIMARY KEY
  - `name` – VARCHAR(255)

#### d. `book_authors` Table

- **Columns:**
  - `id` – INTEGER, PRIMARY KEY
  - `book_id` – INTEGER, FOREIGN KEY referencing `books(id)`
  - `author_id` – INTEGER, FOREIGN KEY referencing `authors(id)`

---

### 3. Data Insertion

<div lang="ar" dir="rtl">

#### 3. إدخال البيانات

</div>

Insert the following sample data into the tables:

<div lang="ar" dir="rtl">
أدخل البيانات العينة التالية في الجداول:

</div>

#### a. Publishers

| id  | name    |
| --- | ------- |
| 1   | Penguin |
| 2   | Harper  |

#### b. Books

| id  | publisher_id | title                              | publication_year |
| --- | ------------ | ---------------------------------- | ---------------- |
| 1   | 1            | The Catcher in the Rye             | 1951             |
| 2   | 1            | To Kill a Mockingbird              | 1960             |
| 3   | 2            | Pride and Prejudice                | 1813             |
| 4   | 2            | Alice's Adventures in Wonderland   | 1865             |
| 5   | 2            | The Adventures of Huckleberry Finn | 1884             |
| 6   | 1            | The Adventures of Sherlock Holmes  | 1887             |
| 7   | 1            | The Adventures of Tom Sawyer       | 1876             |
| 8   | 1            | The Adventures of Huckleberry Finn | 1884             |
| 9   | 1            | Alice's Adventures in Wonderland   | 1865             |
| 10  | 1            | Pride and Prejudice                | 1813             |

#### c. Authors

| id  | name                |
| --- | ------------------- |
| 1   | J.D. Salinger       |
| 2   | F. Scott Fitzgerald |
| 3   | Jane Austen         |
| 4   | Charles Dickens     |
| 5   | Mark Twain          |

#### d. Book Authors

| id  | book_id | author_id |
| --- | ------- | --------- |
| 1   | 1       | 1         |
| 2   | 2       | 3         |
| 3   | 3       | 3         |
| 4   | 4       | 5         |
| 5   | 5       | 4         |
| 6   | 6       | 4         |
| 7   | 7       | 2         |
| 8   | 8       | 1         |
| 9   | 9       | 5         |
| 10  | 10      | 2         |

---

### 4. Viewing the Data

Finally, display the data contained in each table (`publishers`, `books`, `authors`, and `book_authors`) to verify your results.

<div lang="ar" dir="rtl">
أخيراً، عرض البيانات الموجودة في كل جدول (`publishers`، `books`، `authors`، و `book_authors`) لتأكيد النتائج.

</div>

---

Good job! You have successfully completed this midterm exam.
I hope you enjoyed it ;)
