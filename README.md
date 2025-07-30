Sure Nimit! Here's a **clean and professional README** you can use for your **Bookstore Database Project**. It explains the database design, inserted data, and all the sample SQL queries you've run.

---

# 📚 Bookstore Database Project – SQL

## 🧾 Overview

This project represents a relational database for a **Bookstore**, designed using **MySQL**. It includes essential entities like books, authors, customers, orders, payments, reviews, and publishers. The schema supports data analysis, reporting, and transactional queries.

---

## 📁 Database Name

```sql
CREATE DATABASE bookstore;
USE bookstore;
```

---

## 🗂️ Tables Created

### 1. Authors

Stores information about book authors.

| Column     | Type         | Description       |
| ---------- | ------------ | ----------------- |
| author\_id | INT          | Primary Key       |
| name       | VARCHAR(100) | Author name       |
| country    | VARCHAR(50)  | Country of origin |

---

### 2. Publishers

Details of book publishers.

| Column        | Type         | Description    |
| ------------- | ------------ | -------------- |
| publisher\_id | INT          | Primary Key    |
| name          | VARCHAR(100) | Publisher name |
| country       | VARCHAR(50)  | Country        |

---

### 3. Books

Information about books available in stock.

| Column            | Type          | Description                    |
| ----------------- | ------------- | ------------------------------ |
| book\_id          | INT           | Primary Key                    |
| title             | VARCHAR(150)  | Book title                     |
| author\_id        | INT (FK)      | Refers to Authors              |
| genre             | VARCHAR(50)   | Genre (e.g., Romance, Fiction) |
| price             | DECIMAL(10,2) | Price                          |
| stock\_quantity   | INT           | Available stock                |
| publisher\_id     | INT (FK)      | Refers to Publishers           |
| publication\_year | YEAR          | Year of publication            |

---

### 4. Customers

Customer profile data.

| Column       | Type         | Description    |
| ------------ | ------------ | -------------- |
| customer\_id | INT          | Primary Key    |
| name         | VARCHAR(100) | Customer name  |
| email        | VARCHAR(100) | Unique email   |
| phone        | VARCHAR(15)  | Contact number |
| address      | VARCHAR(200) | Street address |
| city         | VARCHAR(50)  | City           |

---

### 5. Orders

Customer orders placed.

| Column        | Type          | Description         |
| ------------- | ------------- | ------------------- |
| order\_id     | INT           | Primary Key         |
| customer\_id  | INT (FK)      | Refers to Customers |
| order\_date   | DATE          | Date of order       |
| total\_amount | DECIMAL(10,2) | Total order value   |

---

### 6. Order\_Items

Items included in an order.

| Column    | Type          | Description       |
| --------- | ------------- | ----------------- |
| order\_id | INT (FK)      | Refers to Orders  |
| book\_id  | INT (FK)      | Refers to Books   |
| quantity  | INT           | Quantity of books |
| price     | DECIMAL(10,2) | Price per unit    |

---

### 7. Payments

Payment details for orders.

| Column        | Type          | Description           |
| ------------- | ------------- | --------------------- |
| payment\_id   | INT           | Primary Key           |
| order\_id     | INT (FK)      | Refers to Orders      |
| payment\_date | DATE          | Payment date          |
| amount        | DECIMAL(10,2) | Paid amount           |
| payment\_mode | VARCHAR(50)   | UPI, Card, Cash, etc. |
| status        | VARCHAR(20)   | Paid / Pending        |

---

### 8. Reviews

Customer reviews and ratings for books.

| Column       | Type      | Description         |
| ------------ | --------- | ------------------- |
| review\_id   | INT       | Primary Key         |
| book\_id     | INT (FK)  | Refers to Books     |
| customer\_id | INT (FK)  | Refers to Customers |
| rating       | INT (1-5) | Star rating         |
| review       | TEXT      | Review text         |
| review\_date | DATE      | Date of review      |

---

## 📌 Sample Queries

### 🔍 Basic Retrieval

```sql
SELECT price, stock_quantity FROM books;
```

### 🔍 Books by Chetan Bhagat

```sql
SELECT title, price ,genre, stock_quantity
FROM books AS b
JOIN authors AS a ON b.author_id = a.author_id
WHERE a.name = 'Chetan Bhagat';
```

### 🔍 Customers from Delhi

```sql
SELECT * FROM customers WHERE city = 'Delhi';
```

### 🔍 Books published after 2020

```sql
SELECT * FROM books WHERE publication_year > 2020;
```

### 🔍 Books within a price range

```sql
SELECT * FROM books WHERE price BETWEEN 300 AND 700;
```

---

### 📦 Inventory + Authors

```sql
SELECT * FROM books AS b
JOIN authors AS a ON a.author_id = b.author_id;
```

---

### 🛒 Order info with customer name

```sql
SELECT o.*, c.name 
FROM orders AS o 
JOIN customers AS c ON o.customer_id = c.customer_id;
```

---

### 📦 Sold books with order ID

```sql
SELECT b.title, b.stock_quantity, o.order_id 
FROM books AS b 
JOIN order_items AS o ON b.book_id = o.book_id;
```

---

### 💰 Orders with payment status

```sql
SELECT 
    O.order_id,
    O.total_amount,
    P.status AS payment_status
FROM 
    Orders O
JOIN 
    Payments P ON O.order_id = P.order_id;
```

---

### 📦 Stock with Publisher Name

```sql
SELECT stock_quantity, price, p.name 
FROM books AS b 
JOIN publishers AS p ON b.publisher_id = p.publisher_id;
```

---

### 📊 Books sold by genre

```sql
SELECT 
    B.genre,
    SUM(OI.quantity) AS total_books_sold
FROM 
    Order_Items OI
JOIN 
    Books B ON OI.book_id = B.book_id
GROUP BY 
    B.genre;
```

---

### 🏆 Top 3 Customers by Spending

```sql
SELECT 
    C.customer_id,
    C.name,
    SUM(O.total_amount) AS total_spent
FROM 
    Customers C
JOIN 
    Orders O ON C.customer_id = O.customer_id
GROUP BY 
    C.customer_id, C.name
ORDER BY 
    total_spent DESC
LIMIT 3;
```

---

## 📌 Notes

* Use `InnoDB` storage engine for foreign key support.
* Foreign keys are properly defined to maintain referential integrity.
* All monetary values are stored using `DECIMAL(10,2)` for precision.
* Ratings are validated using a `CHECK` constraint between 1 and 5.

---

## ✅ Ideal Use Cases

* Sales analysis by genre, author, or publisher.
* Customer spending insights.
* Inventory tracking and forecasting.
* Payment reconciliation and pending reports.

---

