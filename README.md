### Analyse des Ventes de Pizzas sur SQL

Ce projet consiste en l'exploration des données de ventes de pizzas. Vous trouverez ci-dessous le code SQL utilisé pour analyser les données, effectuer des requêtes et explorer les différentes tendances dans les ventes.

Le code suivant permet de récupérer les informations nécessaires pour comprendre les performances des ventes et effectuer des analyses statistiques pertinentes.

```sql
select * from pizza_sales;

--  total number of records and basic data structure
SELECT COUNT(*) AS total_records 
FROM pizza_sales;

-- unique values in each column
SELECT COUNT(DISTINCT id) AS unique_orders,
       COUNT(DISTINCT name) AS unique_pizzas,
       COUNT(DISTINCT size) AS unique_sizes,
       COUNT(DISTINCT type) AS unique_types
FROM pizza_sales;

-- Sales Analysis

-- Total Sales
SELECT SUM(price) AS total_sales 
FROM pizza_sales;
-- *Toatl sales : $817 860*


-- Sales by pizza type 
SELECT  type, 
		SUM(price) AS total_sales, 
		COUNT(*) AS total_orders
FROM pizza_sales
GROUP BY type
ORDER BY total_sales DESC;
-- *The most ordered pizzas are the classic with 14 888 pizzas ordered which generated  about 220k. Followed by the supreme, the chicken then the veggie*

-- Sales by size 
SELECT  size, 
		SUM(price) AS total_sales, 
    	COUNT(*) AS total_orders
FROM pizza_sales
GROUP BY size
ORDER BY total_sales DESC;
-- *Large pizzas are the most sold (18 959 pizzas sold) followed by medium and small.*  

-- Top performers 
SELECT name, SUM(price) AS total_revenue, COUNT(*) AS order_count
FROM pizza_sales
GROUP BY name
ORDER BY total_revenue DESC
LIMIT 10;
-- *Thai chicken pizzas were the most sold.*

--Average Price by Type and Size
SELECT type, size, AVG(price) AS avg_price
FROM pizza_sales
GROUP BY type, size
ORDER BY type, avg_price DESC;

-- Average Number of Pizzas per Order
SELECT AVG(pizza_count) AS avg_pizzas_per_order
FROM (SELECT id, COUNT(*) AS pizza_count FROM pizza_sales GROUP BY id) AS subquery;

--Average Price by Pizza Type
SELECT type, AVG(price) AS avg_price
FROM pizza_sales
GROUP BY type
ORDER BY avg_price DESC;


-- Popular Pizza Combinations
SELECT id, 
	GROUP_CONCAT(name ORDER BY name) AS pizza_combination, 
    COUNT(*) AS combination_count
FROM pizza_sales
GROUP BY id
HAVING COUNT(*) > 1
ORDER BY combination_count DESC
LIMIT 10;

-- Weekend vs. Weekday Sales
SELECT 
    CASE WHEN DAYOFWEEK(date) IN (1, 7) THEN 'Weekend' ELSE 'Weekday' END AS day_type,
    COUNT(*) AS total_orders, SUM(price) AS total_sales
FROM pizza_sales
GROUP BY day_type;
```
