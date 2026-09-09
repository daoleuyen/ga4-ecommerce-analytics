### Query 05: How are active users distributed across device categories (mobile/desktop/tablet)?

* SQL code

```sql
SELECT
  device.category AS device_category,
  COUNT(DISTINCT user_pseudo_id) AS active_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
GROUP BY 1
ORDER BY 2 DESC;
```
*Query results
<img width="292" height="78" alt="{3D32EFF1-E4F7-4C90-9DFC-F4B6AA81E03F}" src="https://github.com/user-attachments/assets/50aaedfd-28bf-4bb9-92ec-86875f44badb" />
