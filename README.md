-- Project Description

-- RETAIL SALES ANALYSIS using MySQL

-- Developed a MySQL database solution for BrightMart Retail Pvt. Ltd. to help the Sales Analytics Department
-- analyze customer orders, product performance, and revenue trends. Designed relational tables, established
-- foreign key relationships, and created SQL reports to identify top-selling products, high-value customers,
-- city-wise revenue contribution, and monthly sales performance.

-- Company Overview  {{BrightMart Retail Pvt. Ltd.}}
-- BrightMart Retail Pvt. Ltd. is a growing electronics and home appliances retailer operating across multiple cities in India. 
-- The company sells products through physical stores and online channels.

-- Sales Analytics Department 

-- Employee Assigned
-- Employee Name: Rahul Sharma
-- Designation: MySQL Developer
-- Employee ID: EMP001
-- Department: Sales Analytics

-- Business Problem ::
-- BrightMart stores all customer orders in spreadsheets.
-- As the number of orders increased, management struggled to answer important business questions:
-- Which products sell the most?
-- Which products generate the highest revenue?
-- Which cities contribute the most sales?
-- Which customers are most valuable?
-- What are the monthly revenue trends?

-- Rahul Sharma has been assigned to build a MySQL database and generate reports for management.


CREATE DATABASE brightmart_sales_db;
USE brightmart_sales_db;

-- Table 1: Customers

CREATE TABLE customers (
customer_id INT PRIMARY KEY,
customer_name VARCHAR(100),
city VARCHAR(50)
);
INSERT INTO customers VALUES
(1,'Amit Patel','Mumbai'),
(2,'Priya Singh','Delhi'),
(3,'Raj Verma','Pune'),
(4,'Neha Kapoor','Bangalore'),
(5,'Arjun Mehta','Chennai'),
(6,'Sneha Joshi','Mumbai'),
(7,'Vikram Shah','Ahmedabad'),
(8,'Riya Gupta','Delhi'),
(9,'Karan Malhotra','Pune'),
(10,'Ananya Rao','Hyderabad');


-- Table 2: Products

CREATE TABLE products (
product_id INT PRIMARY KEY,
product_name VARCHAR(100),
category VARCHAR(50),
price DECIMAL(10,2)
);

INSERT INTO products VALUES
(101,'Laptop','Electronics',55000),
(102,'Smartphone','Electronics',25000),
(103,'Tablet','Electronics',18000),
(104,'Smart Watch','Electronics',12000),
(105,'Headphones','Accessories',3000),
(106,'Refrigerator','Appliances',45000),
(107,'Washing Machine','Appliances',30000),
(108,'Microwave Oven','Appliances',15000),
(109,'Air Conditioner','Appliances',40000),
(110,'Bluetooth Speaker','Accessories',5000);


-- Table 3: Orders

CREATE TABLE orders (
order_id INT PRIMARY KEY,
customer_id INT,
product_id INT,
quantity INT,
order_date DATE,
FOREIGN KEY(customer_id) REFERENCES customers(customer_id),
FOREIGN KEY(product_id) REFERENCES products(product_id)
);

INSERT INTO orders VALUES
(1001,1,101,1,'2025-01-05'),
(1002,2,102,2,'2025-01-10'),
(1003,3,103,1,'2025-01-12'),
(1004,4,104,2,'2025-01-15'),
(1005,5,105,3,'2025-01-20'),
(1006,6,106,1,'2025-02-01'),
(1007,7,107,1,'2025-02-05'),
(1008,8,108,2,'2025-02-08'),
(1009,9,109,1,'2025-02-12'),
(1010,10,110,4,'2025-02-15'),
(1011,1,102,1,'2025-03-01'),
(1012,2,103,2,'2025-03-04'),
(1013,3,104,1,'2025-03-10'),
(1014,4,105,5,'2025-03-12'),
(1015,5,106,1,'2025-03-15'),
(1016,6,107,2,'2025-03-18'),
(1017,7,108,1,'2025-03-20'),
(1018,8,109,1,'2025-03-22'),
(1019,9,110,3,'2025-03-25'),
(1020,10,101,1,'2025-03-28');


-- SQL Challenges

-- Beginner Level

-- 1. Display all customers
SELECT * FROM customers;

-- 2. Display all products
SELECT * FROM products;

-- 3. Display all orders
SELECT * FROM orders;

-- 4. Show customers from Mumbai
SELECT * FROM customers
WHERE city='Mumbai';

-- 5. Show Electronics products
SELECT * FROM products
WHERE category='Electronics';


-- Intermediate Level

-- 6. Count total customers
SELECT COUNT(*) AS total_customers
FROM customers;

-- 7. Count total products
SELECT COUNT(*) AS total_products
FROM products;

-- 8. Find Highest price products
SELECT *
FROM products
ORDER BY price DESC
LIMIT 1;

-- 9. Find Lowest price products
SELECT *
FROM products
ORDER BY price ASC
LIMIT 1;

-- 10. Total quantity sold
SELECT SUM(quantity) AS total_quantity_sold
FROM orders;

-- Business Reporting Level

-- 11. Total orders by customer
SELECT customer_id,
COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id;

-- 12. Total quantity sold by product
SELECT product_id,
SUM(quantity) AS quantity_sold
FROM orders
GROUP BY product_id;

-- 13. Revenue by product
SELECT
p.product_name,
SUM(o.quantity*p.price) AS revenue
FROM orders o
JOIN products p
ON o.product_id=p.product_id
GROUP BY p.product_name;

-- 14. Top-selling product
SELECT
p.product_name,
SUM(o.quantity) AS total_sold
FROM orders o
JOIN products p
ON o.product_id=p.product_id
GROUP BY p.product_name
ORDER BY total_sold DESC
LIMIT 1;

-- 15. Customer spending analysis
SELECT
c.customer_name,
SUM(o.quantity*p.price) AS total_spent
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN products p
ON o.product_id=p.product_id
GROUP BY c.customer_name;

-- 16. Highest spending customer
SELECT
c.customer_name,
SUM(o.quantity*p.price) AS total_spent
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN products p
ON o.product_id=p.product_id
GROUP BY c.customer_name
ORDER BY total_spent DESC
LIMIT 1;

-- 17. Revenue by city
SELECT
c.city,
SUM(o.quantity*p.price) AS revenue
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN products p
ON o.product_id=p.product_id
GROUP BY c.city;

-- 18. Monthly revenue
SELECT
MONTH(order_date) AS month_no,
SUM(o.quantity*p.price) AS revenue
FROM orders o
JOIN products p
ON o.product_id=p.product_id
GROUP BY MONTH(order_date);

-- 19. Revenue by category
SELECT
p.category,
SUM(o.quantity*p.price) AS revenue
FROM orders o
JOIN products p
ON o.product_id=p.product_id
GROUP BY p.category;

-- 20. Top 3 customers by spending
SELECT
c.customer_name,
SUM(o.quantity*p.price) AS spending
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN products p
ON o.product_id=p.product_id
GROUP BY c.customer_name
ORDER BY spending DESC
LIMIT 3;

