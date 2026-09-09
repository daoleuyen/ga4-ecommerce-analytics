### Query 06: Which countries have the highest number of active users?

* SQL code

```sql
SELECT
  geo.country AS country,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
GROUP BY 1
ORDER BY 2 DESC;
```
* Query results
  <img width="284" height="135" alt="{70DD11DB-CD52-4EE8-A63C-68E12913DF76}" src="https://github.com/user-attachments/assets/93131aba-b0d0-4667-a95c-c7f654d1211a" />
