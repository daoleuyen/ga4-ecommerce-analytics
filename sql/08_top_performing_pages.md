### Query 08: Which pages receive the most views, and how many active users visit each one?

* SQL code

```sql
SELECT
  (SELECT value.string_value FROM UNNEST(event_params) WHERE key='page_location') AS page_path,
  COUNTIF(event_name = 'page_view') AS views,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE event_name = 'page_view'
GROUP BY 1
ORDER BY 2 DESC;

```
* Query results
  ![Uploading {706A24B0-3BF7-41B4-8947-7B16245A6E8A}.png…]()
