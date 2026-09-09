### Query 04: For each traffic source/medium, how many sessions start, product-view events, checkouts, purchases, and first-time purchasers are there?

* SQL code

```sql
SELECT
  traffic_source.source AS session_source,
  traffic_source.medium AS session_medium,
  COUNT(DISTINCT CONCAT(
    user_pseudo_id, '-', 
    CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key='ga_session_id') AS STRING)
  )) AS sessions,
  COUNTIF(event_name = 'view_item') AS item_view_events,
  COUNTIF(event_name = 'begin_checkout') AS checkouts,
  COUNTIF(event_name = 'purchase') AS ecommerce_purchase,
  COUNTIF(event_name='first_visit' AND EXISTS(
    SELECT 1 FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*` p
    WHERE p.user_pseudo_id = e.user_pseudo_id AND p.event_name='purchase'
  )) AS first_time_purchasers
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*` e
GROUP BY 1,2
ORDER BY 3 DESC;
```
* Query results
  <img width="621" height="123" alt="{15DBBD57-460C-49D0-A91D-F1117DA1361A}" src="https://github.com/user-attachments/assets/c409bda1-b3a1-4c5c-8a8a-90df899dd789" />
