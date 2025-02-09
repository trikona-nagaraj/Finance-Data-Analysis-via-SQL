

# Finance Data Analysis Using SQL



## Summary

This project focuses on achieving three key requirements placed by stakeholders of a firm. To meet these requirements, SQL was used to generate three reports by leveraging User-Defined Functions (UDFs), stored procedures, and joins. Finally, the reports were delivered in Excel format to the stakeholders.

---

### Business Requirements

We are dealing with a scenario in Atliq Company, where stakeholders have observed that one of their markets, Croma India, is expanding rapidly. As a result, they require product-level aggregation of metrics to track individual product sales. The stakeholders need three key reports to analyze sales performance effectively:
Product-Level Gross Sales Report
Monthly & Yearly Sales Report
Market Badge on Total Quantity Sold
These reports will help stakeholders run further product analytics in Excel.

The Product-Level Gross Sales Report should contain the following fields:
Month
Product Name
Variant
Sold Quantity
Gross Price per Item
Gross Price Total


---

### Dataset Origin & Description

The data used in this project is not public and is part of an organization's internal records. With the consent of the company, this data is being used to perform this sample project for analyzing and producing the reports. Therefore, the dataset is not provided in this repository. However, you can follow the detailed procedure and steps outlined in the repository to understand how the analysis was conducted.
The following datasets were used:

- **fact_sales_monthly**: Monthly sales data.
- **dim_customer**: Customer details.
- **dim_product**: Product and variant details.
- **fact_gross_price**: Gross price by product for each fiscal year.

---

### Techniques & Procedure followed

---

#### Requirement -1 

Product level Gross Sales Report

        Generating a gross sales report for **Croma India** to track product-level sales aggregated 
        on a monthly basis for FY 2021. 
        This aids stakeholders in analyzing product performance and financial metrics.

- Step 1: Exploring Sales Data and identified the customer code for Croma India: 90002002.

```sql
SELECT * FROM fact_sales_monthly;
SELECT * FROM dim_customer WHERE customer LIKE '%Croma%';

SELECT * FROM dim_customer WHERE customer LIKE '%Croma%';     -- Identify customer code.

```

  
- Step 2: Filtered Sales Data for FY 2021

        To filter Croma India’s transactions for FY 2021,
        I have written a User Defined function (UDF) to calculate the fiscal year by adding 4 months to the transaction date.

```sql

SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND YEAR(DATE_ADD(date, INTERVAL 4 MONTH)) = 2021;         -- Filter for FY 2021.

```


User-Defined Function (UDF) to simplify fiscal year filtering:

```sql
CREATE FUNCTION 'get_fiscal_year'(calendar_date date)
  RETURNS integer
  DETERMINISTIC
BEGIN
  DECLARE Fiscal_Year INT;
  SET Fiscal_Year = YEAR(DATE_ADD(date, INTERVAL 4 MONTH));        -- Add 4 months to determine fiscal year.
  RETURN fiscal_Year
END
```

Updated query using UDF:

```sql
SELECT * FROM fact_sales_monthly 
WHERE customer_code = 90002002 
AND get_fiscal_year(date) = 2021;
```


- Step 3: Filtered Data by Quarter

        To analyze transactions by fiscal quarters, another UDF is created
        to determine the fiscal quarter and later created a Query to extract
        Q4 transactions.

- Step 4: Joining Product and Sales Data

        To enrich the sales data with product names and variants,
        I joined the fact_sales_monthly table with dim_product. 

- Step 5: Adding Gross Price

        I integrated gross price data by joining the fact_sales_monthly
        and fact_gross_price tables, using the fiscal year for unique price
        lookup

- Step 6: Calculating Gross Price Total

        Finally, I calculated the gross price total for each
        transaction by multiplying the sold quantity by the gross price


---


#### Requirement -2

Monthly & Yearly Sales Report

       Analyze Croma India's financial performance by calculating and 
       aggregating gross sales prices on a monthly and yearly basis for 
       enhanced decision-making.

a) Monthly Sales Report

1. Calculated Gross Price per Transaction: Multiply sold_quantity by gross_price.
2. Aggregated Monthly Sales: Sum gross sales grouped by the month.


- Step 1: Calculating Gross Price Total for Transactions

        The gross price for each transaction is calculated by multiplying
        the sold quantity by the gross price. 

- Step 2: Aggregating Monthly Gross Sales

        To provide a single row per month, gross sales data is aggregated
        using the SUM function and grouped by the transaction date.

The result of this query provides insights into monthly sales trends and customer spending patterns.


b) Yearly Gross Sales Report

1. Grouped Fiscal Year: Aggregated sales data using the get_fiscal_year() UDF.
2. Summarized Total Gross Sales: Calculated the yearly gross sales to identify financial performance.

- Step 1: Generating Yearly Sales Data

      The fiscal year and total gross sales for that year are calculated by grouping
      data by the fiscal year and summing the gross price totals.
  
      Here, get_fiscal_year() is the UDF I created to simplify the code and minimize t
      he complexity in code readability.

This query provides a concise yearly summary of gross sales, allowing stakeholders to assess the company's financial growth over time.


---


#### Requirement -2

Market Badge on Total Quantity Sold

This requirment helps to determine a market badge based on total sold quantity for a specific market and fiscal year.  

- **Gold Badge**: Total sold quantity > 5 million.  
- **Silver Badge**: Total sold quantity ≤ 5 million.


#### solution:

        Create a stored procedure to determine a market badge based 
        on total sold quantity for a specific market and fiscal year.  
  

### Inputs and Outputs
- **Inputs**:
  - in_market : Market name (default is "India").
  - in_fiscal_year: Fiscal year for analysis.
- **Output**:
  - out_market_badge: Market badge (Gold or Silver).

