### Query 02: What types of user actions (event_name) is the website tracking? List them all.

* SQL code

```sql
WITH raw_data1 AS (
  SELECT
    FORMAT_DATE ('%Y-%m-%d', PARSE_DATE('%Y%m%d', event_date)) AS event_dates,
    event_name,
    COUNT(event_name) AS event_count,
    COUNT(DISTINCT user_pseudo_id) AS active_users
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  GROUP BY event_dates, event_name
),
raw_data2 AS (
  SELECT
    FORMAT_DATE ('%Y-%m-%d', PARSE_DATE ('%Y%m%d', event_date)) AS event_dates,
    COUNT(event_name) AS total_event_count_per_day
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  GROUP BY FORMAT_DATE('%Y-%m-%d', PARSE_DATE('%Y%m%d',event_date))
)

SELECT
  t1.event_dates,
  t1.event_name,
  t1.event_count,
  ROUND(SAFE_DIVIDE(event_count, total_event_count_per_day), 3) AS event_percent,
  t1.active_users
FROM raw_data1 AS t1
LEFT JOIN raw_data2 AS t2
  ON t1.event_dates = t2.event_dates
ORDER BY t1.event_dates, event_percent DESC;
```
* Query results
<img width="624" height="131" alt="{F33B65D4-8840-4F10-B930-AD5610A727F9}" src="https://github.com/user-attachments/assets/7fd7fa70-2684-4f97-bcd6-7b99eb29bae3" />
