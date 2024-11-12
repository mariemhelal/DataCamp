
touch bucketing_users_by_orders.sql
cat << EOF > bucketing_users_by_orders.sql
1 -- Store each user's count of orders in a CTE named user_orders
2 with user_orders as (
3     SELECT 
4         user_id,
5         count (distinct order_id) AS orders
6     FROM orders
7     GROUP BY user_id
8 )
9 SELECT
10     -- Write the conditions for the three buckets
11     CASE
12     WHEN orders < 8 THEN 'Low-orders users' 
13     WHEN orders < 15 THEN 'Mid-orders users'
14     ELSE 'High-orders users'
15     END AS order_group,
16     -- Count the distinct users in each bucket
17     count (distinct user_id) AS users
18 FROM user_orders
19 GROUP BY order_group;
