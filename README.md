
## SQL Project - Amazon Sales Analysis
![amazon_logo](https://www.amalytix.com/blog/amazon_ai_features_cover.jpg)

- This prject is based on Amazon sales which are happend in the 2021,2022,2023,2024 
- Derived valuable insights from the data by using various SQL concepts
- Also this project covers all the SQL concepts from basics to advanced stuff

#### Concepts Covered Through This Project:-
- Creation of the DATABASE and TABLEs 
- Generate data using the Python Faker Library for sample data
- JOINS with multiple tables
- Grouping and Aggregation (GROUP BY,HAVING,WHERE)
- DATE Functions (YEAR,MONTH,QUARTER,DATEDIFF,DAYNAME,YEARWEEK)
- Window Functions (LEAD,LAG,RANK,DENSE_RANK,ROW_NUMBER)
- CTE,RECURSIVE CTEs
- Store-Procedure

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
#### 49. Calculate the average shipping time for delivered orders.
```sql
SELECT 
    ROUND(AVG(DATEDIFF(s.shipping_date, o.order_date)), 2) AS avg_shipping_time
FROM orders o
JOIN shipping s ON o.order_id = s.order_id
WHERE s.delivery_status = 'Delivered';
```

#### 50. Show the top 5 customers with the highest average order value.
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

#### 51. Find sellers who sell products in more than 3 different categories.
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

#### 52. Calculate the refund rate by payment mode.
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

#### 53. Identify the peak sales hour of the day.
```sql
SELECT 
    HOUR(order_time) AS hour_,
    COUNT(*) AS total_orders
FROM orders
GROUP BY HOUR(order_time)
ORDER BY total_orders DESC
LIMIT 1;
```

#### 54. Show the revenue generated by each category for each quarter of 2023.
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

#### 55. Find customers who haven't placed an order in the last 6 months.
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

#### 56. Calculate the stock turnover ratio for each product.
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

#### 57. Show the YoY growth in customer acquisition.
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

#### 58. Identify orders where the item quantity is greater than 3.
```sql
SELECT o.*, oi.quantity
FROM orders o 
JOIN order_items oi ON o.order_id = oi.order_id
WHERE oi.quantity > 3;
```

#### 59. Find the most common product category purchased by customers in each state.
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

#### 60. Rank sellers within each category based on their total revenue.
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
