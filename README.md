# AtliQ Hardware - Sales Insights Data Analysis

## <span> Table of Contents </span>
* [1. Problem Statement](#1-problem-statement)
* [2. AIMS Grid](#2-aims-grid)
* [3. ER Diagram](#3-er-diagram)
* [4. Power BI Dashboard](#4-power-bi-dashboard)
  
### Problem Statement :
In this project performed India-based AtliQ hardware company sales insights - A Data Analysis project.

AtliQ Hardware is a company which supplies computer hardware and peripherals to many clients such as surge stores, Nomad stores etc. across India. AtliQ Hardware head office is situated in Delhi, India and they have many regional office through out the India.

Sales director for this company is facing a lot of challenges is this the market is growing dynamically and sales director is facing issue in terms of tracking the sales in this dynamical growth market and he is having issues with growth of this bussiness, as overall sales was declining. He has regional manager for North India, South and Central India. Whenever he wants to get insights of thses region he would call these people and on the phone regional manager give some insights to him that this was the sales last quarter and we are going to grow by this much in the next quarter.

The problem was that all thses thing happening is verbal and these was mo proof with facts that how his business is affected and which made him frustraed as he can see that overall sales is declining but when he can ask regional manager, he is not getting complete picture of this bussiness and when he and this AtliQ hardware is big business. so to see insights clearly. and he will get proper insights anbd can take data driven decision to increase sales of hos company. All he wants is a simple data visualization tool which he can access on daily basis. By using such tools and technology one can make data driven decisiions which helps to increase the sales of the company. So, In this projects, we will help a company make its own sales related dashboard using power BI.

### AIMS Grid - Project Management Tool
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

### ER Diagram

### Power BI Dashboard
