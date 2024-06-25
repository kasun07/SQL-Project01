Question 1: What are the top 5 products which has been ordered most? Provide product id, name and the number of ordered

SQL Queries:
```SQL
	SELECT sbs.product_sku, p.name, sbs.total_ordered
	FROM sales_by_sku sbs
	JOIN products p ON sbs.product_sku = p.sku_id
	ORDER BY total_ordered DESC
	LIMIT 5
```

Answer: 

product_sku,name,total_ordered

GGOEGOAQ012899,Ballpoint LED Light Pen,456 

GGOEGDHC074099,17oz Stainless Steel Sport Bottle,334

GGOEGOCB017499,Leatherette Journal,319

GGOEGOCC077999,Spiral Journal with Pen,290

GGOEGFYQ016599,Foam Can and Bottle Cooler,253



Question 2: What are the products have been ordered without having a single order?

SQL Queries:
```SQL
	SELECT *
	FROM (
		SELECT p.sku_id, p.name, p.ordered_quantity, sbs.total_ordered
		FROM products p
		LEFT JOIN sales_by_sku sbs ON p.sku_id = sbs.product_sku
		WHERE sbs.total_ordered IS NULL OR sbs.total_ordered = 0 
		) AS temp
	WHERE ordered_quantity > 0
```
Answer:

sku_id,name,ordered_quantity,total_ordered

GGOEGAAX0365,Women's Scoop Neck Tee Black,65,0

GGOEGEVB070899,Bongo Cupholder Bluetooth Speaker,85,NULL

GGOEGESC014699,Aluminum Handy Emergency Flashlight,66,NULL

GGOEGAAX0296,Women's Short Sleeve Tri-blend Badge Tee Grey,19,0

GGOEGAWH061350,Onesie Green,7,NULL

Total rows: 597

Question 3: Rank the city and country which has the highest average time on the site? 

SQL Queries:
```SQL
	WITH avgTimeOnSite AS (
		SELECT city, country, 
			ROUND(AVG(time_on_site) OVER (PARTITION BY city, country), 2) AS avg_time_on_site
		FROM all_sessions
		WHERE city <> 'not available in demo dataset'
			AND city <> '(not set)' 
			AND country <> '(not set)' 
			AND time_on_site IS NOT NULL
		ORDER BY avg_time_on_site DESC
	)
	SELECT *, 
		RANK() OVER (ORDER BY avg_time_on_site DESC)
	FROM avgTimeOnSite
	ORDER BY RANK
```

Answer:
city,country,avg_time_on_site,rank

Sakai,Japan,2382.00,1

Cork,Ireland,2366.00,2

Villeneuve-d'Ascq,France,2280.00,3

Singapore,France,2061.00,4

East Lansing,United States,1733.00,5

Rexburg,United States,1714.00,6

San Mateo,United States,1630.50,7

San Mateo,United States,1630.50,7

Sapporo,Japan,1556.00,9

Oxford,United Kingdom,1390.00,10

Total rows: 5423

Question 4: Find the percentage of visits that made a product reveune out of the visits spent time on the site. 

SQL Queries:
```SQL
	SELECT COUNT(*) AS total_records,
		COUNT(product_revenue),
		ROUND((COUNT(product_revenue) * 100.0/ COUNT(*)), 4) AS percentage_product_revenue
	FROM all_sessions
	WHERE time_on_site IS NOT NULL
```
Answer:
total_records,count_product_revenue,percentage_product_revenue

11834,4,0.0338


Question 5: 

SQL Queries:

Answer:
