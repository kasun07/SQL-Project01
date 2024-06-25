What are your risk areas? Identify and describe them.

QA Process:
Describe your QA process and include the SQL queries used to execute it.

1) In the all_sessions table, values for city and country contain (not set) and not available in demo dataset. If it's a NULL value we can query and find those. 
Out of 15134 records I found above values and I can ignore when I do queries. But how do I know what kind of inappropiate values will be there that 
should be removed from queries. 
	```SQL
		SELECT *
		FROM all_sessions
		WHERE AND transactions IS NOT NULL
			AND city <> 'not available in demo dataset'
			AND city <> '(not set)' 
			AND country <> '(not set)' 
	```
2) In all_sessions table, consider following sql query... (this applies to the analytics table as well)
	```SQL
	SELECT date,
		CASE
 		  WHEN date ~ '^\d{4}-\d{2}-\d{2}$' THEN 'Correct Format'
			  ELSE 'Incorrect Format'
		END AS format_status
	FROM all_sessions;
	```
When I run this query it gives following error, 

      ERROR:  operator does not exist: date ~ unknown
  
      LINE 3:            WHEN date ~ '^\d{4}-\d{2}-\d{2}$' THEN 'Correct F...
 
      HINT:  No operator matches the given name and argument types. You might need to add explicit type casts. 
  
      SQL state: 42883
  
      Character: 47
	
Reason - The error is happening because I have used the ~ operator, which checks for patterns in text, but I tried to use it on a date column. 
Dates are not text, so it wasn't worked. Have to convert to a text.

Correct SQL query;
```SQL
	SELECT date,
		CASE
			WHEN date::text ~ '^\d{4}-\d{2}-\d{2}$' THEN 'Correct Format'
			ELSE 'Incorrect Format'
		END AS format_status
	FROM all_sessions
```

3) all_seesions table - There is not primary key (column with unique values). Let's search whether values of following columns;

*full_visitor_id and visit_id 

```SQL
		SELECT full_visitor_id, visit_id, COUNT(*)
		FROM analytics
		GROUP BY full_visitor_id, visit_id
		HAVING COUNT(*) > 1
```

*product_sku

```SQL
		SELECT full_visitor_id, visit_id, COUNT(*)
		FROM analytics
		GROUP BY full_visitor_id, visit_id
		HAVING COUNT(*) > 1
```

4) all_seesions table - this is the only table that talks about country and city. I assumed country and city mean visitor's country and city. 
So every visitor should have a country and city.

Total records of this table are 15134, out of that 8656 records have invalid values for country or city either both.

```SQL
		SELECT *
		FROM all_sessions
		WHERE city = 'not available in demo dataset' 
			OR city = '(not set)' 
			OR country = '(not set)' 
```

So need to remove those records using from following query;

```SQL
		SELECT *
		FROM all_sessions
		WHERE city <> 'not available in demo dataset' 
			AND city <> '(not set)' 
			AND country <> '(not set)'
```

5) all_sessions table - most of the values in transactions and total_transaction_revenue are null and also same as product_quantity. Can't come up with a 
conclusion without having the calculation of the formula of the revenue.

```SQL
		SELECT *
		FROM all_sessions
		WHERE city <> 'not available in demo dataset' 
			AND city <> '(not set)' 
			AND country <> '(not set)' 
			AND total_transaction_revenue IS NOT NULL
			AND transactions IS NOT NULL
			AND product_quantity IS NOT NULL 
```

7) analytics table - what is the format and meaning of the visit_start_time? Out of 4301122 records, in 4262512 records visit_id and start_visit_time are equal.

How could be an ID and time have equal values?

```SQL
		SELECT visit_id, visit_start_time
		FROM analytics
		WHERE visit_id = visit_start_time
```

9) analytics table - there are duplicate rows. 

```SQL
		SELECT visit_number,visit_id,visit_start_time,date,full_visitor_id,user_id,channel_grouping,
			social_engagement_type,units_sold,page_views,time_on_site,bounces,revenue,unit_price,
			COUNT(*) AS count
		FROM analytics
		GROUP BY visit_number,visit_id,visit_start_time,date,full_visitor_id,user_id,channel_grouping,
			social_engagement_type,units_sold,page_views,time_on_site,bounces,revenue,unit_price
		HAVING COUNT(*) > 1
```

10) There are no matching results when joining the all_sessions and analytics tables using the full_visitor_id columns because there are no common values in both tables.
	
 Additionally, what is the purpose of having two identical columns named full_visitor_id if they are not related?

```SQL
		SELECT * FROM all_sessions alls
		JOIN analytics a USING (full_visitor_id)
```

12) I assume all the details of products has been recorded in products table. But there are 8 products not recorded in products table but sales_by_sku table.
    
```SQL
		SELECT product_sku, total_ordered
		FROM sales_by_sku sbs
		WHERE NOT EXISTS (SELECT 1 FROM products p WHERE p.sku_id  = sbs.product_sku)
```
	
14) When we compare products, sales_by_sku, sales_report tables
    
	  sales_by_sku table contains total ordered based on the each product

	  There's no point of having two tables as products and sales_report, we can create one table with one table with following columns, 
		product_sku, ordered_quantity, name, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude, ratio
		(Note - no idea about ration column and how it's calculated. Hope there's no impact with total_ordered values to calculate ration)

	  For the new table no need to have total_ordered column because we can have it from sales_by_sku table. 
