

# Finance Data Analysis Using SQL



## Summary

Generate a gross sales report for **Croma India** to track product-level sales aggregated on a monthly basis for FY 2021. This aids stakeholders in analyzing product performance and financial metrics.

---

### Business Requirements

- Product level Gross Sales Report
- Monthly & Yearly Sales Report
- Market Badge on Total Quantity Sold


---

### Dataset Origin & Description


- **fact_sales_monthly**: Monthly sales data.
- **dim_customer**: Customer details.
- **dim_product**: Product and variant details.
- **fact_gross_price**: Gross price by product for each fiscal year.

---

### Techniques & Procedure followed


---

#### Requirement -1 

Product level Gross Sales Report

        Generate a gross sales report for **Croma India** to track product-level sales aggregated 
        on a monthly basis for FY 2021. 
        This aids stakeholders in analyzing product performance and financial metrics.

- Step 1: Explored Sales Data and identified the customer code for Croma India: 90002002.
- Step 2: Filtered Sales Data for FY 2021

        To filter Croma India’s transactions for FY 2021,
        I have written a function to calculate the fiscal year by adding 4 months to the transaction date. 
        Optimized the code by Creating a User-Defined Function (UDF) to simplify fiscal year filtering.

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

