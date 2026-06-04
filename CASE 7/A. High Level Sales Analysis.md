## Q1
```sql
SELECT product_name,SUM( qty )
FROM balanced_tree.sales AS sales JOIN balanced_tree.product_details
AS details ON sales.prod_id = details.product_id
GROUP BY product_name
```
