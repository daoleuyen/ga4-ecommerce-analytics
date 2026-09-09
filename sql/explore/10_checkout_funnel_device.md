### Query 10: For each device category, how many sessions complete each step of the checkout path - begin checkout, add shipping info, add payment info, purchase?

* SQL code

```sql
WITH raw_data AS (
  SELECT
    device.category AS device_category,
    COUNT(DISTINCT CASE
      WHEN event_name = 'begin_checkout' THEN CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING))
      ELSE NULL
    END) AS session_begin_checkout,
    COUNT(DISTINCT CASE
      WHEN event_name = 'add_shipping_info' THEN CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING))
      ELSE NULL
    END) AS session_add_shipping,
    COUNT(DISTINCT CASE
      WHEN event_name = 'add_payment_info' THEN CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING))
      ELSE NULL
    END) AS session_add_payment,
    COUNT(DISTINCT CASE
      WHEN event_name = 'purchase' THEN CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING))
      ELSE NULL
    END) AS session_purchase
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  GROUP BY 1
)

SELECT
  *, 
  ROUND(SAFE_DIVIDE(session_add_payment, session_begin_checkout), 2) AS add_payment_rate,
  ROUND(SAFE_DIVIDE(session_purchase, session_add_payment), 2) AS purchase_rate
FROM raw_data
ORDER BY session_begin_checkout DESC;
```
* Query results
  <img width="632" height="79" alt="{D8DD7F26-752B-4983-835D-53AAD026D33B}" src="https://github.com/user-attachments/assets/ddff12b8-a45a-4ee7-bb36-f123c484f748" />
