### Query 07: How are active users distributed across operating systems and browsers?

* SQL code

```sql
SELECT
  device.operating_system AS os,
  device.web_info.browser AS browser,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
GROUP BY 1,2
ORDER BY 3 DESC;
```
* Query results
  ![Uploading {6ED2D339-22CA-4324-BD48-D44A28DD8CFD}.png…]()
