## Q1
```sql
SELECT DISTINCT(TO_CHAR(week_date, 'day')) AS week_day 
FROM clean_weekly_sales;
```

## Q2
```sql
WITH week_number_gen AS (
  SELECT GENERATE_SERIES(1,52) AS week_number
)
  
SELECT DISTINCT week_no.week_number
FROM week_number_gen AS week_no
LEFT JOIN clean_weekly_sales AS sales
  ON week_no.week_number = sales.week_number
WHERE sales.week_number IS NULL;
```
## Q3
```sql
SELECT calendar_year,SUM(transactions)
FROM clean_weekly_sales
GROUP BY calendar_year
ORDER BY calendar_year
```
## Q4
```sql
SELECT region,month_number,
SUM(sales)
FROM clean_weekly_sales
GROUP BY region,month_number
ORDER BY region,month_number
```
## Q5
```sql
SELECT platform,SUM(transactions)
FROM clean_weekly_sales
GROUP BY platform
```
## Q6
```sql
WITH monthly_platform_sales AS
(
  SELECT calendar_year,month_number,
  SUM(sales) FILTER(WHERE platform = 'Retail') AS retail_sales,
  SUM(sales) FILTER(WHERE platform = 'Shopify') AS shopify_sales,
  SUM(sales) AS total_sales
  FROM clean_weekly_sales
  GROUP BY calendar_year,month_number
  ORDER BY calendar_year,month_number
)

SELECT calendar_year,month_number,
ROUND(retail_sales * 100.0 / total_sales, 2) AS retail_percent,
ROUND(shopify_sales * 100.0 / total_sales, 2) AS shopify_percent
FROM monthly_platform_sales
```
## Q7
```sql
WITH demo_sales AS
( 
  SELECT calendar_year,
  SUM(sales) FILTER(WHERE demographic = 'Couples') AS couple_sales,
  SUM(sales) FILTER(WHERE demographic = 'Families') AS family_sales,
  SUM(sales) FILTER(WHERE demographic = 'Unknown') AS unknown_sales,
  SUM(sales)
  FROM clean_weekly_sales
  GROUP BY calendar_year
)

SELECT calendar_year,
ROUND( couple_sales * 100.0 / sum ,2) AS couple_percent,
ROUND( family_sales * 100.0 / sum ,2) AS family_percent,
ROUND( unknown_sales * 100.0 / sum ,2) AS unknown_percent
FROM demo_sales
ORDER BY calendar_year
```
## Q8
```sql
WITH age_demo_sale AS
(
  SELECT age_band,demographic,
  SUM(sales) FILTER(WHERE platform = 'Retail' ) AS sales
  FROM clean_weekly_sales
  GROUP BY age_band,demographic
)

SELECT age_band,demographic,
ROUND(sales * 100.0 / SUM(sales) OVER(),2) AS sale_percent
FROM age_demo_sale
ORDER BY sale_percent DESC
```
## Q9
```sql
SELECT 
  calendar_year, 
  platform, 
  ROUND(AVG(avg_transaction),2) AS correct_avg, 
  ROUND(SUM(sales)*1.0 / sum(transactions),2) AS avg_transaction_group
FROM clean_weekly_sales
GROUP BY calendar_year, platform
ORDER BY calendar_year, platform;
```
<img width="1165" height="314" alt="Screenshot 2026-06-01 at 7 41 25 PM" src="https://github.com/user-attachments/assets/d2523e8f-aba7-40ad-b48e-c91c9e0f765a" />

Conclusion: No
