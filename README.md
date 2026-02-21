# 📚 Online Bookstore SQL Project

## 📌 Project Overview

**Project Title:** Online Bookstore Analysis  
**Level:** Beginner to Intermediate  
**Database:** `OnlineBookstore`

This project demonstrates SQL skills used to design a relational database, import structured CSV data, and perform business-driven analysis.

The project includes:

- Database creation
- Table design with Primary & Foreign Keys
- Data import using CSV files
- Exploratory Data Analysis (EDA)
- Business-driven SQL queries

---

## 📂 Dataset Used

This project uses three CSV files:

- `Books.csv`
- `Customers.csv`
- `Orders.csv`

---

## 🎯 Objectives

1. Set up an Online Bookstore database.
2. Import CSV data into PostgreSQL tables.
3. Perform exploratory data analysis (EDA).
4. Answer real-world business questions using SQL.
5. Analyze revenue, customer behavior, and inventory levels.

---

## 🏗️ Project Structure

### 1️⃣ Database Setup

- **Database Creation**
- **Table Creation**
- **Foreign Key Relationships**

```sql
CREATE DATABASE OnlineBookstore;

-- Switch to the database
\c OnlineBookstore;

CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);

CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);

CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);
```

---

### 2️⃣ Data Import

Data was imported using PostgreSQL's `COPY` command.

```sql
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock)
FROM 'path_to_Books.csv'
CSV HEADER;

COPY Customers(Customer_ID, Name, Email, Phone, City, Country)
FROM 'path_to_Customers.csv'
CSV HEADER;

COPY Orders(Order_ID, Customer_ID, Book_ID, Order_Date, Quantity, Total_Amount)
FROM 'path_to_Orders.csv'
CSV HEADER;
```

---

## 📊 Data Exploration & Analysis

### 🔹 Basic Filtering Queries

```sql
-- Retrieve all Fiction books
SELECT *
FROM Books
WHERE Genre = 'Fiction';

-- Find books published after 1950
SELECT *
FROM Books
WHERE Published_Year > 1950;

-- List customers from Canada
SELECT *
FROM Customers
WHERE Country = 'Canada';

-- Show orders placed in November 2023
SELECT *
FROM Orders
WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30';
```

---

### 🔹 Aggregation Queries

```sql
-- Total stock available
SELECT SUM(Stock) AS Total_Stock
FROM Books;

-- Find the most expensive book
SELECT *
FROM Books
ORDER BY Price DESC
LIMIT 1;

-- Total revenue generated
SELECT SUM(Total_Amount) AS Total_Revenue
FROM Orders;

-- Average price of Fantasy books
SELECT AVG(Price) AS Average_Price
FROM Books
WHERE Genre = 'Fantasy';
```

---

### 🔹 JOIN & Grouping Queries

```sql
-- Total books sold per genre
SELECT 
    b.Genre,
    SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY b.Genre;
```

```sql
-- Customers who placed at least 2 orders
SELECT 
    o.Customer_ID,
    c.Name,
    COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY o.Customer_ID, c.Name
HAVING COUNT(o.Order_ID) >= 2;
```

```sql
-- Most frequently ordered book
SELECT 
    o.Book_ID,
    b.Title,
    COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY o.Book_ID, b.Title
ORDER BY Order_Count DESC
LIMIT 1;
```

---

### 🔹 Inventory Analysis

```sql
-- Calculate remaining stock after fulfilling all orders
SELECT 
    b.Book_ID,
    b.Title,
    b.Stock,
    COALESCE(SUM(o.Quantity), 0) AS Order_Quantity,
    b.Stock - COALESCE(SUM(o.Quantity), 0) AS Remaining_Stock
FROM Books b
LEFT JOIN Orders o
ON b.Book_ID = o.Book_ID
GROUP BY b.Book_ID, b.Title, b.Stock
ORDER BY b.Book_ID;
```

---

## 🚀 SQL Skills Demonstrated

- Database creation and management
- Table design with Primary Keys
- Foreign Key relationships
- Data import using `COPY`
- Filtering using `WHERE`
- Aggregations (`SUM`, `AVG`, `COUNT`)
- `GROUP BY` and `HAVING`
- `INNER JOIN` and `LEFT JOIN`
- `ORDER BY` and `LIMIT`
- `DISTINCT`
- Null handling using `COALESCE`
- Revenue and inventory analysis

---

## 📈 Key Business Insights

- Identified total revenue generated from sales
- Determined most frequently ordered books
- Found repeat and high-spending customers
- Analyzed genre-wise performance
- Calculated remaining stock after order fulfillment

---

## 🛠 Tools Used

- PostgreSQL
- SQL
- CSV Datasets
- Command Line / pgAdmin

---

## 📎 How to Run This Project

1. Install PostgreSQL  
   https://www.postgresql.org/download/

2. Create the database:
   ```sql
   CREATE DATABASE OnlineBookstore;
   ```

3. Create tables using the provided schema.

4. Update file paths and import CSV files using `COPY`.

5. Run analysis queries to generate insights.

---

## 📌 Conclusion

This project demonstrates practical SQL skills including relational database design, data import, business-driven querying, and analytical reporting.

It simulates a real-world online bookstore system and showcases the ability to transform structured CSV data into actionable business insights using SQL.
