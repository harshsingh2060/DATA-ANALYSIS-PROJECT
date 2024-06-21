# project
Analysing Sales Data with Excel,Sql and Visualizaton

PROJECT - SALES DATA OF PRODUCTS
identify the best-selling products, calculate revenue metrics such as total sales and profit margins, and create visualizations to present our findings effectively
 
EXCEL-
step 1 - cleaning the data and changing basic column width and proper formatting category
a, fixed columns width for purchase address and used proper date format for (order_date) column and 
checked for missing data and inconsistencies
gave first column the name (id) because it was empty and had attributes of a unique identifier
 
then converted the excel file to csv file and imported it to sql compiler (sqlliteonline)

we have some questions to analyse the data 

step 2
in sql complier
we used this code to analyse the top 5 best selling product in the table
-
**********
with cte as
(SELECT * FROM sales)

select product,round(sum(Sales),0) su  from cte 
group by 1
order by su desc
limit 5
************
with this code we will find sales trend over time along with its cumulative sum

********
with cte as
(SELECT * FROM sales)
,
cte2 as
(select product ,month,round(sum(Sales),0) as sum from cte
group by 1,2
order by 1,2)
select *,sum(sum) over(partition by product order by month) as cumul from cte2 
*****
with this code when can see the month in which the product had maximum sales and its total sale amount till now.
******
with cte as
(SELECT * FROM sales)
,
cte2 as
(select product ,month,round(sum(Sales),0) as sum from cte
group by 1,2
order by 1,2),
cte3 as
(select *,sum(sum) over(partition by product order by month) as cumul from cte2 ),
cte4 as
(select product,month,max(sum) as maximum_sale,max(cumul) as total_sale
from cte3
group by 1
order by 1,2)
select * from cte4

********
with this we can see best selling products,their sales per month and sum of sales amount and total cumulative sum according to total sales amount
*****
with cte as
(SELECT * FROM sales)
,
cte2 as
(select product ,month,round(sum(Sales),0) as sum from cte
group by 1,2
order by 1,2),
cte3 as
(select *,sum(sum) over(partition by product order by month) as cumul from cte2 ),
cte4 as
(select product,month,max(sum) as maximum_sale,max(cumul) as total_sale
from cte3
group by 1
order by 1,2),
cte5 as
(select product,round(sum(sales),0) as sum from cte
group by 1
order by sum desc)
select * from cte3
order by sum desc
