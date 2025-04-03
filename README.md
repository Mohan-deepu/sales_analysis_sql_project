# Sales Analysis SQL_Project
This project focuses on analyzing retail sales data using SQL queries to gain insights into customer purchasing behavior, product performance, and overall business trends. The dataset contains transaction records, including sales details, customer demographics, and product categories.

# Objectives
1. set up retail sales database:create a database  and import the data from the provided dataset
2. Data cleaning:identifying invalied data type and fixing it and identifying any missing values and removing them
3. Exploratory Data Analysis: perform basic EDA to understand the dataset
4. Business Analysis: uing sql queries answering the specific business problems and derive meaningful insights from the given data

# Project Structure
### 1 Database Setup
- ## Database Creation: Creating a Database named Sales_db
- ## Table creation: Creating  a table named retail_sales having the columns transactions_id,sale_date,sale_time,customer_id,gender,age,category,quantiy,groupby,cogs,total_sale

'''sql

  create database sale_db;

create table  retail_sales
(
    transactions_id	int,
    sale_date date,
	  sale_time time,
    customer_id	 int,
    gender varchar(20),
	  age	int,
    category varchar(20),
	  quantiy int,
	  price_per_unit  int,
	 cogs 	decimal(10,2),
    total_sale int 
);
'''
### 2 Data Exploration and Cleaning
- **Counting Records**: Counting number of records  present in the  table
- **counting Customers** : Counting number of unique customer in the table
- **null values** : checking for  any null  values present in the table using where condition

'''sql  
  1. total number of records
    select 
    count(*)
    from retail_sales;

   2.  how many unique customer we have 
     select
     count(distinct(customer_id))  as total_customer 
     from retail_sales;

   3. to find the null values
    select * from retail_sales
    where transactions_id is null
    or  sale_date is null
    or sale_time is null
    or customer_id is null
    or gender is null
    or age is null
    or category is null
    or quantiy is null
    or price_per_unit is null
    or cogs is null 
    or total_sale is null;
'''
### Data Analysis and Findings
- **following queries were developed  to answer the business questions**
 1. **write a query to retrieve all colums for sales made on 2022-12-18**
  '''sql
    select *from retail_sales where sale_date= "2022-12-18";
   '''
 2. **Write a SQL query to retrieve all transactions where the category is 'beauty' and the quantity sold is more than 10 in the month of Nov-2022**
  '''sql
      select *from retail_sales
      where category= "beauty"and quantity>=4
       and  date_format(sale_date,'%Y-%m')="2022-12";
    '''
 3. **Write a SQL query to calculate the total sales (total_sale) for each category**
  '''sql
    select category,
        sum(total_sale)as total_sale ,
        count(*) as total_order
        from retail_sales
         group by category;
    '''
 4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**
  '''sql
    select 
        round(avg(age), 2) as avg_age
        from retail_sales
        where category="beauty";
    '''
 5. **Write a SQL query to find all transactions where the total_sale is greater than 1000**
  '''sql
     select *
     from retail_sales
     where total_sale>1000;
  '''
 6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**
  '''sql
      select 
       category,
       gender,
       count(*)
       from retail_sales
      group by category,gender
      order by category;
   '''
 7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**
    '''sql
      select * from
        (select 
        year(sale_date) as y,
        month(sale_date)as m,
        avg(total_sale)as avg_sales,
        rank() over(partition by year(sale_date) order by avg(total_sale) desc) as ranks
        from retail_sales
        group by y,m
        order by y,avg_sales desc)  table1
        where ranks=1;
    '''
 8. **Write a SQL query to find the top 5 customers based on the highest total sales**
    '''sql
       select  customer_id,
       sum(total_sale) as  total_sale
       from retail_sales
       group by customer_id
       order by total_sale desc
       limit 5;
    '''   
 9. **Write a SQL query to find the number of unique customers who purchased items from each category**
     '''sql
        select 
        count(distinct(customer_id)) as no_unique_custm ,
        category 
        from retail_sales 
        group by  category;
    '''
 10. **Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)**
     '''sql
        select
        count(*)  as total_number_of_orders, shift
        from( select
        case 
        when hour(sale_time) < 12 then "morning"
        when hour(sale_time) between 12 and 17 then "afternoon"
        else "evening"
        end as shift
        from retail_sales) as shift_table 
        group by shift;
    '''
 11. **Identify the busiest time of day for sales**
     '''sql
        SELECT sale_time, COUNT(*) AS total_orders 
        FROM  retail_sales
        GROUP BY sale_time 
        ORDER BY total_orders DESC 
        LIMIT 1;
    '''
 12. **Find the average sales per day**
     '''sql
        SELECT sale_date, 
        AVG(total_sale) AS avg_sales
        FROM retail_sales
        GROUP BY sale_date;
    '''
 13. **Find the category with the highest profit (total_sale - cogs)**
     '''sql
        SELECT category, SUM(total_sale - cogs) AS total_profit 
        FROM retail_sales 
        GROUP BY category 
        ORDER BY total_profit DESC 
        LIMIT 1;
    '''
 14. **dentify the age group that spends the most on purchases**
     '''sql
        SELECT 
        CASE 
           WHEN age BETWEEN 18 AND 25 THEN '18-25'
           WHEN age BETWEEN 26 AND 35 THEN '26-35'
           WHEN age BETWEEN 36 AND 45 THEN '36-45'
           WHEN age BETWEEN 46 AND 60 THEN '46-60'
           ELSE '60+'
       END AS age_group,
       SUM(total_sale) AS total_spent
       FROM retail_sales
       GROUP BY age_group
       ORDER BY total_spent DESC
       LIMIT 1;
    '''
 15. **Find the percentage of total sales contributed by each category**
     '''sql
        SELECT category, 
        SUM(total_sale) AS category_sales, 
        (SUM(total_sale) * 100.0 / (SELECT SUM(total_sale) FROM retail_sales AS sales_percentage) ) as contribution_by_category
        FROM retail_sales
        GROUP BY category;
    '''

  

   
