# Project Overview
  This project analyzes store-level sales performance to uncover key revenue drivers. It identifies trends across products, regions, and customer segments, highlights the areas contributing most to overall sales, and delivers actionable recommendations for business stakeholders.
#### Key skills demonstrated:
- SQL (Joins, CTEs, Window functions, aggregations)
- Exploratory Data Analysis
- Tableau Visualization
- Business insight

## Business Questions
1. How do yearly and quarterly sales trends change over time?
2. Which products perform best and worst?
3. What do the core KPIs (Sales, Products, Customers, Avg Price, Quantity Sold) reveal?
4. How are customers segmented by spending levels?
5. Who are the top revenue‑generating customers?

## Exploratory Data Analysis
- Mountain and Road bikes are the top revenue drivers.
- United States has the largest consumers based but France produced the highest-value customers.
- Road bike prices dropped by 50% in year 2012, indicating a significant shift in pricing.
- 2013 recorded the highest sales with Mountain Bikes as the highest contributor.
- Sales performance show no meaningful difference between male and female customer groups. Both contribute significantly to total revenue.

<details>
 <summary><b>Sample Queries</b></summary>
  
```sql
-- Joins and CTEs --
with cte_gender (subcategory, gender, total_sales, ranking) as
(select dp.subcategory, dc.gender, sum(fs.sales_amount),
dense_rank() over(partition by dc.gender order by sum(fs.sales_amount)desc)
from fact_sales fs
join dim_customers dc
	on fs.customer_key = dc.customer_key
join dim_products dp
	on fs.product_key = dp.product_key
group by dp.subcategory, dc.gender
)
select *
from cte_gender cg1
join cte_gender cg2
	on cg1.subcategory = cg2.subcategory
where cg1.gender ='female'
		and cg2.gender = 'male';
```
```sql
-- Subqueries --
select *
from 
	(select dp.product_name, sum(sales_amount), row_number() over (order by sum(sales_amount) desc) as ranking
	from fact_sales fs
	join dim_products dp 
		on fs.product_key = dp.product_key
	group by dp.product_name
	order by 2 desc) sales_rank
where ranking <= 10
```
</details>

## Tableau Dashboard 
- KPI Summary
- Yearly and Quarterly sales trend
- Category level sales performance
- Top and Bottom products by Sales
  
#### Dashboard preview:
<a href="https://shorturl.at/WXtCl">
	<img src="Sales_dashboard.gif" width="600">
<a/>

## Recommendations
- Increase targeted campaigns to young adults, especially in Mountain and Road Bikes.
- Develop targeted campaigns for French consumers, leveraging their strong purchasing behaviour.
- Launch seasonal promotions aligned with Tour De France to drive higher engagement for Mountain and Road bikes.
- Review Road bike pricing strategy to ensure consistency across years.
- Add bundle promotions to increase sales in clothing and accessories.
- Conduct a deeper analysis in 2013 improvements to replicate winning strategies in future periods.
