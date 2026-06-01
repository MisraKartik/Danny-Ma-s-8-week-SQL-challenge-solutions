```sql
DROP TABLE IF EXISTS clean_weekly_sales;
CREATE TEMP TABLE clean_weekly_sales AS
SELECT
  TO_DATE(week_date, 'DD/MM/YY') AS week_date,
  EXTRACT(WEEK FROM TO_DATE(week_date, 'DD/MM/YY')) AS week_number,
  EXTRACT(MONTH FROM TO_DATE(week_date, 'DD/MM/YY')) AS month_number,
  EXTRACT(YEAR FROM TO_DATE(week_date, 'DD/MM/YY')) AS calendar_year,
  region, 
  platform, 
  segment,
  CASE 
    WHEN segment LIKE '%1' THEN 'Young Adults'
    WHEN segment LIKE '%2' THEN 'Middle Aged'
    WHEN segment = 'null' THEN 'Unknown' 
    ELSE 'Retirees' 
  END AS age_band,
  CASE 
    WHEN segment LIKE 'C%' THEN 'Couples'
    WHEN segment LIKE 'F%' THEN 'Families'
    ELSE 'Unknown' 
  END AS demographic,
  transactions,
  ROUND((sales::NUMERIC / transactions), 2) AS avg_transaction,
  sales
FROM data_mart.weekly_sales
```
