### Query 03: Which traffic sources and mediums generate the most sessions, and what percentage of total sessions does each one represent?

* SQL code

```sql
WITH raw_data1 AS (
  SELECT
    traffic_source.source AS session_source,
    traffic_source.medium AS session_medium,
    COUNT(DISTINCT CONCAT(
      user_pseudo_id, '-', 
      CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key='ga_session_id') AS STRING)
    )) AS sessions
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  WHERE event_name = 'session_start'
  GROUP BY 1,2
)

SELECT
  *,
  SAFE_DIVIDE(sessions, SUM(sessions) OVER()) AS pct_sessions
FROM raw_data1;
```
* Query results
<img width="534" height="117" alt="{99782FFF-2B6B-4CE2-8437-3204091D9595}" src="https://github.com/user-attachments/assets/af93866a-1689-4337-93f9-cf4527b30695" />
