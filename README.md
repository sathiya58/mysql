-- Step 1: Create the database
CREATE DATABASE ecommerce;

-- Step 2: Use the created database
USE ecommerce;

-- Step 3: Create the tables

-- Customers Table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    address VARCHAR(255)
);

-- Orders Table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Products Table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT
);

-- Order_Items Table for normalization
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT DEFAULT 1,
    price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Step 4: Insert sample data

-- Insert sample data into customers
INSERT INTO customers (name, email, address) VALUES
('sathiyapriya', 'sathiya@example.com', '123 kovil St'),
('suresh', 'suresh@example.com', '456 kovil Ave'),
('teju', 'teju@example.com', '789 kovil st');

-- Insert sample data into products
INSERT INTO products (name, price, description) VALUES
('Product A', 25.00, 'Description of Product A'),
('Product B', 35.00, 'Description of Product B'),
('Product C', 40.00, 'Description of Product C');

-- Insert sample data into orders
INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, '2024-10-10', 200.00),
(2, '2024-11-01', 150.00),
(3, '2024-11-05', 300.00);

-- Insert sample data into order_items
INSERT INTO order_items (order_id, product_id, quantity, price) VALUES
(1, 1, 2, 25.00),  -- 2 units of Product A in Order 1
(1, 2, 1, 35.00),  -- 1 unit of Product B in Order 1
(2, 3, 2, 45.00),  -- 2 units of Product C in Order 2 (after price update)
(3, 1, 3, 25.00);  -- 3 units of Product A in Order 3

-- Step 5: Queries

-- 1. Retrieve all customers who have placed an order in the last 30 days.
SELECT DISTINCT customers.*
FROM customers
JOIN orders ON customers.id = orders.customer_id
WHERE orders.order_date >= CURDATE() - INTERVAL 30 DAY;

-- 2. Get the total amount of all orders placed by each customer.
SELECT customer_id, SUM(total_amount) AS total_spent
FROM orders
GROUP BY customer_id;

-- 3. Update the price of Product C to 45.00.
UPDATE products
SET price = 45.00
WHERE name = 'Product C';

-- 4. Add a new column discount to the products table.
ALTER TABLE products
ADD COLUMN discount DECIMAL(5, 2) DEFAULT 0.00;

-- 5. Retrieve the top 3 products with the highest price.
SELECT * FROM products
ORDER BY price DESC
LIMIT 3;

-- 6. Get the names of customers who have ordered Product A.
SELECT DISTINCT customers.name
FROM customers
JOIN orders ON customers.id = orders.customer_id
JOIN order_items ON orders.id = order_items.order_id
JOIN products ON order_items.product_id = products.id
WHERE products.name = 'Product A';

-- 7. Join the orders and customers tables to retrieve the customer's name and order date for each order.
SELECT customers.name, orders.order_date
FROM orders
JOIN customers ON orders.customer_id = customers.id;

-- 8. Retrieve the orders with a total amount greater than 150.00.
SELECT * FROM orders
WHERE total_amount > 150.00;

-- 9. Retrieve the average total of all orders.
SELECT AVG(total_amount) AS average_order_total
FROM orders;
![alt text](<Screenshot 2024-11-11 210621.png>)
![alt text](<Screenshot 2024-11-12 180710.png>)
![alt text](<Screenshot 2024-11-12 180723.png>)
![alt text](<Screenshot 2024-11-12 180749.png>)
![alt text](<Screenshot 2024-11-12 180817.png>)
![alt text](<Screenshot 2024-11-12 180834.png>)
![alt text](<Screenshot 2024-11-12 181825.png>)
