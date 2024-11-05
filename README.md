# Sales-Performance-Analysis
## Project overview ##
 This project contains a series of SQL queries designed to analyze sales data for a company. The queries focus on evaluating sales performance, customer behavior, and product trends, helping business analysts and managers derive insights from transactional data.

The database used in this analysis is named Sales Performance Analysis, and the main table for analysis is dbo.atobatele. Below is a breakdown of each query and its purpose:
## Project Scope ## 
This project covers sales performance analysis, the price variation during the period under review, product performance, sales trends, and frequency of sales and consistency.

## Business Objective ##
The purpose for this analysis is to identify if there are any noticeable sales trends over time, to find out the best and the worst selling product by identifying high performing and underperforming, identifying key customer, and to find out variation across product

## Document Purpose ##
This documentation serves as a guide for project stakeholders, providing insights into the sales performance, data sources, data analysis, visualizations, and any other relevant information.

## Use Case ##
This project for sales analysis will provide valuable insights and improvements across various operational aspects. Different stakeholders and decision making personnel within the organization could leverage these findings to enhance their functionality. Here are key stakeholders who could make use of this analysis and benefit from it.

**Shareholders**  
- Use of Analysis: shareholders can leverage on this analysis when carrying out decision making guarding the companies on product sales and improve overall efficiency in day-to-day operations.
-	Benefits: this will help then to knowing the best selling product in the market to invest in
  
**Management team**
-	Use of Analysis: The management team can benefit from insightful analysis and know customer preferences, satisfaction levels, and overall market trends.
-	Benefits: More targeted marketing strategies, improved customer engagement, and increased sales.

 **Operational managers**
-	Use of Analysis: operational managers are able to plan and keep tracking of the day to day business trend and a better understanding of customer preferences and issues.
- Benefits: Enhanced customer satisfaction, increased sales, and improved customer retention

 ## Skills Demostrated ##
 
-	Data Connection in SQL 
-	Data Profiling
-	Data Cleaning 
-	Data Analysis
-	Data Visualization Using power bi

 	 **Data Source**
The project utilizes a dataset containing information on sales performance analysis for a retail store. The dataset used for this analysis was gotten from a retail store. The dataset is a excel file and it consist single tables with ten column which are orderid,customerid,product,region,orderdate,quantity,unityprice,totalprice,revenue,month. The row’s in the table contains about 500001 which is use in the data analysis, this data is from the period of 2023 to 2024

**Data Connection Details**

in SQL, connecting to a EXCEL file involves specifying the location of the EXCEL file and defining the data import settings. Here are the steps taken in data connection in SQL Server. 

## Data Profiling ##

Data profiling in  SQL server helps to examining and analysing the characteristics and quality of data to gain insights into its structure, patterns, potential issues, and identify outliers. It helps to make informed decision on data cleaning and transformation. SQL server provides several tools and features that help to profile data effectively. These are column quality, column distribution and column profile.


## Data Cleaning and Processes ## 

Cleaning in SQL Server generally refers to tasks that help maintain the health, performance, and integrity of the database. This involve cleaning up unused data, optimizing indexes, updating statistics, and managing database files, among others.

Here are the key steps for cleaning up a SQL Server database
Delete Old Data: Remove records that are no longer needed, such as old log data, historical records, or obsolete entries.
 Update Statistics
Outdated statistics can cause poor query performance, so updating them is a good practice.
Cleanup Temporary Tables and Objects
Drop Temporary Tables:temporary tables (#TempTables) are dropped after use to avoid unnecessary space usage
Backup the Database
After performing cleanup operations, back was done on the database

## Data Analysis and Processes ##
Write queries to extract key insights based on the following questions. 

*retrieve the total sales for each product category*

*find the number of sales transactions in each region*

*find the highest-selling product by total sales value*

*calculate total revenue per product*

*calculate monthly sales totals for the current year*

*find the top 5 customers by total purchase amount*

*calculate the percentage of total sales contributed by each region*

*identify products with no sales in the last quarter*

### Setup
To run these queries, i made use of the following :

Create a database named Sales Performance Analysis.
Use the dbo.atobatele table, which should contain the following columns
product: The product sold.
total_price: The price of the product sold.
region: The region where the sale occurred.
orderdate: The date of the transaction.
customer_id: The ID of the customer.
revenue: The revenue generated by the sale.
customername: The name of the customer.



This query retrieves the total sales for each product category by summing up the `total_price` for each distinct `product` in the `dbo.atobatele` table.

```sql
SELECT product, SUM(total_price) AS total_sales
FROM dbo.atobatele
GROUP BY product;

## ```2```find the number of sales transactions in each region

SELECT region, COUNT(*) AS transaction_count
FROM dbo.atobatele
GROUP BY region;

--3--find the highest-selling product by total sales value.

``SELECT product, SUM(total_price) AS total_value
```FROM dbo.atobatele
```GROUP BY product
```ORDER BY total_value DESC
```OFFSET 0 ROWS FETCH NEXT 1 ROWS ONLY;  

---4--calculate total revenue per product

``SELECT product, SUM(revenue) AS total_revenue
``FROM dbo.atobatele
``GROUP BY product;

---5--calculate monthly sales totals for the current year
``SELECT MONTH(orderdate) AS month, SUM(Total_price) AS monthly_sales
``FROM dbo.atobatele
``WHERE YEAR(orderdate) = 2024
``GROUP BY MONTH(orderdate)
``ORDER BY month;

---6--find the top 5 customers by total purchase amount.

``SELECT top 5 customer_id,customername, SUM(total_price) AS total_purchase
``FROM dbo.atobatele
``GROUP BY customer_id,customername
``ORDER BY total_purchase DESC

---7--calculate the percentage of total sales contributed by each region


``WITH total_amount AS (
    ``SELECT SUM(total_price) AS total FROM dbo.atobatele
)
``SELECT 
   `` region,
    ``SUM(total_price) AS total_sales,
    ``CASE 
       `` WHEN (SELECT total FROM total_amount) = 0 THEN 0
       `` ELSE (SUM(total_price) * 1.0 / (SELECT total FROM total_amount) * 100)  -- Cast to decimal
   `` END AS sales_percentage
``FROM dbo.atobatele
``GROUP BY region
``order by region desc;




---8--identify products with no sales in the last quarter.


``  SELECT product,
``FROM dbo.atobatele
``WHERE orderdate >= DATEADD(quarter, -1, DATEADD(quarter, DATEDIFF(quarter, 0, GETDATE()), 0))
 `` AND orderdate < DATEADD(quarter, DATEDIFF(quarter, 0, GETDATE()), 0)
 `` AND customer_id IS NULL;


## Conclusion 
These queries provide insights into the sales performance across various dimensions, including products, regions, and customers. They can be adapted and extended to suit specific business needs, such as forecasting, performance comparison, and detailed analysis.


  





  
