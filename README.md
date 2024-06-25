# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(fill in your description and goals here)

Project Description

Evaluate site visitor integration with the product revenue

This project looks at how visitors interact with an e-commerce website using detailed database records. 
The database tracks various aspects of visitor behavior, including...
	Visit Details: Timestamp of visits, pages viewed, and duration of stay.
	Location Information: Geographic location of the visitors.
	Engagement: Number of pages viewed, length of site visits.
	Transaction Data: Revenue generated from transactions and the number of transactions made by visitors.

The aim is to combine and apply SQL skills to extract, clean, transform, and analyze this data to obtain meaningful insights about user interactions and 
revenue disciplines.

Project Goals
1) Data Cleaning and Transformation:
Clean and preprocess the extracted data to ensure accuracy and consistency.Transform the data into a structured format suitable for analysis.
	
	Remove unwanted or irrelavent observations.

	Ensure accurate datatypes - in this project we set up the data type for the columns. 

	Address missing values

	Fixing typos

2) Data Analysis:
Analyze the data to identify user behavior and product revenue. Evaluate the impact of various factors such as visit duration, page views, and 
visitor location on product revenue. And also product details, number of ordered and total quantity that ordered based on the each product. 

3) Quality Assurance (QA):
Develop and execute a comprehensive QA process to validate the accuracy and consistency of the transformed data against the raw data.


## Process
Step 01 - Load the tables to the database in PGADMIN. 

When loading the table, had to create a database and tables. 
Then create columns for each table as per the given date files. Defined the data types for the columns based on the values. 
Uploaded records to the tables. 

Step 02 - Analyzing data. 

There are 5 different tables with huge number of records. Out of five tables, three tables are about the product details, ordered quantitiy 
and total number of orders. And other two tables are related to transactions and revenues. 

	I. all_session table with 32 columns and 15134 rows. No primary key. Mainly focus on the site/ pages visits by visitors, 
		transactions done and revenues and contains visitors' details. Lots of null values.
  
	II. analytics table with 14 columns and 4301122 rows. No primary key. Contains details about login details of visitors. 
 	Lots of duplicate rows. 
 
	III. products table with 7 columsn and 1092 rows.  Primary key - sku_id and contains details of the products and warehouse store. 
 
	IV.  sales_report table with 8 columns and 454 rows.  Primary key - product_sku and similar details as products table. 
 	It says the total ordered. 
 
	V. sales_by_sku table with 2 columsn and 462 rows. Primary key - product_skuand contains product id and number of total ordered. 
 

Step 03 - Data cleaning

The most important part that need to execute before making a conclusion or querying the database.

	> Remove null values
 
	> Remove duplicates
 
	> Check the accuracy of the data types
 
	> Ignore the unuseful records 
 
	> Addressing missing values
 

Step 04 - QA process

Once completed with the data cleaning, we need to verify and validate the accuracy of the data. 

	> Verification - Ensuring that the data is collected accurately and is consistent with defined standards.
 
	> Validation  - Confirming that the data meets the requirements and is suitable for the intended analysis.
 
	> Documentation - Keeping thorough documentation of data sources, transformations, and cleaning steps.
 
	> Testing - Performing tests to check the reliability and consistency of the data.
 
In essence, data cleaning is a part of the QA process. QA ensures the overall quality while data cleaning focuses specifically on rectifying the data.

## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)
1) To see the total transaction revenue of each city and country
2) Most visited and ordered product by visitors
3) The year and month which has most product revenue and transaction revenue


## Challenges  
(discuss challenges you faced in the project)

1) Before dealing with the database we need to have proper knowledge of the database and what does represent in each column? There are some columns it's really hard
to understand. Otherwise we can't realize the value or use of the column and also the format of the data. 
2) Without having proper study/ knowledge about the domain or the business process of the table, we can't identify risks and improvements of the database. 
Same table could be have different disciplines. 
3) There are columns with lots of NULL or inappropiate values as an example, in all_sessions table city and coutry contain '(not set)' and 
'not available in demo dataset'. This is not possible. Any visitor should have a city and country. 
If we remove those records, total rows count decrease to 6478 rows from 15134 rows
4) There are no matching results when joining the all_sessions and analytics tables using the full_visitor_id columns because there are no common values in both tables.
Additionally, what is the purpose of having two identical columns named full_visitor_id if they are not related?

## Future Goals
(what would you do if you had more time?)

1) Study the domain and the business process behide the dataset. I believe it will help to overcome all the issues related to the database. 
2) Remove unuseful columns and rows from the all_sessions and analytics tables. To do that need business process expertise knowledge. 
3) Create three separete ables called product_details, order_details, warehouse_storing_details. 
4) Creating a document and adding description or formulas for each columns. 

