# AtliQ Hardware - Sales Insights Data Analysis

## <span> Table of Contents </span>
* [Problem Statement](#problem-statement)
* [AIMS Grid](#aims-grid)
* [ER Diagram](#er-diagram)
* [Analysis using SQL](#analysis-using-sql)
* [Power BI Dashboard](#power-bi-dashboard)
  
### Problem Statement

In this project performed India-based AtliQ hardware company sales insights - A Data Analysis project.

AtliQ Hardware is a company which supplies computer hardware and peripherals to many clients such as surge stores, Nomad stores etc. across India. AtliQ Hardware head office is situated in Delhi, India and they have many regional office through out the India.

Sales director for this company is facing a lot of challenges is this the market is growing dynamically and sales director is facing issue in terms of tracking the sales in this dynamical growth market and he is having issues with growth of this bussiness, as overall sales was declining. He has regional manager for North India, South and Central India. Whenever he wants to get insights of thses region he would call these people and on the phone regional manager give some insights to him that this was the sales last quarter and we are going to grow by this much in the next quarter.

The problem was that all thses thing happening is verbal and these was mo proof with facts that how his business is affected and which made him frustraed as he can see that overall sales is declining but when he can ask regional manager, he is not getting complete picture of this bussiness and when he and this AtliQ hardware is big business. so to see insights clearly. and he will get proper insights anbd can take data driven decision to increase sales of hos company. All he wants is a simple data visualization tool which he can access on daily basis. By using such tools and technology one can make data driven decisiions which helps to increase the sales of the company. So, In this projects, we will help a company make its own sales related dashboard using power BI.

### AIMS Grid

1. Purpose - To unlock sales insights that were not visible before for the sales for decision support and automate them to reduce manual time spent in data gathering.
2. Stake Holders -
* Sales Director
* Marketing Team
* Customer Service Team
* Data & Analytics Team
* IT
3. End result - An automated dashboard providing quick and latest sights in order to support Data driven decision making.
4. Success Criteria -
* Dahboard uncovering sales order insights with latest data available.
* Sales team able to take better decisions and prove 10% cost saving of total spend.
* Sales analysis stop data gathering manually in order to save 20% business time andreinvest it value added activity.

### ER Diagram
![sales drawio](https://github.com/karlyndiary/AtliQ-Hardware-Sales-Insights-Data-Analysis/assets/116041695/842e325a-083e-46e0-a803-c60c67a2ba57)

### Analysis using SQL

Server -> Data Import -> Select from self-contained file -> Import Progress Tab -> Start Import
1. Total no of sales made in 2020
```
SELECT count(*) AS sales_total
FROM sales.date d
INNER JOIN sales.transactions t 
ON t.order_date = d.date
WHERE year = 2020;
```
21550

2. Total Revenue made in 2020
```
SELECT sum(sales_amount) AS total_revenue_2020
FROM sales.date d
INNER JOIN sales.transactions t 
ON t.order_date = d.date
WHERE year = 2020;
```
142235559

3. Total Revenue per month in 2020
```
SELECT month_name AS month, sum(sales_amount) AS sales_amount
FROM sales.date d
INNER JOIN sales.transactions t 
ON t.order_date = d.date
WHERE year = 2020
GROUP BY month_name;
```

4. Total Revenue per month in 2020 by the Mumbai Market
```
SELECT month_name AS month, sum(sales_amount)AS sales_amount
FROM sales.date d
INNER JOIN sales.transactions t 
ON t.order_date = d.date
WHERE year = 2020 AND market_code = 'Mark002'
GROUP BY month_name
ORDER BY sales_amount DESC;
```

5. What are the peak sales months?
```
SELECT month_name as month, year, sum(sales_amount) as total_sales 
FROM sales.date d
INNER JOIN sales.transactions t 
ON t.order_date = d.date
GROUP BY year, month_name
ORDER BY total_sales DESC;
```

6. How many unique customers are there in the dataset?
```
SELECT DISTINCT count(*) AS customers 
FROM sales.customers;
```

7. Which customers generate the highest sales volume?
```
SELECT DISTINCT customer_name, sum(sales_amount) AS sales_amount
FROM sales.customers c
JOIN sales.transactions t
ON t.customer_code = c.customer_code
GROUP BY customer_name
ORDER BY sales_amount DESC;
```

8. What are the different customer types present?
```
SELECT DISTINCT customer_type
FROM sales.customers; 
```

9. What are the sales performances across different markets?
```
SELECT markets_name, sum(sales_amount) AS sales_amount
FROM sales.transactions t
JOIN sales.markets m
ON m.markets_code = t.market_code
GROUP BY market_code
ORDER BY sales_amount DESC; 
```

10. Which products contribute the most to sales revenue?
```
SELECT product_type, sum(sales_amount) AS sales_amount
FROM sales.transactions t
JOIN sales.products p
ON p.product_code = t.product_code
GROUP BY product_type
ORDER BY sales_amount DESC; 
```

11. Which products contribute the most to sales revenue?
```
SELECT product_type, sum(sales_amount) AS sales_amount
FROM sales.transactions t
JOIN sales.products p
ON p.product_code = t.product_code
GROUP BY product_type
ORDER BY sales_amount DESC; 
```

12. Can we identify the top-selling products in each market?
```
SELECT markets_name, product_type, sum(sales_amount) AS sales_amount
FROM sales.products p 
JOIN sales.transactions t
ON p.product_code = t.product_code
JOIN sales.markets m 
ON m.markets_code = t.market_code
GROUP BY markets_name
ORDER BY sales_amount DESC;
```

13. Can we identify the top-selling products in each market?
```
SELECT markets_name, product_type, sum(sales_amount) AS sales_amount
FROM sales.products p 
JOIN sales.transactions t
ON p.product_code = t.product_code
JOIN sales.markets m 
ON m.markets_code = t.market_code
GROUP BY markets_name
ORDER BY sales_amount DESC;
```

14. What is the distribution of customers across different customer types?
```
SELECT customer_type, COUNT(*) AS customer_count
FROM sales.customers
GROUP BY customer_type;
```

15. What is the overall sales quantity and sales amount?
```
SELECT sum(sales_qty) AS sales_quantity, sum(sales_amount) AS sales_amount
FROM sales.transactions;
```

15. How do sales vary across different market zones over time?
```
SELECT m.markets_name, m.zone, year,
    month_name AS month,
    SUM(t.sales_amount) AS sales_amount
FROM sales.date d 
JOIN sales.transactions t 
ON t.order_date = d.date
JOIN sales.markets m ON m.markets_code = t.market_code
GROUP BY 
    m.markets_name, 
    m.zone,
    year,
    month
ORDER BY 
    year, 
    month,
    m.zone;

```

### Power BI Dashboard
