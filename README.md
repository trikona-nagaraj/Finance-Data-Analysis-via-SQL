

# Finance Data Analysis Using SQL

This project focuses on achieving three key requirements placed by stakeholders of a firm. To meet these requirements, SQL was used to generate three reports by leveraging User-Defined Functions (UDFs), stored procedures, and joins. Finally, the reports were delivered in Excel format to the stakeholders.

---

### Business Requirements

Present project deals with a scenario in Atliq Company, where stakeholders have observed one of their markets; Croma India, is expanding rapidly. As a result, they require product-level aggregation of metrics to track individual product sales. The stakeholders need three key reports to analyze sales performance effectively:
- Product-Level Gross Sales Report
- Monthly & Yearly Sales Report
- Market Badge on Total Quantity Sold
  
These reports will help stakeholders run further product analytics in Excel.

The Product-Level Gross Sales Report should contain the following fields:

- Month
- Product Name
- Variant
- Sold Quantity
- Gross Price per Item
- Gross Price Total


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

        Generating a gross sales report for **Croma India** to track product-level 
        sales aggregated on a monthly basis for FY 2021. 
        This aids stakeholders in analyzing product performance and financial metrics.

- Step 1: Explored Sales Data and identified the customer code for Croma India: 90002002.
  
- Step 2: Filtered Sales Data for FY 2021

        To filter Croma India’s transactions for FY 2021, I have written a
        User Defined function (UDF) to calculate the fiscal year by adding 4 months
        to the transaction date.

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

        Finally, I computed the gross price total for each
        transaction by multiplying the sold quantity by the gross price


---


#### Requirement -2

Monthly & Yearly Sales Report

       To Analyze Croma India's financial performance, calculating and 
       aggregating gross sales prices on a monthly and yearly basis.

a) Monthly Sales Report

- Step 1: Calculated Gross Price Total per Transaction

        The gross price for each transaction is calculated by multiplying
        the sold quantity by the gross price. 

- Step 2: Aggregated Monthly Gross Sales

        To provide a single row individual month (ex: jan, feb etc.,) gross sales
        data is aggregated using the SUM function and grouped by the transaction date.

The result of this query provides insights into monthly sales trends and customer spending patterns.


b) Yearly Gross Sales Report


- Step 1: Calculated Gross Price Total per Fiscal Year

      The fiscal year for each transaction is determined using the `get_fiscal_year()` UDF,
      and the total gross sales for that year are calculated by summing the gross price totals.  

- Step 2: Aggregated Yearly Gross Sales

      To provide a single-row yearly gross sales summary, data is aggregated using the `SUM`
      function and grouped by the fiscal year.
  
The result of this query provides insights into yearly sales trends and overall financial performance.

---


#### Requirement -2

Market Badge on Total Quantity Sold

This requirment helps to determine a market badge based on total sold quantity for a 
specific market and fiscal year.  


- Step 1: Defining Market Badge Criteria

   The total quantity sold is evaluated to determine the appropriate badge:
  
  **Gold Badge**: Total sold quantity > 5 million.
  
  **Silver Badge**: Total sold quantity ≤ 5 million.  

- Step 2: A stored procedure is created to classify markets based on their total sold quantity 
in a given fiscal year.  
   

### Inputs and Outputs

- **Inputs**:
  - in_market : Market name (default is "India").
  - in_fiscal_year: Fiscal year for analysis.
- **Output**:
  - out_market_badge: Market badge (Gold or Silver).

This implementation provides a structured way to categorize markets based on sales performance,
offering clear insights into sales trends.


---

### Conclusion

This project shows how SQL can be used to create useful financial reports for business decisions. By using User-Defined Functions (UDFs), stored procedures, and SQL joins, I built three important reports—Product-Level Gross Sales Report, Monthly & Yearly Sales Report, and Market Badge Classification—to help Atliq Company analyze sales performance.

With these reports, stakeholders can easily track sales trends, product performance, and market growth. The methods used in this project provide a simple and effective way to analyze financial data, helping businesses make better decisions based on data.
