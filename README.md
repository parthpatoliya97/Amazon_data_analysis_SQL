
## SQL Project - Amazon Sales Analysis
![amazon_logo](https://www.amalytix.com/blog/amazon_ai_features_cover.jpg)

- This project analyzes Amazon sales data from 2021 to 2024 to extract valuable business insights.
The aim of this project is to showcase end-to-end SQL skills — from database design and querying, to advanced concepts like window functions, stored procedures, and CTEs.

- The project uses sample data generated with Python’s Faker library to simulate real-world Amazon transactions.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Tech Stack :-

- Database: MySQL

- Data Generation: Python (Faker library)

- Tools: MySQL Workbench, VS Code, GitHub, dbdiagram.io

------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#### Beginner Concepts :-

- Database creation & table design

- Data retrieval & filtering (SELECT, WHERE)

- Aggregations (GROUP BY, HAVING, SUM, COUNT)

- Multi-table JOINs
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Intermediate Concepts :-

- Conditional logic (CASE WHEN, IF ELSE)

- Date functions (YEAR, MONTH, QUARTER, DATEDIFF, DAYNAME, YEARWEEK)

- Generating synthetic sales data with Python Faker
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Advanced Concepts :-

- Window functions (RANK, DENSE_RANK, ROW_NUMBER / LEAD, LAG for growth trends)

- Common Table Expressions (CTEs, Recursive CTEs)

- Stored Procedures for automating sales & inventory updates

- Pivoting data for reports
------------------------------------------------------------------------------------------------------------------------------------------------------------------
### ER Diagram :-
![ER_Diagram](https://github.com/parthpatoliya97/Amazon_data_analysis_SQL/blob/main/ER-Diagram.png?raw=true)

#### Category Table :-

- category_id: Unique ID for each category.

- category_name: Name of the category.
----------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Seller Table :-

- seller_id: Unique ID for each seller.

- seller_name: Name of the seller.

- origin: Seller’s location (e.g., USA, India).
----------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Products Table :-

- product_id: Unique ID for each product.

- product_name: Name/description of the product.

- price: Selling price.

- cogs: Cost of Goods Sold (helps calculate profit margins).

- category_id: Linked to Category table to classify products.
----------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Customers Table :-

- customer_id: Unique ID for each customer.

- first_name / last_name: Customer details.

- state: Customer’s state/region (useful for regional sales insights).
----------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Orders Table :-

- order_id: Unique ID for each order.

- order_date: Date when the order was placed.

- customer_id: Linked to Customers table.

- seller_id: Linked to Seller table.

- order_status: Status of order (e.g., Completed, Cancelled, Pending).
----------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Order_Items Table :-

- order_item_id: Unique ID for each order item.

- order_id: Linked to Orders table.

- product_id: Linked to Products table.

- quantity: Number of units ordered.

- price_per_unit: Price of one unit at purchase time.

- total_price: Final amount for that item (quantity × price_per_unit).
----------------------------------------------------------------------------------------------------------------------------------------------------------------
#### Payments Table :-

- payment_id: Unique ID for payment.

- order_id: Linked to Orders table.

- payment_date: Date when payment was made.

- payment_mode: Mode of payment (e.g., Credit Card, UPI, Net Banking).

- payment_status: Status (e.g., Success, Failed, Pending).
----------------------------------------------------------------------------------------------------------------------------------------------------------------
#### Shipping Table :-

- shipping_id: Unique ID for shipping record.

- order_id: Linked to Orders table.

- shipping_date: Date product was shipped.

- shipping_provider: Courier partner (e.g., FedEx, Amazon Logistics).

- delivery_status: Status (Delivered, In Transit, Returned).
----------------------------------------------------------------------------------------------------------------------------------------------------------------
#### Inventory Table :-

- inventory_id: Unique ID for stock record.

- product_id: Linked to Products table.

- stock: Current stock quantity available.

- warehouse_id: ID of warehouse storing the product.

- last_stock_date: Last updated date of stock.
----------------------------------------------------------------------------------------------------------------------------------------------------------------
#### In short:

- Master Data: category, products, customers, seller

- Transactional Data: orders, order_items

- Operational Data: payments, shipping, inventory

#### 1. List all customers from California.
```sql
SELECT * 
FROM customers
WHERE state = 'California';
```

#### 2. Show all products in the 'electronics' category.
```sql
SELECT c.category_name, p.product_name
FROM category c
JOIN products p ON c.category_id = p.category_id
WHERE c.category_name = 'electronics';
```

#### 3. Find all orders with a 'Cancelled' status.
```sql
SELECT * 
FROM orders
WHERE order_status = 'Cancelled';
```

#### 4. Count the total number of sellers from the 'USA'.
```sql
SELECT COUNT(*) 
FROM seller
WHERE origin = 'USA';
```

#### 5. Display the top 5 most expensive products.
```sql
SELECT * 
FROM products
ORDER BY price DESC
LIMIT 5;
```
#### 6. Find all payments made via 'PayPal'.
```sql
SELECT * 
FROM payments
WHERE payment_mode = 'PayPal';
```

#### 7. List all orders placed in the year 2023.
```sql
SELECT * 
FROM orders
WHERE YEAR(order_date) = 2023;
```

#### 8. Show the product names and their categories.
```sql
SELECT p.product_name, c.category_name
FROM products p 
JOIN category c ON p.category_id = c.category_id;
```

#### 9. Find customers whose first name starts with 'A'.
```sql
SELECT * 
FROM customers
WHERE first_name LIKE 'A%';
```

#### 10. Count how many orders each customer has placed.
```sql
SELECT customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id;
```

#### 11. Show all shipping records where the delivery status is 'Delivered'.
```sql
SELECT * 
FROM shipping
WHERE delivery_status = 'Delivered';
```

#### 12. Find the average price of all products.
```sql
SELECT ROUND(AVG(price), 2) AS avg_price
FROM products;
```

#### 13. List all orders along with the customer's first and last name.
```sql
SELECT o.*, c.first_name, c.last_name
FROM orders o 
JOIN customers c ON o.customer_id = c.customer_id;
```

#### 14. Show products with a price between $50 and $100.
```sql
SELECT * 
FROM products
WHERE price BETWEEN 50 AND 100;
```

#### 15. Find the total number of items sold for each product.
```sql
SELECT p.product_id, p.product_name, SUM(oi.quantity) AS total_items
FROM order_items oi 
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_id, p.product_name;
```

#### 16. List all sellers and their origin, ordered by origin alphabetically.
```sql
SELECT * 
FROM seller 
ORDER BY origin;
```

#### 17. Show the earliest and latest order dates in the dataset.
```sql
SELECT MIN(order_date) AS earliest_order, MAX(order_date) AS last_order_date
FROM orders;
```

#### 18. Find all orders that were shipped by 'FedEx'.
```sql
SELECT o.*
FROM orders o
JOIN shipping s ON o.order_id = s.order_id
WHERE shipping_provider = 'FedEx';
```

#### 19. Count the number of products in each category.
```sql
SELECT c.category_name, COUNT(*) AS total_products
FROM products p 
JOIN category c ON p.category_id = c.category_id
GROUP BY c.category_name;
```

#### 20. Show the total revenue generated from each order (from order_items).
```sql
SELECT order_id, SUM(total_price) AS total_revenue
FROM order_items
GROUP BY order_id;
```

#### 21. List all orders that have a payment status of 'Failed'.
```sql
SELECT * 
FROM payments
WHERE payment_status = 'Failed';
```

#### 22. Find the customer who has placed the most orders.
```sql
SELECT CONCAT(c.first_name, ' ', c.last_name) AS customer_name, 
       COUNT(o.order_id) AS total_orders
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY CONCAT(c.first_name, ' ', c.last_name)
ORDER BY total_orders DESC
LIMIT 1;
```

#### 23. Show all products that are out of stock (stock = 0).
```sql
SELECT p.product_id, p.product_name
FROM inventory i
JOIN products p ON i.product_id = p.product_id
WHERE i.stock = 0;
```

#### 24. List the names of all shipping providers used.
```sql
SELECT DISTINCT shipping_provider 
FROM shipping;
```

#### 25. Find the average quantity of items ordered per order.
```sql
SELECT ROUND(AVG(quantity), 0) AS avg_quantity
FROM order_items;
```

#### 26. Show all orders that are still 'Processing'.
```sql
SELECT * 
FROM orders 
WHERE order_status = 'Processing';
```

#### 27. Count how many orders were delivered to each state.
```sql
SELECT c.state, COUNT(*) AS total_orders
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.state
ORDER BY total_orders DESC;
```

#### 28. Find the most common payment mode.
```sql
SELECT payment_mode, COUNT(*) AS total_payments
FROM payments
GROUP BY payment_mode
ORDER BY total_payments DESC
LIMIT 1;
```

#### 29. List all products with their seller's name.
```sql
SELECT p.product_id, p.product_name, s.seller_name
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o ON oi.order_id = o.order_id
JOIN seller s ON s.seller_id = o.seller_id;
```

#### 30. Show the total value of inventory in each warehouse.
```sql
SELECT i.warehouse_id, 
       COUNT(stock) AS total_stock, 
       SUM(p.price * i.stock) AS total_value
FROM products p
LEFT JOIN inventory i ON i.product_id = p.product_id
GROUP BY i.warehouse_id;
```

#### 31. Calculate the total sales revenue for each month in 2023.
```sql
SELECT MONTH(o.order_date) AS month_, 
       SUM(oi.total_price) AS total_revenue
FROM order_items oi
JOIN orders o ON oi.order_id = o.order_id
WHERE YEAR(o.order_date) = 2023
GROUP BY MONTH(o.order_date)
ORDER BY MONTH(o.order_date);
```

#### 32. Find the top 5 best-selling products by quantity sold.
```sql
SELECT p.product_id, p.product_name, 
       SUM(oi.quantity) AS total_quantity_sold
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_quantity_sold DESC
LIMIT 5;
```

#### 33. Calculate the average order value (AOV) for each customer.
```sql
SELECT c.customer_id, 
       CONCAT(c.first_name, ' ', c.last_name) AS full_name, 
       ROUND(AVG(oi.total_price), 2) AS AOV
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.customer_id, CONCAT(c.first_name, ' ', c.last_name);
```

#### 34. Show the monthly growth rate (%) in revenue for 2024.
```sql
WITH cte AS (
    SELECT 
        MONTH(o.order_date) AS month_,
        SUM(oi.total_price) AS current_revenue
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    WHERE YEAR(o.order_date) = 2024
    GROUP BY MONTH(o.order_date)
    ORDER BY MONTH(o.order_date)
),
prev_revenue AS (
    SELECT 
        month_,
        current_revenue,
        LAG(current_revenue, 1) OVER (ORDER BY month_) AS previous_revenue
    FROM cte
)
SELECT 
    month_,
    current_revenue,
    previous_revenue,
    ROUND(((current_revenue - previous_revenue) / previous_revenue) * 100, 2) AS growth_ratio_percent
FROM prev_revenue;
```

#### 35. Identify customers who have made more than 10 orders.
```sql
SELECT CONCAT(c.first_name, ' ', c.last_name) AS full_name, 
       COUNT(o.order_id) AS total_orders
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY CONCAT(c.first_name, ' ', c.last_name)
HAVING COUNT(o.order_id) > 10;
```

#### 36. Find the category that generates the highest total revenue.
```sql
SELECT c.category_id, c.category_name, 
       SUM(oi.total_price) AS total_revenue 
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN category c ON p.category_id = c.category_id
GROUP BY c.category_id, c.category_name
ORDER BY total_revenue DESC
LIMIT 1;
```

#### 37. Calculate the average time between order date and shipping date for each shipping provider.
```sql
SELECT s.shipping_id, s.shipping_provider, 
       ROUND(AVG(DATEDIFF(s.shipping_date, o.order_date)), 0) AS avg_time
FROM orders o
JOIN shipping s ON o.order_id = s.order_id
GROUP BY s.shipping_id, s.shipping_provider;
```

#### 38. Show the top 3 states that generate the highest revenue.
```sql
SELECT c.state, SUM(oi.total_price) AS total_revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.state
ORDER BY total_revenue DESC
LIMIT 3;
```

#### 39. Identify products that have never been ordered.
```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

#### 40. Find the seller with the highest total sales revenue.
```sql
SELECT s.seller_id, s.seller_name, 
       SUM(oi.total_price) AS total_revenue
FROM orders o 
JOIN order_items oi ON o.order_id = oi.order_id
JOIN seller s ON o.seller_id = s.seller_id
GROUP BY s.seller_id, s.seller_name
ORDER BY total_revenue DESC
LIMIT 1;
```

#### 41. Calculate the percentage of orders that were returned.
```sql
SELECT 
    SUM(CASE WHEN order_status = 'Returned' THEN 1 ELSE 0 END) AS total_returned_orders,
    ROUND(SUM(CASE WHEN order_status = 'Returned' THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS returned_orders_percentage
FROM orders;
```

#### 42. Show the customer retention rate month-over-month.
```sql
WITH monthly_customers AS (
    SELECT 
        MONTH(o.order_date) AS month_,
        YEAR(o.order_date) AS year_,
        COUNT(DISTINCT o.customer_id) AS unique_customers
    FROM orders o
    GROUP BY YEAR(o.order_date), MONTH(o.order_date)
),
previous_customers AS (
    SELECT 
        year_,
        month_,
        unique_customers AS current_customers,
        LAG(unique_customers, 1) OVER (ORDER BY year_, month_) AS previous_customers
    FROM monthly_customers
)
SELECT 
    year_,
    month_,
    current_customers,
    previous_customers,
    ROUND((current_customers / previous_customers) * 100, 2) AS retention_rate
FROM previous_customers
ORDER BY year_, month_;
```

#### 43. Find the day of the week with the highest number of orders.
```sql
WITH daywise_orders AS (
    SELECT 
        YEAR(o.order_date) AS year_,
        DAYNAME(o.order_date) AS day_name,
        COUNT(o.order_id) AS total_orders
    FROM orders o
    GROUP BY YEAR(o.order_date), DAYNAME(o.order_date)
),
ranked_orders AS (
    SELECT 
        year_,
        day_name,
        total_orders,
        RANK() OVER (PARTITION BY year_ ORDER BY total_orders DESC) AS rnk
    FROM daywise_orders
)
SELECT year_, day_name, total_orders
FROM ranked_orders
WHERE rnk = 1;
```

#### 44. Identify orders where the payment was made after the order was shipped.
```sql
WITH order_payment AS (
    SELECT 
        o.order_id,
        o.order_date,
        s.shipping_date,
        p.payment_date,
        p.payment_mode,
        p.payment_status
    FROM orders o
    JOIN shipping s ON o.order_id = s.order_id
    JOIN payments p ON o.order_id = p.order_id
)
SELECT 
    order_id,
    order_date,
    shipping_date,
    payment_date,
    payment_mode,
    payment_status
FROM order_payment
WHERE payment_date > shipping_date;
```

#### 45. Calculate the profit margin for each product and categorize it.
```sql
WITH margin AS (
    SELECT 
        p.product_id,
        p.product_name,
        ROUND(((oi.price_per_unit - p.cogs) / oi.price_per_unit) * 100, 2) AS profit_margin
    FROM products p
    JOIN order_items oi ON p.product_id = oi.product_id
)
SELECT 
    product_id,
    product_name,
    profit_margin,
    CASE 
        WHEN profit_margin > 50 THEN 'High'
        WHEN profit_margin BETWEEN 20 AND 50 THEN 'Medium'
        ELSE 'Low'
    END AS profit_margin_category
FROM margin;
```

#### 46. Show the running total of revenue for each month in 2023.
```sql
SELECT YEAR(o.order_date) AS year_,
       MONTH(o.order_date) AS month_,
       SUM(oi.total_price) AS monthly_revenue,
       SUM(SUM(oi.total_price)) OVER (ORDER BY MONTH(o.order_date)) AS running_total
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE YEAR(o.order_date) = 2023
GROUP BY YEAR(o.order_date), MONTH(o.order_date)
ORDER BY YEAR(o.order_date), MONTH(o.order_date);
```


#### 47. Identify the most valuable customer (by total spend) for each state.
```sql
WITH customer_revenue AS (
    SELECT 
        c.customer_id,
        CONCAT(c.first_name, ' ', c.last_name) AS full_name,
        c.state,
        SUM(oi.total_price) AS total_revenue
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN customers c ON o.customer_id = c.customer_id
    GROUP BY c.customer_id, CONCAT(c.first_name, ' ', c.last_name), c.state
),
ranked AS (
    SELECT 
        customer_id,
        full_name,
        state,
        total_revenue,
        DENSE_RANK() OVER (PARTITION BY state ORDER BY total_revenue DESC) AS rnk
    FROM customer_revenue
)
SELECT 
    customer_id,
    full_name,
    state,
    total_revenue
FROM ranked
WHERE rnk = 1;
```
#### 48. Calculate the average shipping time for delivered orders.
```sql
SELECT 
    ROUND(AVG(DATEDIFF(s.shipping_date, o.order_date)), 2) AS avg_shipping_time
FROM orders o
JOIN shipping s ON o.order_id = s.order_id
WHERE s.delivery_status = 'Delivered';
```

#### 49. Show the top 5 customers with the highest average order value.
```sql
SELECT c.customer_id,
       CONCAT(c.first_name, ' ', c.last_name) AS full_name,
       ROUND(AVG(oi.total_price), 2) AS avg_revenue
FROM orders o 
JOIN order_items oi ON o.order_id = oi.order_id
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.customer_id, CONCAT(c.first_name, ' ', c.last_name)
ORDER BY avg_revenue DESC
LIMIT 5;
```

#### 50. Find sellers who sell products in more than 3 different categories.
```sql
SELECT 
    s.seller_id,
    s.seller_name,
    COUNT(DISTINCT c.category_id) AS total_categories
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN seller s ON o.seller_id = s.seller_id
JOIN products p ON oi.product_id = p.product_id
JOIN category c ON p.category_id = c.category_id
GROUP BY s.seller_id, s.seller_name
HAVING COUNT(DISTINCT c.category_id) > 3;
```

#### 51. Calculate the refund rate by payment mode.
```sql
SELECT 
    p.payment_mode,
    ROUND(
        (SUM(CASE WHEN p.payment_status = 'Failed' THEN 1 ELSE 0 END) 
        + SUM(CASE WHEN o.order_status = 'Returned' THEN 1 ELSE 0 END)) 
        * 100.0 / COUNT(*), 2
    ) AS refund_rate_percentage
FROM payments p
JOIN orders o ON p.order_id = o.order_id
GROUP BY p.payment_mode;
```

#### 52. Identify the peak sales hour of the day.
```sql
SELECT 
    HOUR(order_time) AS hour_,
    COUNT(*) AS total_orders
FROM orders
GROUP BY HOUR(order_time)
ORDER BY total_orders DESC
LIMIT 1;
```

#### 53. Show the revenue generated by each category for each quarter of 2023.
```sql
SELECT c.category_name,
       QUARTER(o.order_date) AS quarter_,
       SUM(oi.total_price) AS total_revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN category c ON p.category_id = c.category_id
WHERE YEAR(o.order_date) = 2023
GROUP BY c.category_name, QUARTER(o.order_date)
ORDER BY c.category_name, QUARTER(o.order_date);
```

#### 54. Find customers who haven't placed an order in the last 6 months.
```sql
WITH max_date AS (
    SELECT MAX(order_date) AS latest_order_date
    FROM orders
),
customer_last_order AS (
    SELECT 
        o.customer_id,
        MAX(o.order_date) AS last_order_date
    FROM orders o
    GROUP BY o.customer_id
)
SELECT 
    c.customer_id,
    CONCAT(c.first_name, ' ', c.last_name) AS full_name,
    clo.last_order_date
FROM customers c
JOIN customer_last_order clo ON c.customer_id = clo.customer_id
JOIN max_date m ON 1=1
WHERE clo.last_order_date < DATE_SUB(m.latest_order_date, INTERVAL 6 MONTH);
```

#### 55. Calculate the stock turnover ratio for each product.
```sql
WITH sales AS (
    SELECT 
        p.product_id,
        p.product_name,
        SUM(oi.quantity) AS total_units_sold
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY p.product_id, p.product_name
),
inventory_avg AS (
    SELECT 
        i.product_id,
        AVG(i.stock) AS avg_inventory
    FROM inventory i
    GROUP BY i.product_id
)
SELECT 
    s.product_id,
    s.product_name,
    s.total_units_sold,
    ia.avg_inventory,
    ROUND(s.total_units_sold / NULLIF(ia.avg_inventory, 0), 2) AS stock_turnover_ratio
FROM sales s
JOIN inventory_avg ia ON s.product_id = ia.product_id;
```

#### 56. Show the YoY growth in customer acquisition.
```sql
WITH yearly_customers AS (
    SELECT 
        YEAR(order_date) AS year_,
        COUNT(DISTINCT customer_id) AS new_customers
    FROM orders
    GROUP BY year_
),
growth AS (
    SELECT 
        year_,
        new_customers,
        LAG(new_customers, 1) OVER (ORDER BY year_) AS prev_year_customers
    FROM yearly_customers
)
SELECT 
    year_,
    new_customers,
    prev_year_customers,
    ROUND(((new_customers - prev_year_customers) / prev_year_customers) * 100, 2) AS yoy_growth_percentage
FROM growth
ORDER BY year_;
```

#### 57. Identify orders where the item quantity is greater than 3.
```sql
SELECT o.*, oi.quantity
FROM orders o 
JOIN order_items oi ON o.order_id = oi.order_id
WHERE oi.quantity > 3;
```

#### 58. Find the most common product category purchased by customers in each state.
```sql
WITH category_count AS (
    SELECT 
        c.state,
        ca.category_name,
        COUNT(*) AS total_orders
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    JOIN customers c ON o.customer_id = c.customer_id
    JOIN category ca ON p.category_id = ca.category_id
    GROUP BY c.state, ca.category_name
),
ranked AS (
    SELECT 
        state,
        category_name,
        total_orders,
        DENSE_RANK() OVER (PARTITION BY state ORDER BY total_orders DESC) AS rnk
    FROM category_count
)
SELECT state, category_name, total_orders
FROM ranked
WHERE rnk = 1;
```

#### 59. Rank sellers within each category based on their total revenue.
```sql
WITH category_wise_revenue AS (
    SELECT s.seller_name, c.category_name, 
           SUM(oi.total_price) AS total_revenue
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN seller s ON o.seller_id = s.seller_id
    JOIN products p ON p.product_id = oi.product_id
    JOIN category c ON p.category_id = c.category_id
    GROUP BY s.seller_name, c.category_name
),
ranked AS (
    SELECT seller_name, category_name, total_revenue,
           DENSE_RANK() OVER (PARTITION BY category_name ORDER BY total_revenue DESC) AS rnk
    FROM category_wise_revenue
)
SELECT seller_name, category_name, total_revenue
FROM ranked
WHERE rnk = 1
ORDER BY category_name;
```




#### 60. Revenue by Category
- Find the revenue generated by each category and calculate its percentage contribution to total revenue.
```sql
WITH cte AS (
    SELECT 
        c.category_id,
        c.category_name,
        SUM(ot.total_price) AS total_category_revenue
    FROM order_items ot
    JOIN products p ON ot.product_id = p.product_id
    JOIN category c ON p.category_id = c.category_id
    GROUP BY c.category_id, c.category_name
)
SELECT 
    category_id,
    category_name,
    total_category_revenue,
    ROUND(
        total_category_revenue / (SELECT SUM(total_price) FROM order_items) * 100, 
        2
    ) AS contribute_percent
FROM cte;

```

#### 61. Average Order Value (AOV) per Customer
- Calculate the average order value for each customer who has placed more than 10 orders.
------------------------------
- Method 1: Based on Total Value / Total Orders
```sql
WITH cte AS (
    SELECT 
        c.customer_id,
        c.first_name,
        c.last_name,
        SUM(ot.total_price) AS total_value,
        COUNT(o.order_id) AS total_orders
    FROM orders o
    JOIN customers c ON o.customer_id = c.customer_id
    JOIN order_items ot ON o.order_id = ot.order_id
    GROUP BY c.customer_id, c.first_name
)
SELECT 
    customer_id,
    first_name,
    last_name,
    ROUND((total_value / total_orders), 2) AS AOV,
    total_orders
FROM cte 
WHERE total_orders >= 10;

```
- Method 2: Based on Average Order Items
```sql
WITH cte AS (
    SELECT 
        c.customer_id,
        c.first_name,
        c.last_name,
        ROUND(AVG(ot.total_price), 2) AS AOV,
        COUNT(o.order_id) AS total_orders
    FROM orders o
    JOIN customers c ON o.customer_id = c.customer_id
    JOIN order_items ot ON o.order_id = ot.order_id
    GROUP BY c.customer_id, c.first_name
)
SELECT 
    customer_id,
    first_name,
    last_name,
    AOV,
    total_orders
FROM cte 
WHERE total_orders >= 10;

```

#### 62. Monthly Sales Trend
- Show monthly total sales and calculate growth compared to the previous month.
```sql
WITH monthly_sales AS (
    SELECT 
        YEAR(o.order_date) AS year_,
        MONTH(o.order_date) AS month_,
        SUM(ot.quantity) AS total_units,
        SUM(ot.total_price) AS current_sale
    FROM orders o
    JOIN order_items ot ON o.order_id = ot.order_id
    GROUP BY YEAR(o.order_date), MONTH(o.order_date)
),
cte AS (
    SELECT 
        year_,
        month_,
        total_units,
        current_sale,
        LAG(current_sale, 1) OVER (ORDER BY year_, month_) AS previous_sale
    FROM monthly_sales
)
SELECT 
    year_,
    month_,
    current_sale,
    previous_sale,
    ROUND(
        ((current_sale - previous_sale) / NULLIF(previous_sale, 0)) * 100, 
        2
    ) AS monthly_growth
FROM cte
ORDER BY year_, month_;

```

#### 63. Customers Without Orders
- List customers who registered but never placed an order.
```sql
SELECT 
    c.customer_id,
    c.first_name,
    c.last_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;

```

#### 64.Best-Selling Categories by State
- Identify the best-selling product category in each state.
```sql
WITH state_category_sales AS (
    SELECT 
        c.state,
        ca.category_name,
        SUM(ot.quantity) AS total_units
    FROM orders o
    JOIN order_items ot ON o.order_id = ot.order_id
    JOIN customers c ON o.customer_id = c.customer_id
    JOIN products p ON ot.product_id = p.product_id
    JOIN category ca ON p.category_id = ca.category_id
    GROUP BY c.state, ca.category_name
),
ranked AS (
    SELECT 
        state,
        category_name,
        total_units,
        DENSE_RANK() OVER (PARTITION BY state ORDER BY total_units DESC) AS rnk
    FROM state_category_sales
)
SELECT 
    state,
    category_name AS best_selling_category,
    total_units
FROM ranked
WHERE rnk = 1
ORDER BY state;

```
#### 65. Customer Lifetime Value
- Calculate total units and revenue generated by each customer across their lifetime.
```sql
SELECT 
    c.customer_id,
    CONCAT(c.first_name,' ',c.last_name) AS full_name,
    SUM(ot.quantity) AS total_units,
    SUM(ot.total_price) AS total_price
FROM orders o 
JOIN order_items ot ON o.order_id = ot.order_id
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.customer_id, full_name
ORDER BY c.customer_id;

```

#### 66. Inventory Stock Alerts
- List products with stock level below 300 units.
```sql
SELECT 
    p.product_id,
    p.product_name,
    i.stock,
    i.warehouse_id,
    i.last_stock_date
FROM inventory i
JOIN products p ON i.product_id = p.product_id
WHERE i.stock < 300;

```

#### 67. Shipping Delays
- Identify orders where shipping took more than 7 days after order date.
```sql
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS full_name,
    o.customer_id,
    o.order_date,
    s.shipping_date,
    s.shipping_provider
FROM orders o
JOIN shipping s ON o.order_id = s.order_id
JOIN customers c ON o.customer_id = c.customer_id
WHERE DATEDIFF(s.shipping_date, o.order_date) > 7;

```

#### 68. Top Performing Sellers
- Find the top 5 sellers based on total sales value.
```sql
SELECT 
    s.seller_id,
    s.seller_name,
    s.origin,
    SUM(oi.total_price) AS total_sales_value
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN seller s ON o.seller_id = s.seller_id
GROUP BY s.seller_id, s.seller_name, s.origin
ORDER BY total_sales_value DESC
LIMIT 5;

```

#### 69.Payment Success Rate
#### Question - 1
- Calculate percentage of successful payments across all orders.
```sql
SELECT 
    p.payment_status,
    COUNT(*) AS total_count,
    ROUND(COUNT(*) / (SELECT COUNT(*) FROM payments) * 100, 2) AS payment_rate
FROM payments p 
JOIN orders o ON p.order_id = o.order_id 
GROUP BY p.payment_status;

```
#### Question - 2
- Success rate by payment mode.
```sql
SELECT 
    p.payment_mode,
    p.payment_status,
    COUNT(*) AS total_payments,
    ROUND(
        COUNT(*) * 100.0 / (SELECT COUNT(*) FROM payments), 
        2
    ) AS payment_rate_percent
FROM payments p
JOIN orders o ON p.order_id = o.order_id
GROUP BY p.payment_mode, p.payment_status
ORDER BY p.payment_mode;

```

#### 70. Seller Orders by Status
- Count seller orders by different statuses (Delivered, Cancelled, etc.).
```sql
SELECT 
    s.seller_id,
    s.seller_name,
    s.origin,
    o.order_status,
    SUM(oi.quantity) AS total_count
FROM orders o 
JOIN order_items oi ON o.order_id = oi.order_id
JOIN seller s ON o.seller_id = s.seller_id
GROUP BY s.seller_id, s.seller_name, s.origin, o.order_status
ORDER BY s.seller_id, total_count DESC;

```

#### 71. Seller Order Status Ratio
- Show the ratio of each order status compared to seller’s total orders.
```sql
WITH seller_status AS (
    SELECT 
        s.seller_id,
        s.seller_name,
        s.origin,
        o.order_status,
        COUNT(o.order_id) AS status_count
    FROM orders o
    JOIN seller s ON o.seller_id = s.seller_id
    GROUP BY s.seller_id, s.seller_name, s.origin, o.order_status
),
seller_totals AS (
    SELECT 
        seller_id,
        SUM(status_count) AS total_orders
    FROM seller_status
    GROUP BY seller_id
)
SELECT 
    ss.seller_id,
    ss.seller_name,
    ss.origin,
    ss.order_status,
    ss.status_count,
    ROUND((ss.status_count / st.total_orders) * 100, 2) AS status_ratio_percent
FROM seller_status ss
JOIN seller_totals st ON ss.seller_id = st.seller_id
ORDER BY ss.seller_id, status_ratio_percent DESC;

```

#### 72. Pivot Orders by Status
- Show seller orders pivoted by status.
```sql
SELECT 
    s.seller_id,
    s.seller_name,
    SUM(CASE WHEN o.order_status = 'Delivered' THEN oi.quantity ELSE 0 END) AS successful_orders,
    SUM(CASE WHEN o.order_status = 'Cancelled' THEN oi.quantity ELSE 0 END) AS cancelled_orders,
    SUM(CASE WHEN o.order_status = 'Returned'  THEN oi.quantity ELSE 0 END) AS returned_orders,
    SUM(CASE WHEN o.order_status = 'Shipped' THEN oi.quantity ELSE 0 END) AS shipped_orders,
    SUM(CASE WHEN o.order_status = 'Processing' THEN oi.quantity ELSE 0 END) AS processing_orders,
    SUM(CASE WHEN o.order_status = 'Pending'  THEN oi.quantity ELSE 0 END) AS pending_orders,
    SUM(oi.quantity) AS total_orders
FROM seller s
JOIN orders o ON s.seller_id = o.seller_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY s.seller_id, s.seller_name;

```

#### 73. Product Profit Margin
- Calculate profit margin for each product.
```sql
SELECT 
    p.product_name,
    SUM(oi.quantity * oi.price_per_unit) AS total_revenue,
    SUM(oi.quantity * p.cogs) AS total_cost,
    SUM(oi.quantity * oi.price_per_unit) - SUM(oi.quantity * p.cogs) AS profit_margin
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
GROUP BY p.product_name
ORDER BY profit_margin DESC;

```

#### 74. Most Returned Products
- Find the top 10 most returned products.
```sql
WITH cte AS (
    SELECT 
        p.product_id,
        p.product_name,
        SUM(oi.quantity) AS total_units_sold,
        SUM(CASE WHEN o.order_status = 'Returned' THEN oi.quantity ELSE 0 END) AS returned_units
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY p.product_id, p.product_name
    ORDER BY returned_units DESC
    LIMIT 10
)
SELECT 
    product_id,
    product_name,
    total_units_sold,
    returned_units,
    ROUND((returned_units / total_units_sold) * 100, 2) AS return_percent
FROM cte;

```

#### 75. Inactive Sellers
- Find sellers with no sales in the last 2 years.
```sql
SELECT *
FROM seller s
WHERE NOT EXISTS (
    SELECT 1 
    FROM orders o
    WHERE o.seller_id = s.seller_id
      AND o.order_date >= CURDATE() - INTERVAL 2 YEAR
);

```

#### 76. Returning Customers
- Identify customers who returned more than 5 orders.
```sql
WITH cte AS (
    SELECT 
        c.customer_id,
        CONCAT(c.first_name, ' ', c.last_name) AS full_name,
        COUNT(o.order_id) AS total_orders,
        SUM(CASE WHEN o.order_status = 'Returned' THEN 1 ELSE 0 END) AS returned_orders
    FROM orders o
    JOIN customers c ON o.customer_id = c.customer_id
    GROUP BY c.customer_id, full_name
    ORDER BY returned_orders DESC
)
SELECT 
    customer_id,
    full_name,
    total_orders,
    returned_orders,
    CASE 
        WHEN returned_orders > 5 THEN 'Returning_customer' 
        ELSE 'Normal_Customer' 
    END AS customer_category
FROM cte;

```

#### 77. Top 5 Customers by Sales (per State)
```sql
WITH cte AS (
    SELECT 
        c.customer_id,
        CONCAT(c.first_name,' ',c.last_name) AS full_name,
        c.state,
        COUNT(o.order_id) AS total_orders,
        SUM(oi.total_price) AS total_price
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN customers c ON o.customer_id = c.customer_id
    GROUP BY c.customer_id, full_name, c.state
),
ranked AS (
    SELECT 
        customer_id,
        full_name,
        state,
        total_orders,
        total_price,
        DENSE_RANK() OVER(PARTITION BY state ORDER BY total_price DESC) AS rnk
    FROM cte
)
SELECT 
    customer_id,
    full_name,
    state,
    total_orders,
    total_price
FROM ranked 
WHERE rnk <= 5   
ORDER BY state, rnk, total_price DESC;

```

#### 78. Top 5 Customers by Orders (per State)
```sql
WITH cte AS (
    SELECT 
        c.customer_id,
        CONCAT(c.first_name,' ',c.last_name) AS full_name,
        c.state,
        COUNT(o.order_id) AS total_orders
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN customers c ON o.customer_id = c.customer_id
    GROUP BY c.customer_id, full_name, c.state
),
ranked AS (
    SELECT 
        customer_id,
        full_name,
        state,
        total_orders,
        DENSE_RANK() OVER(PARTITION BY state ORDER BY total_orders DESC) AS rnk
    FROM cte
)
SELECT 
    customer_id,
    full_name,
    state,
    total_orders
FROM ranked 
WHERE rnk <= 5   
ORDER BY state, rnk, total_orders DESC;

```

#### 79. Revenue by Shipping Provider
- Calculate total revenue, total orders, and average delivery time per shipping provider.
```sql
SELECT 
    s.shipping_provider,
    COUNT(DISTINCT o.order_id) AS total_orders,
    ROUND(SUM(oi.total_price), 2) AS total_revenue,
    ROUND(AVG(DATEDIFF(s.shipping_date, o.order_date)), 0) AS avg_delivery_time_days
FROM orders o 
JOIN shipping s ON o.order_id = s.order_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY s.shipping_provider
ORDER BY total_revenue DESC;

```

#### 80. Top 10 Products by Growth (2023 → 2024)
```sql
WITH cte AS (
    SELECT
        p.product_id,
        p.product_name,
        c.category_name,
        YEAR(o.order_date) AS year,
        SUM(oi.total_price) AS total_revenue
    FROM orders o 
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    JOIN category c ON p.category_id = c.category_id
    WHERE YEAR(o.order_date) IN (2023, 2024)
    GROUP BY p.product_id, p.product_name, c.category_name, YEAR(o.order_date)
),
year_wise_revenue AS (
    SELECT 
        product_id,
        product_name,
        category_name,
        SUM(CASE WHEN year = 2023 THEN total_revenue ELSE 0 END) AS 2023_revenue,
        SUM(CASE WHEN year = 2024 THEN total_revenue ELSE 0 END) AS 2024_revenue
    FROM cte
    GROUP BY product_id, product_name, category_name
),
growth_ratio AS (
    SELECT 
        product_id,
        product_name,
        category_name,
        2023_revenue,
        2024_revenue,
        (2024_revenue - 2023_revenue) AS revenue_diff,
        ROUND(((2024_revenue - 2023_revenue) / 2023_revenue), 2) AS growth_pattern
    FROM year_wise_revenue
),
ranked_ratio AS (
    SELECT 
        product_id,
        product_name,
        category_name,
        2023_revenue,
        2024_revenue,
        revenue_diff,
        growth_pattern,
        DENSE_RANK() OVER(ORDER BY growth_pattern DESC) AS rnk
    FROM growth_ratio
)
SELECT 
    product_id,
    product_name,
    category_name,
    2023_revenue,
    2024_revenue,
    revenue_diff,
    CONCAT(growth_pattern, '%') AS growth_percent
FROM ranked_ratio
WHERE rnk <= 10;

```

#### 81. Stored Procedure: Update Inventory after Sale
- When a product is sold, update inventory automatically by reducing sold quantity.
```sql
DELIMITER $$

CREATE PROCEDURE update_inventory_stock_data(
    IN p_customer_id INT,
    IN p_product_id INT,
    IN p_order_item_id INT,
    IN p_order_id INT,
    IN p_quantity INT,
    IN p_seller_id INT
)
BEGIN
    DECLARE v_count INT;
    DECLARE v_price DECIMAL(10,2);
    DECLARE v_product_name VARCHAR(50);
    
    SELECT price, product_name 
    INTO v_price, v_product_name
    FROM products
    WHERE product_id = p_product_id;

    SELECT COUNT(*) 
    INTO v_count
    FROM inventory 
    WHERE product_id = p_product_id AND stock >= p_quantity;

    IF v_count > 0 THEN
        INSERT INTO orders(order_id, order_date, customer_id, seller_id)
        VALUES (p_order_id, CURDATE(), p_customer_id, p_seller_id);

        INSERT INTO order_items(order_item_id, order_id, product_id, quantity, price_per_unit, total_price)
        VALUES (p_order_item_id, p_order_id, p_product_id, p_quantity, v_price, p_quantity * v_price);

        UPDATE inventory
        SET stock = stock - p_quantity
        WHERE product_id = p_product_id;

        SELECT CONCAT('Product sale added. Inventory updated for: ', v_product_name) AS message;

    ELSE
     
        SELECT CONCAT('Product ', v_product_name, ' is not available in required quantity.') AS message;
    END IF;
END$$

DELIMITER ;

```





