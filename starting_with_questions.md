Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```SQL
SELECT city, country, SUM(total_transaction_revenue) AS total_transaction_revenue
FROM all_sessions
WHERE total_transaction_revenue IS NOT NULL 
	AND city <> 'not available in demo dataset'
	AND city <> '(not set)' 
	AND country <> '(not set)'  
GROUP BY city, country
ORDER BY total_transaction_revenue DESC
```
Answer:

city,country,total_transaction_revenue

San Francisco,United States,1564320000

Sunnyvale,United States,992230000

Atlanta,United States,854440000

Palo Alto,United States,608000000

Tel Aviv-Yafo,Israel,602000000

New York,United States,530360000

Mountain View,United States,483360000

Total rows: 20

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```SQL
SELECT country, city, ROUND(AVG(product_quantity), 2) AS avg_ordered_products
FROM all_sessions
WHERE product_quantity IS NOT NULL 
	AND city <> 'not available in demo dataset' 
	AND city <> '(not set)' 
	AND country <> '(not set)' 
GROUP BY city, country
ORDER BY avg_ordered_products DESC
```
Answer:

country,city,avg_ordered_products

Spain,Madrid,10.00

United States,Salem,8.00

United States,Atlanta,4.00

United States,Houston,2.00

United States,New York,1.17

United States,Detroit,1.00

Ireland,Dublin,1.00

Total rows: 19


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL
SELECT country, city, product_quantity, v2_product_category
FROM all_sessions
WHERE product_quantity IS NOT NULL 
	AND city <> 'not available in demo dataset' 
	AND city <> '(not set)' 
	AND country <> '(not set)' 
	AND v2_product_category <> '(not set)' 
ORDER BY country, city
```
Answer:
There's no any pattern in product categories of products ordered from visitors in each city and country. 

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

Assume - I assumed top-selling product means product_quantity. In other hand, we can calculate the top-selling prodjuct from the total cost
of the ordered quantity and unit price(product_quantity * unit price)

SQL Queries:
```SQL
SELECT country, city, 
	MAX(product_quantity) OVER (PARTITION BY country, city) AS product_quantity,
	product_sku, v2_product_name
FROM all_sessions
WHERE product_quantity IS NOT NULL 
	AND city <> 'not available in demo dataset' 
	AND city <> '(not set)' 
	AND country <> '(not set)' 
ORDER BY product_quantity DESC
```
Answer:
country,city,product_quantity,product_sku,v2_product_name

Spain,Madrid,10,GGOEWAEA083899,Waze Dress Socks

United States,Salem,8,GGOEGOCT019199,Red Spiral Google Notebook

United States,Atlanta,4,GGOEGBJR018199,Reusable Shopping Bag

United States,New York,2,GGOEGAFB035814,Google Men's  Zip Hoodie

United States,New York,2,GGOENEBJ079499,Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel

United States,New York,2,GGOEGAAX0104,Google Men's 100% Cotton Short Sleeve Hero Tee White

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
SELECT city, country, SUM(total_transaction_revenue) AS total_transaction_revenue
FROM all_sessions
WHERE total_transaction_revenue IS NOT NULL 
	AND city <> 'not available in demo dataset'
	AND city <> '(not set)' 
	AND country <> '(not set)'  
GROUP BY city, country
ORDER BY total_transaction_revenue DESC
```
Answer:







