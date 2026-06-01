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

