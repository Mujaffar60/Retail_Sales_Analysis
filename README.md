# Retail Sales Analysis - SQL Project

## Project Overview:

**Project Title**: Retail Sales Analysis

**Database**: ``` retail_sales ```

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives:

1. Set up a retail sales database: Create and populate a retail sales database with the provided sales data.

2. Data Cleaning: Identify and remove any records with missing or null values.

3. Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.

4. Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

# Project Structure:

## 1. Database Setup
**Database Creation**: The project starts by creating a database named retail_sales.

**Table Creation**: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE retail_sales;

CREATE TABLE `retail sales analysis`
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
```

## 2.Data Analysis & Findings


The following SQL queries were developed to answer specific business questions:

**1. write a sql query to find the total number of transactions(transaction_id) made by each gender in each category.** 
```sql
select gender, category, count(transaction_id) as Total_transaction from `retail sales analysis`
group by category, gender
order by category;
```
<img width="236" height="134" alt="Q1" src="https://github.com/user-attachments/assets/7f2266b0-c7bf-4fd1-b93e-efc47cbe87ab" />

**2. write a sql query to find the top 5 customers based on the highest total sales.** 
```sql
select customer_id, sum(total_sale) as total_sale from `retail sales analysis`
group by customer_id
order by total_sale desc
limit 5;
```
<img width="145" height="108" alt="Q2" src="https://github.com/user-attachments/assets/d74b3d20-7ce2-44c8-b161-4006b92b0d30" />

**3. write a sql query to retrieve all transactions where the category is 'clothing and the quantity sold is more than 4 in the 
 month of Nov-2022.**
 ```sql
select*from `retail sales analysis`
where category= "Clothing" and quantiy>=4 and month(sale_date)=11 and year(sale_date)=2022;
```
<img width="694" height="314" alt="image" src="https://github.com/user-attachments/assets/5b5f3ff6-0b23-487f-aaea-c4e3e33d9f6c" />

**4. write a sql query to find all transactions where the total_sales is greater than 1000.**
```sql
select*from `retail sales analysis`
where total_sale > 1000;
```
<img width="702" height="285" alt="Q4" src="https://github.com/user-attachments/assets/d4b6a50c-4ff0-4faf-8646-07dc9927afd2" />

**5. write a sql query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)**
```sql
with shift_time as (
select *,
 case
when hour(sale_time) <12 then "Morning"
when hour(sale_time) between 12 and 17 then "Afternoon"
when hour(sale_time) >17 then "Evening"
end as Shift
from `retail sales analysis`
)
select Shift, count(*) as Total_orders from shift_time
group by Shift
order by
case
when shift = "Morinig" then 1
when shift = "Afternoon" then 2
when shift = "Evening" then 3
end;
```
<img width="155" height="82" alt="Q5" src="https://github.com/user-attachments/assets/f320c8d6-30c5-42c5-aa93-b0be5a79f689" />

**6. write a sql query to retrieve all column for sales made on '2022-11-05'**
```sql
select*from `retail sales analysis`
where sale_date= "2022-11-05";
```
<img width="706" height="226" alt="Q6" src="https://github.com/user-attachments/assets/f84de124-0150-4d99-9083-6f49e99299e2" />

**7. write a sql query to calculate the total sales for each category.**
```sql
select category,sum(total_sale) as Total_Sales from `retail sales analysis`
group by category;
```
<img width="151" height="81" alt="Q7" src="https://github.com/user-attachments/assets/d1bbde89-b5b7-4e11-b2f9-8c7eeb5735e5" />

**8. write a sql query to find the number of unique customers who purchased items from each category.**
```sql
select count(distinct customer_id) as unique_customers, category from `retail sales analysis`
group by category;
```
<img width="182" height="82" alt="Q8" src="https://github.com/user-attachments/assets/e5069155-1588-4f24-b54a-efe1a8e6f49e" />

**9. write a sql query to calculate average sale for each month.Find out best selling month in each year.** 
```sql
with monthly_sales as (
select
extract(year from sale_date) as Year, extract(month from sale_date) as Month, avg(total_sale) as avg_sale
 from `retail sales analysis`
group by Year, Month
),
Ranked_Sales as (
select Year,
Month,
avg_sale,
rank() over(partition by Year order by avg_sale desc) as sale_rank
from monthly_sales
)
select Year, Month, avg_sale from Ranked_Sales
where sale_rank = 1; 
```
<img width="159" height="63" alt="Q9" src="https://github.com/user-attachments/assets/e5bc13fd-06cd-44ff-ae85-c15c3f82fc81" />

**10. write a sql query to find the average age of customers who purchased items from the 'Beauty' category.**
```sql
select round(avg(age),2) as avg_age, category from `retail sales analysis`
where category= "Beauty";
```
<img width="129" height="48" alt="Q10" src="https://github.com/user-attachments/assets/4e4aa38f-5ce3-4b03-809e-9633cde7ee3f" />




## Findings
Customer Demographics: 
The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.

High-Value Transactions:
Several transactions had a total sale amount greater than 1000, indicating premium purchases.
Sales Trends:
Monthly analysis shows variations in sales, helping identify peak seasons.

Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

## Reports
Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.

Trend Analysis: Insights into sales trends across different months and shifts.

Customer Insights: Reports on top customers and unique customer counts per category.

## Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
