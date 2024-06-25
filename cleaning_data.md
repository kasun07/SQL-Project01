What issues will you address by cleaning the data?

Queries:

Below, provide the SQL queries you used to clean your data.


1) all_sessions table - Total rows: 15134

	I. 	Lots of values are null. If we remove NULL values from total_transaction_revenue & transaction, number of rows getting very low. 
	
	II. Few countries and cities are not available but there is a record. There are values called 
		'(not set)' and '(not available in demo dataset)' how can we remove those? how do we know how many are there like this?

```SQL
SELECT *
FROM all_sessions
WHERE total_transaction_revenue IS NOT NULL 
	AND transactions IS NOT NULL
	AND city <> 'not available in demo dataset'
	AND city <> '(not set)' 
	AND country <> '(not set)' 
```
	
	III.In the 'time' column there are some values zero. Time couldn't be zero. There are 5098 rows, which is time is zero. 

```SQL
SELECT *
FROM all_sessions
WHERE time <> 0
```
	
	IV. There is a column 'time_on_site', I have no clear idea what does this mean. Time can't be zero. 
	
 	V. 	There are lots of column that we don't use much (including null values), so when we do some analysis we can ignore those columns. 	
  		Example, session_quality_dim, type, product_variant, item_quantity etc...

3) analytics table - Total rows: 4301122

	I. There are duplicate rows in the table

```SQL
SELECT visit_number,visit_id,visit_start_time,date,full_visitor_id,user_id,channel_grouping,
	social_engagement_type,units_sold,page_views,time_on_site,bounces,revenue,unit_price,
	COUNT(*) AS count
FROM analytics
GROUP BY visit_number,visit_id,visit_start_time,date,full_visitor_id,user_id,channel_grouping,
	social_engagement_type,units_sold,page_views,time_on_site,bounces,revenue,unit_price
HAVING COUNT(*) > 1
```
