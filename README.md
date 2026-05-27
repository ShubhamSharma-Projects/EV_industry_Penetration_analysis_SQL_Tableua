# EV_industry_Penetration_analysis_SQL_Tableua
## 🔍Project overview 
A Leading EV manufacturer in North America, Plans to expand its presence in the Indian electric vehicle market,where its current market share is 2%. To support this expansion strategy,the company's leadership team initiated a comprehensice market study to understand India's rapidly growing EV and Hybrid vehicle ecosystem.

This project focuses on analyzing India's EV market using data analytics and business intelligence to uncover key trend,market oppurtunity, consumer adoption pattern,competitive insight.Acting as an data analyst for AtilQ Motors,the analysis was performed using SQL,Tableau,Excel to evaluate sales performance,penetration rate,top-performing states,vehicle category,manufacturer-wise growth.

## 🎯Business Problem
To identify growth oppurtunity and develop data-driven market expansion strategy that would help AtliQ Motors stringth its presence and improve its competitiveness in the EV industry.

## 📌Project Objectives
1. Analyzing  the Indian Electric Vehicle(EV) and hybrid vehicle market to understand current industry trend and growth patterns.
2. Identifying the top-performing states and regions based on EV sales and penetration rates.
3. Evaluating the performance of diffrent vehicle categories,including 2-Wheelers and 4-Wheelers, within the EV market
4. Studying the EV adoption pattern on a yearly basis and market expansion oppurtunities 
5. Performing Sql Based analysis to answer key business question and extract actional insight.

## 🧹Data Cleaning and Preparation
I cleaned the dataset and transformed it to ensure the data accuracy and consistency
Data Cleaning steps
- checking and handling of null and missing values
- Correct Date Format
- Removal of the duplicates records
- converting the mismatched data type

## SQL Analysis
### 1) Identify the top 5 states with the highest penetration rate in 2-wheeler and 4-wheeler EV sales in FY 2022,2024.
``` sql

select * from
	(
		select state,
		vehicle_category
		penetration_rate_2022,
		penetration_rate_2024,
		rank()over(partition by vehicle_category order by trend desc) as rnk

		from(
		select 
		state,vehicle_category,

		 max(case when d.fiscal_year='2022' then 
		 (s.electric_vehicles_sold) / (s.total_vehicles_sold) * 100  end)AS penetration_rate_2022,
		 max(case when d.fiscal_year='2024' then 
		 (s.electric_vehicles_sold) / (s.total_vehicles_sold) * 100  end)AS penetration_rate_2024,
		 
		max(case when d.fiscal_year='2024' then 
		 (s.electric_vehicles_sold) / (s.total_vehicles_sold) * 100  end)-max(case when d.fiscal_year='2022' then 
		 (s.electric_vehicles_sold) / (s.total_vehicles_sold) * 100  end)  as trend
		 
		 from  sales_state s
		 join dim_date d on s.datee=d.datee
		where fiscal_year in ('2022','2024') 
		 group by state,vehicle_category 
		 order by state ,trend desc
		 )t
		 order by vehicle_category
)t2
where rnk<=5
;
```
