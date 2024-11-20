# SQL-Finance-Domain-Data-Analytics-for-Atliq-Hardware

## Company Overview

Atliq Hardware is a prominent hardware production company. Their business has been growing rapidly over the years, but they previously relied on Excel sheets for data management. As the company expanded, this method became unsustainable. To enhance data transparency, they have transitioned to using a **MySQL Database**. The product owner now requires insights into **Finance Analytics**. This project focuses on fulfilling the requirements assigned by the Product Owner.

---

## Task 1 - Croma India Gross Sales Report: Monthly Product Transactions

This task focuses on analyzing individual product-level sales for **Croma India** on a monthly basis for the fiscal year (FY) 2021. By leveraging **MySQL**, I extracted, transformed, and analyzed data to provide insights into customer transactions, product performance, and financial metrics.

---

### Task Objectives

- Aggregate monthly sales data for **Croma India** for FY 2021.
- Retrieve detailed product and variant information for all transactions.
- Calculate gross price totals for transactions by integrating multiple data sources.
- Utilize **MySQL User-Defined Functions (UDFs)** to simplify fiscal year and quarter calculations.

---

### Dataset Overview

The analysis uses the following tables:

- **fact_sales_monthly**: Contains monthly sales transaction data.
- **dim_customer**: Provides customer details, including codes and names.
- **dim_product**: Includes product and variant details.
- **fact_gross_price**: Contains gross price per product for each fiscal year.

---

### Steps and Queries


```sql


Step 1: Exploring Sales Data

SELECT * FROM fact_sales_monthly;
SELECT * FROM dim_customer WHERE customer LIKE '%Croma%';

Result: Identified the customer code for Croma India: 90002002.


---


Step 2: Filtering Sales Data for FY 2021

To filter Croma India’s transactions for FY 2021, I wrote a function to calculate the fiscal year by adding 4 months to the transaction date:


SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND YEAR(DATE_ADD(date, INTERVAL 4 MONTH)) = 2021;


Optimization: Created a User-Defined Function (UDF) to simplify fiscal year filtering.

Updated query using UDF:

SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND get_fiscal_year(date) = 2021;


---


Step 3: Filtering Data by Quarter

To analyze transactions by fiscal quarters, another UDF is created to determine the fiscal quarter.

Query to extract Q4 transactions:


SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND get_fiscal_year(date) = 2021 
AND get_fiscal_quarter(date) = 'Q4'
ORDER BY date ASC;


----


Step 4: Joining Product and Sales Data

To enrich the sales data with product names and variants, I joined the fact_sales_monthly table with dim_product:


SELECT date, s.product_code, product, variant, sold_quantity
FROM fact_sales_monthly s
JOIN dim_product p ON s.product_code = p.product_code
WHERE customer_code = 90002002 
AND get_fiscal_year(s.date) = 2021 
AND get_fiscal_quarter(s.date) = 'Q4'
ORDER BY date ASC;


---


Step 5: Adding Gross Price

I integrated gross price data by joining the fact_sales_monthly and fact_gross_price tables, using the fiscal year for unique price lookup


SELECT date, s.product_code, product, variant, sold_quantity, gross_price
FROM fact_sales_monthly s
JOIN dim_product p ON s.product_code = p.product_code
JOIN fact_gross_price g 
ON s.product_code = g.product_code 
AND g.fiscal_year = get_fiscal_year(s.date)
WHERE customer_code = 90002002 
AND get_fiscal_year(s.date) = 2021 
AND get_fiscal_quarter(s.date) = 'Q4'
ORDER BY date ASC;


---


Step 6: Calculating Gross Price Total

Finally, I calculated the gross price total for each transaction by multiplying the sold quantity by the gross price


SELECT date, s.product_code, product, variant, sold_quantity, gross_price, 
ROUND(sold_quantity * gross_price, 2) AS Gross_price_total
FROM fact_sales_monthly s
JOIN dim_product p ON s.product_code = p.product_code
JOIN fact_gross_price g 
ON s.product_code = g.product_code 
AND g.fiscal_year = get_fiscal_year(s.date)
WHERE customer_code = 90002002 
AND get_fiscal_year(s.date) = 2021 
AND get_fiscal_quarter(s.date) = 'Q4'
ORDER BY date ASC;
---

