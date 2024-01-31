# AtliQ Hardware - Sales Insights Data Analysis

## <span> Table of Contents </span>
* [Problem Statement](#problem-statement)
* [AIMS Grid](#aims-grid)
* [Tools Used](#tools-used)
* [ER Diagram](#er-diagram)
* [Analysis using SQL](#analysis-using-sql)
* [Power BI Analysis & Dashboard](#power-bi-analysis-and-dashboard)
  
### Problem Statement

AtliQ Hardware, an India-based company specializing in computer hardware and peripherals, faces challenges in tracking sales and achieving growth in a dynamically evolving market. With its headquarters in Delhi and numerous regional offices across India, the company serves various clients including surge stores and Nomad stores.

The sales director encounters difficulties as the market expands rapidly, leading to a decline in overall sales. The absence of concrete data and reliance on verbal reports from regional managers exacerbates the problem. Despite verbal updates, the director lacks tangible evidence to assess the business's performance accurately.

The sales director seeks a comprehensive data visualization tool to address these issues and enable data-driven decision-making. Such a tool would provide clear insights into regional and overall sales trends, facilitating informed decisions to drive business growth.

In this project, we aim to assist AtliQ Hardware in developing its own sales dashboard using Power BI. This dashboard will offer real-time access to sales data, empowering the company to make strategic decisions based on concrete insights and thereby enhance its sales performance.

Dataset: https://codebasics.io/resources/sales-insights-data-analysis-project

### AIMS Grid

1. Purpose - To unlock sales insights that were not visible before for the sales for decision support and automate them to reduce manual time spent in data gathering.
2. Stake Holders
    * Sales Director
  	* Marketing Team
    * Customer Service Team
    * Data & Analytics Team
    * IT
3. End result - An automated dashboard providing quick and latest sights to support Data-driven decision making.
4. Success Criteria -
* Dahboard uncovering sales order insights with the latest data available.
* Sales team able to make better decisions and prove 10% cost saving of total spend.
* Sales analysis stops data gathering manually to save 20% of business time and reinvest it value added activity.

### Tools Used
* MySQL
* Microsoft Power BI
* Power Query Editor

### ER Diagram
![sales drawio](https://github.com/karlyndiary/AtliQ-Hardware-Sales-Insights-Data-Analysis/assets/116041695/842e325a-083e-46e0-a803-c60c67a2ba57)

### Analysis using SQL

Server -> Data Import -> Select from self-contained file -> Import Progress Tab -> Start Import

1. Total no of products sold in 2020
```
SELECT count(customer_code) as customer_total, sales_quantity
FROM(
     SELECT customer_code, sum(sales_qty) as sales_quantity
     FROM atliq.transactions 
     WHERE year(order_date) = 2020
     GROUP BY customer_code) AS total_products_sold;
```
The total no of products sold in 2020 is 14933.

2. Total Revenue made in 2020 [2017 has 2 international clients]
```
SELECT sum(sales_amount) AS total_revenue_2020
FROM atliq.date d
INNER JOIN atliq.transactions t 
ON t.order_date = d.date
WHERE year = 2020;
```
Total Revenue made in 2020 is Rs 14,22,35,559/-

3. Total Revenue per month in 2020
```
SELECT month_name AS month, sum(sales_amount) AS sales_amount
FROM atliq.date d
INNER JOIN atliq.transactions t 
ON t.order_date = d.date
WHERE year = 2020
GROUP BY month_name;
```
The highest revenue in 2020 is February with Rs 2,69,25,734/-

4. Total Revenue per month in 2020 by the Mumbai Market
```
SELECT month_name AS month, sum(sales_amount)AS sales_amount
FROM atliq.date d
INNER JOIN atliq.transactions t 
ON t.order_date = d.date
WHERE year = 2020 AND market_code = 'Mark002'
GROUP BY month_name
ORDER BY sales_amount DESC;
```
The highest revenue in 2020 is February with Rs 49,68,501/- by the Mumbai Market

5. What are the peak sales months?
```
SELECT month_name as month, year, sum(sales_qty) as total_sales 
FROM atliq.date d
INNER JOIN atliq.transactions t 
ON t.order_date = d.date
WHERE year = 2020
GROUP BY month_name
ORDER BY total_sales DESC;
```
March produced a sale of 73384 in 2020.

6. How many unique customers are there in the dataset?
```
SELECT DISTINCT count(*) AS customers 
FROM atliq.customers;
```
There are 38 unique customers.

7. Which customers generate the highest sales volume?
```
SELECT customer_name, sum(sales_qty) AS sales_amount
FROM atliq.customers c
JOIN atliq.transactions t
ON t.customer_code = c.customer_code
WHERE year(order_date) = 2020
GROUP BY customer_name
ORDER BY sales_amount DESC;
```
Electricalsara Stores with 100374, Premium Stores with 28211, and Surge Stores with 22317 are the top 3 customers who generated the highest sales volumn in 2020.

8. What are the different customer types present?
```
SELECT DISTINCT customer_type
FROM atliq.customers; 
```
Brick & Mortar and E-Commerce are the two customer types.

9. What are the sales performances across different markets?
```
SELECT markets_name, sum(sales_amount) AS sales_amount
FROM atliq.transactions t
JOIN atliq.markets m
ON m.markets_code = t.market_code
GROUP BY market_code
ORDER BY sales_amount DESC; 
```


10. Which products contribute the most to sales revenue?
```
SELECT product_type, sum(sales_amount) AS sales_amount
FROM atliq.transactions t
JOIN atliq.products p
ON p.product_code = t.product_code
GROUP BY product_type
ORDER BY sales_amount DESC; 
```


11. Can we identify the top-selling products in each market?
```
SELECT markets_name, product_type, sum(sales_amount) AS sales_amount
FROM atliq.products p 
JOIN atliq.transactions t
ON p.product_code = t.product_code
JOIN atliq.markets m 
ON m.markets_code = t.market_code
GROUP BY markets_name
ORDER BY sales_amount DESC;
```
In Delhi NCR of Own Brand product type 

12. What is the distribution of customers across different customer types?
```
SELECT customer_type, COUNT(*) AS customer_count
FROM atliq.customers
GROUP BY customer_type;
```


13. What is the overall sales quantity and sales amount?
```
SELECT sum(sales_qty) AS sales_quantity, sum(sales_amount) AS sales_amount
FROM atliq.transactions;
```
The overall sales quantity is and sales amount is

### Power BI Analysis and Dashboard

#### ETL (Extract, Transform, Load) & Data Cleaning - Power Query

1. The market table has two international markets with no zones allocated. Filtering it out using the dropdown. or Market table -> Home tab -> Transform data -> Transform data 
```
= Table.SelectRows(sales_markets, each ([zone] <> ""))
```
2. The transaction table has a ton of 0 and negative values in the sales_amount column.
```
= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))
```
3. Converting USD currency to INR [General currency conversion for now]
Add column -> Conditional column
```
= Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*83 else [sales_amount])
```

#### Key Measures (DAX):

* Profit Margin % = DIVIDE([Total Profit Margin],[Revenue],0)
* Profit Margin Contribution % = DIVIDE([Total Profit Margin],CALCULATE([Total Profit Margin],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
* Revenue = SUM('sales transactions'[sales_amount])
* Revenue Contribution % = DIVIDE([Revenue],CALCULATE([Revenue],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))
* Revenue LY = CALCULATE([Revenue],SAMEPERIODLASTYEAR('sales date'[date]))
sales quntity = SUM('sales transactions'[sales_qty])
* Total Profit Margin = SUM('Sales transactions'[Profit_Margin])
* Profit Target1 = GENERATESERIES(-0.05, 0.15, 0.01)
* Profit Target Value = SELECTEDVALUE('Profit Target1'[Profit Target])
* Target Diff = [Profit Margin %]-'Profit Target1'[Profit Target Value]

#### Dashboard 
![Atliq Hardware Sales Insight Dashboard_page-0001](https://github.com/karlyndiary/AtliQ-Hardware-Sales-Insights-Data-Analysis/assets/116041695/acddd264-b5e9-4915-8940-ab3c74c7c409)
![Atliq Hardware Sales Insight Dashboard_page-0002](https://github.com/karlyndiary/AtliQ-Hardware-Sales-Insights-Data-Analysis/assets/116041695/f2d8bb71-17b7-4e06-a30b-b3e678f74340)
![Atliq Hardware Sales Insight Dashboard_page-0003](https://github.com/karlyndiary/AtliQ-Hardware-Sales-Insights-Data-Analysis/assets/116041695/bc69e271-e7e1-4649-a7ce-dfd8a0e089a7)
