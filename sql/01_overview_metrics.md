### Query 01: Overview metrics (Sessions, Purchase revenue, Ecommerce purchases, AOV, Purchase rate, View_product_rate, View Cart%, Cart Purchase%, Avg Basket Size, Revenue/User)

* SQL code

```sql
WITH raw_data1 AS (
  SELECT
    FORMAT_DATE('%Y-%m-%d', PARSE_DATE ('%Y%m%d', event_date)) AS event_date,
    -- Tính sessions
    COUNT(DISTINCT CONCAT(
      user_pseudo_id,
      CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING)
    )) AS sessions,
    -- Tính view_item_sessions
    COUNT(DISTINCT CASE
      WHEN event_name = 'page_view' THEN (CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING)))
      ELSE NULL
    END) AS view_item_sessions,
    -- Tính cart_sessions
    COUNT(DISTINCT CASE
      WHEN event_name = 'add_to_cart' THEN (CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING)))
      ELSE NULL
* Query results
<img width="633" height="124" alt="query1" src="https://github.com/user-attachments/assets/eb2402f0-3a20-4312-8ccc-152063bfe6a8" />

    END) AS cart_sessions,
    -- Tính purchase_sessions (số phiên truy cập có đơn hàng)
    COUNT(DISTINCT CASE
      WHEN event_name = 'purchase' THEN (CONCAT(user_pseudo_id, CAST((SELECT value.int_value FROM UNNEST (event_params) WHERE key = 'ga_session_id') AS STRING)))
      ELSE NULL
    END) AS purchase_sessions,
    -- Tính revenue
    SUM(ecommerce.purchase_revenue) AS revenue,
    -- Tính transactions
    COUNT(DISTINCT ecommerce.transaction_id) AS transactions,
    -- Tính basket_qty
    SUM(CASE 
      WHEN event_name = 'purchase' THEN (SELECT SUM(quantity) FROM UNNEST(items))
      ELSE NULL
    END) AS basket_qty
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  GROUP BY FORMAT_DATE('%Y-%m-%d', PARSE_DATE('%Y%m%d', event_date))
)

SELECT
  event_date,
  sessions,
  ROUND(SAFE_DIVIDE(view_item_sessions, sessions), 4) AS view_product_rate,
  ROUND(SAFE_DIVIDE(cart_sessions, view_item_sessions), 4) AS view_to_cart_rate,
  ROUND(SAFE_DIVIDE (purchase_sessions, cart_sessions), 4) AS cart_to_purchase_rate,
  ROUND(SAFE_DIVIDE(purchase_sessions, sessions), 4) AS purchase_rate,
  purchase_sessions AS ecommerce_purchases,
  ROUND (revenue, 4) AS purchase_revenue,
  ROUND(SAFE_DIVIDE (revenue, transactions), 4) AS avg_purchase_revenue,
  ROUND(SAFE_DIVIDE(basket_qty, transactions), 4) AS avg_basket_size,
  ROUND(SAFE_DIVIDE (revenue, sessions), 4) AS revenue_per_user
FROM raw_data1
ORDER BY 1
LIMIT 5;
* Query results
<img width="633" height="124" alt="query1" src="https://github.com/user-attachments/assets/b43dcb47-a162-4f84-8813-2c7c835ff8f4" />

