## Q1
```sql
SELECT COUNT( DISTINCT user_id)
FROM clique_bait.Users
```
## Q2
```sql
WITH user_cookie AS (
SELECT user_id, COUNT(DISTINCT cookie_id) AS cookie_count
FROM clique_bait.users
GROUP BY user_id 
)
SELECT AVG(cookie_count)
FROM user_cookie
```
## Q3
```sql
WITH visit AS
(
  SELECT user_id,visit_id,
  EXTRACT( MONTH FROM event_time ) AS month_
  FROM clique_bait.users JOIN clique_bait.events 
  ON clique_bait.users.cookie_id = clique_bait.events.cookie_id
)

SELECT month_,COUNT(DISTINCT visit_id)
FROM visit
GROUP BY month_
```
## Q4
```sql
SELECT event_type , COUNT( event_type)
FROM clique_bait.events
GROUP BY event_type
ORDER BY event_type
```
## Q5
```sql
SELECT 
  100 * COUNT(DISTINCT e.visit_id)/
    (SELECT COUNT(DISTINCT visit_id) FROM clique_bait.events) AS percentage_purchase
FROM clique_bait.events AS e
JOIN clique_bait.event_identifier AS ei
  ON e.event_type = ei.event_type
WHERE ei.event_name = 'Purchase';
```
## Q6
```sql
```
## Q7
```sql
WITH page_view AS
( 
  SELECT *
  FROM clique_bait.events AS ev JOIN clique_bait.page_Hierarchy AS pg
  ON ev.page_id = pg.page_id
)
SELECT page_name, COUNT(visit_id) AS visits
FROM page_view
GROUP BY page_name
ORDER BY visits DESC
LIMIT 3;
```
## Q8
```sql
WITH cart AS
( 
  SELECT *
  FROM clique_bait.events AS ev JOIN clique_bait.page_hierarchy AS pg
  ON ev.page_id = pg.page_id
  WHERE ev.page_id NOT IN (1,2,12,13)
)
SELECT product_category,
COUNT(event_type) FILTER(WHERE event_type = 1) AS view_,
COUNT(event_type) FILTER(WHERE event_type = 2) AS cart_add
FROM cart
GROUP BY product_category
```
## Q9
```sql

