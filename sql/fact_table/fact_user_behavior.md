### Fact Table: User Behavior
- **Grain:** 1 row / event[cite: 1].
- **Purpose:** Phân tích user behavior, event, device, geography, OS/browser và page[cite: 1].
- **Mapping:** Tab 03 - User Behavior & Audience[cite: 1].

* SQL code

```sql
CREATE TABLE `k51-dac-sql.GA4_ANALYSIS.fact_user_behavior` AS
SELECT
  FORMAT_DATE('%Y-%m-%d', PARSE_DATE('%Y%m%d', event_date)) AS event_date,
  user_pseudo_id,
  event_name,
  device.category AS device_category,
  geo.country AS country,
  geo.region AS region,
  geo.city AS city,
  device.operating_system AS operating_system,
  device.web_info.browser AS browser,
  (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS page_location,
  (SELECT value.string_value FROM UNNEST (event_params) WHERE key = 'page_title') AS page_title,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS ga_session_id,
  CONCAT(
    user_pseudo_id, '-',
    CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING)
  ) AS session_key,
  CASE
    WHEN (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'session_engaged') = '1' THEN 1 
    ELSE 0
  END AS is_engaged_session,
  1 AS event_count,
  CASE WHEN event_name = 'page_view' THEN 1 ELSE 0 END AS page_view_count
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE user_pseudo_id IS NOT NULL;
