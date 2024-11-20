# SQL-Finance-Domain-Data-Analytics-for-Atliq-Hardware




## Company Overview

Atliq Hardware, a prominent hardware production company. Their business is growing rapidly over the years but they rely on excel sheets before but as the company is growing that is not possible hence for sophisticated data transparency, they have now moved to using MYSQL Database. And the product owner now needs information on Finance analytics We will be looking into fullfilling requirements assigned by Product Owner in this project.


## Task 1 - Croma India Gross sales Report: Monthly Product Transactions


This Task focuses on analyzing individual product-level sales for Croma India on a monthly basis for the fiscal year (FY) 2021. By leveraging MySQL, I extracted, transformed, and analyzed data to provide insights into customer transactions, product performance, and financial metrics.


### Task Objectives

- Aggregate monthly sales data for Croma India for FY 2021.
- Retrieve detailed product and variant information for all transactions.
- Calculate gross price totals for transactions by integrating multiple data sources.
- Utilize MySQL User-Defined Functions (UDFs) to simplify fiscal year and quarter calculations.



### Dataset Overview

The analysis uses the following tables:

- fact_sales_monthly: Contains monthly sales transaction data.
- dim_customer: Provides customer details, including codes and names.
- dim_product: Includes product and variant details.
- fact_gross_price: Contains gross price per product for each fiscal year.

### Steps and Queries

Step 1: Exploring Sales Data

I began by inspecting the monthly sales data and identifying relevant customer records for Croma India:


SELECT * FROM fact_sales_monthly;
SELECT * FROM dim_customer WHERE customer LIKE '%Croma%';

Result: Identified the customer code for Croma India: 90002002.


### Step 2: Filtering Sales Data for FY 2021

To filter Croma India’s transactions for FY 2021, we wrote a function to calculate the fiscal year by adding 4 months to the transaction date:


SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND YEAR(DATE_ADD(date, INTERVAL 4 MONTH)) = 2021;


Optimization: Created a User-Defined Function (UDF) to simplify fiscal year filtering:

Updated query using UDF:

SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND get_fiscal_year(date) = 2021;


