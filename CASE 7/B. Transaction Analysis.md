## Q1
```sql
SELECT COUNT( DISTINCT txn_id )
FROM balanced_tree.sales
```

## Q2
```sql
WITH uni_prod AS
(
  SELECT txn_id, prod_id
  FROM balanced_tree.sales
),
prod_count AS (
SELECT txn_id,COUNT(prod_id) AS prod_count
FROM uni_prod
GROUP BY txn_id )

SELECT AVG(prod_count)
FROM prod_count
```
## Q3
```sql
WITH rev_txn AS
(
  SELECT txn_id,qty * price - discount AS sale
  FROM balanced_tree.sales
),
rev_per_txn AS
(
  SELECT txn_id,SUM(sale) AS total_sale
  FROM rev_txn
  GROUP BY txn_id
  ORDER BY total_sale
)
SELECT 
PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY total_sale) AS _25th,
PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY total_sale) AS _50th,
PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY total_sale) AS _75th
FROM rev_per_txn
```
## Q4
```sql
WITH disc AS
(
  SELECT txn_id,
  qty * price * discount / 100.0 AS disc
  FROM balanced_tree.sales
),
disc_txn AS
(
  SELECT txn_id,SUM(disc) AS total_disc
  FROM disc
  GROUP BY txn_id
)
SELECT ROUND( SUM(total_disc) * 1.0 / COUNT(txn_id) ,2)
FROM disc_txn
```
## Q5
```sql
WITH txn_count AS (
 SELECT COUNT( DISTINCT txn_id ),member
 FROM balanced_tree.sales
 GROUP BY member
)

SELECT member, count * 100.0 / ( SELECT COUNT( DISTINCT txn_id ) FROM
   balanced_tree.sales )
FROM txn_count
```

## Q6
```sql
WITH rev As
(
  SELECT txn_id, price * qty AS total_price,
  qty * price * discount / 100.0 AS disc,
  member
  FROM
  balanced_tree.sales
),
member_rev AS
( 
  SELECT txn_id,SUM(total_price) AS total_rev,
  member
  FROM rev
  GROUP BY txn_id,member
),
total_rev AS
(
  SELECT member, SUM(total_rev),
  COUNT(member)
  FROM member_rev
  GROUP BY member
)
SELECT sum * 1.0 / count
FROM total_rev
```
