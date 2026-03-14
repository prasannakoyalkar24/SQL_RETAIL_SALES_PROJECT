Retail Sales Analysis SQL Project
Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: sql_project_p1

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

Objectives
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
Data Cleaning: Identify and remove any records with missing or null values.
Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

Project Structure
1. Database Setup
Database Creation: The project starts by creating a database named sql_project_p1.
Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE sql_project_p1;
... sql
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
...

2. Data Exploration & Cleaning
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.

SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

1. WRITE A SQL QUERY TO RETRIEVE SALES DONE ON 2022/11/05
SELECT * FROM RETAIL_SALES WHERE SALE_DATE = '2022-11-05';

2. WRITE A SQL QUERY TO RETRIEVE SALES FROM CLOTHING CATEGORY,QUANTITY IS MORE THAN 4, AND SALES 
 HAPPENED ON NOV 2022
SELECT *
FROM retail_sales
WHERE lower(category) = 'clothing'
AND quantity >= 4
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';

3. WRITE A SQL QUERY TO CALCULATE TOTAL SALES BASED ON EACH CATEGORY
SELECT CATEGORY , SUM(TOTAL_SALE),COUNT(*) FROM retail_sales
GROUP BY CATEGORY;

4. WRITE A SQL QUERY TO RETRIEVE AVERAGE AGE OF CUSTOMERS FROM BEAUTY CATEGORY
SELECT (AVG(age) FROM retail_sales WHERE lower(CATEGORY) = 'beauty';
SELECT ROUND((AVG(age), 2) FROM retail_sales WHERE lower(CATEGORY) = 'beauty';
SELECT  distinct age FROM retail_sales WHERE lower(category) = 'beauty';

5. WRITE A SQL QUERY TO RETRIEVE ALL TRANSACTIONS WHERE TOTAL_SALE IS MORE THAN 1000
SELECT * FROM retail_sales WHERE total_sale > 1000;

6. WRITE A SQL QUERY TO FIND TOTAL TRANSACTIONS MADE BY EACH GENDER IN EACH CATEGORY
SELECT CATEGORY , GENDER, COUNT(*) FROM retail_sales 
GROUP BY CATEGORY , GENDER
order by 1;

7. WRITE A SQL QUERY TO CALCULATE AVERAGE SALE FOR EACH MONTH AND FIND OUT BEST SELLING MONTH OF EACH YEAR 
SELECT * FROM (
SELECT 
EXTRACT(YEAR FROM sale_date) as year,
EXTRACT(MONTH FROM sale_date) as month,
avg(total_sale),
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY avg(total_sale)DESC)
as Rank
FROM retail_sales
GROUP BY year, month)
AS t1 WHERE Rank = 1;

SELECT * FROM retail_sales;

8. WRITE A SQL QUERY TO FIND TOP 10 CUSTOMERS BASED ON HIGHEST TOTAL_SALES
SELECT customer_id, SUM(total_sale)
FROM retail_sales GROUP BY customer_id
ORDER BY 2 DESC
LIMIT 10;

9. WRITE A SQL QUERY TO FIND NUMBER OF UNIQUE CUSTOMERS FROM EACH CATEGORY
SELECT category, COUNT(DISTINCT customer_id) as unique_customers
FROM retail_sales
GROUP BY 1;

10.WRITE A SQL QUERY TO FIND EACH SHIFT AND NUMBER OF ORDERS IN EACH SHIFT EXAMPLE(UPTO 12AM MORNING SHIFT, 12-17 AFTERNOON , ELSE EVENING)
WITH hourly_sales 
AS (
SELECT *,
CASE 
    WHEN EXTRACT(HOUR from sale_time) < 12 THEN 'Morning'
	WHEN EXTRACT(HOUR from sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
	ELSE 'Evening'
	END as shift
FROM retail_sales
) 
SELECT shift,
COUNT(*) as total_sales
FROM hourly_sales 
GROUP BY shift;

Findings:
Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

Reports:
Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
Trend Analysis: Insights into sales trends across different months and shifts.
Customer Insights: Reports on top customers and unique customer counts per category.

Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.




