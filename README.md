-- Create database schema
CREATE SCHEMA sales_db;

-- Use the schema
USE sales_db;

-- Create tables
CREATE TABLE stores (
    store_id INT PRIMARY KEY,
    store_name VARCHAR(50)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50)
);

CREATE TABLE sales (
    sale_id INT PRIMARY KEY AUTO_INCREMENT,
    date DATE,
    store_id INT,
    product_id INT,
    quantity INT,
    sales DECIMAL(10, 2),
    FOREIGN KEY (store_id) REFERENCES stores(store_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
-- Insert sample data into stores table
INSERT INTO stores (store_id, store_name) VALUES
(1, 'Store A'),
(2, 'Store B'),
(3, 'Store C');

-- Insert sample data into products table
INSERT INTO products (product_id, product_name) VALUES
(1, 'Product X'),
(2, 'Product Y'),
(3, 'Product Z');

-- Insert sample data into sales table
INSERT INTO sales (date, store_id, product_id, quantity, sales) VALUES
('2024-01-01', 1, 1, 10, 100.00),
('2024-01-01', 2, 2, 20, 200.00),
('2024-01-01', 3, 3, 15, 150.00),
('2024-01-02', 1, 2, 5, 50.00),
('2024-01-02', 2, 1, 30, 300.00),
('2024-01-02', 3, 2, 25, 250.00);
-- Total sales by date
SELECT
    date,
    SUM(sales) AS total_sales
FROM
    sales
GROUP BY
    date
ORDER BY
    date;
-- Total sales by store
SELECT
    st.store_name,
    SUM(s.sales) AS total_sales
FROM
    sales s
JOIN
    stores st ON s.store_id = st.store_id
GROUP BY
    st.store_name
ORDER BY
    total_sales DESC;
-- Monthly sales trend
SELECT
    DATE_FORMAT(date, '%Y-%m') AS month,
    SUM(sales) AS total_sales
FROM
    sales
GROUP BY
    month
ORDER BY
    month;
-- Sales performance by product and store
SELECT
    p.product_name,
    st.store_name,
    SUM(s.sales) AS total_sales
FROM
    sales s
JOIN
    products p ON s.product_id = p.product_id
JOIN
    stores st ON s.store_id = st.store_id
GROUP BY
    p.product_name, st.store_name
ORDER BY
    total_sales DESC;
-- Top 5 products by sales
SELECT
    p.product_name,
    SUM(s.sales) AS total_sales
FROM
    sales s
JOIN
    products p ON s.product_id = p.product_id
GROUP BY
    p.product_name
ORDER BY
    total_sales DESC
LIMIT 5;
-- Export sales data for correlation analysis
SELECT
    date,
    store_id,
    product_id,
    quantity,
    sales
FROM
    sales;
