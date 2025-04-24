# SQL-Data-Analysis-Task3 -- Create and use database
CREATE DATABASE IF NOT EXISTS ecommerce;
USE ecommerce;

-- Customers table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

-- Orders table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Insert sample data
INSERT INTO customers (name, email) VALUES 
('John Doe', 'john@example.com'),
('Jane Smith', 'jane@example.com'),
('Alice Johnson', 'alice@example.com');

INSERT INTO orders (customer_id, order_date, amount) VALUES 
(1, '2024-04-01', 200.00),
(2, '2024-04-02', 150.50),
(1, '2024-04-03', 320.75),
(3, '2024-04-04', 500.00);

-- Basic select
SELECT * FROM customers;

-- Filtering
SELECT * FROM orders WHERE amount > 200;

-- Group By + Aggregate
SELECT customer_id, SUM(amount) AS total_spent
FROM orders
GROUP BY customer_id;

-- Join
SELECT c.name, o.order_date, o.amount
FROM customers c
JOIN orders o ON c.id = o.customer_id;

-- Subquery
SELECT name FROM customers 
WHERE id IN (
    SELECT customer_id FROM orders WHERE amount > 200
);

-- Average revenue per user
SELECT customer_id, AVG(amount) AS avg_revenue
FROM orders
GROUP BY customer_id;

-- Create view
CREATE VIEW customer_orders_view AS
SELECT c.name, o.order_date, o.amount
FROM customers c
JOIN orders o ON c.id = o.customer_id;

-- Create index
CREATE INDEX idx_customer_id ON orders(customer_id);
