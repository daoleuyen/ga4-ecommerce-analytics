### Fact Table: Session
- **Grain:** 1 row / session[cite: 1].
- **Purpose:** Tập trung vào traffic → funnel → purchase → revenue ở cấp phiên[cite: 1].
- **Mapping:** Tab 01 - Executive Overview; Tab 02 - Acquisition & Revenue Drivers[cite: 1].

* SQL code

```sql
CREATE TABLE `k51-dac-sql.GA4_ANALYSIS.fact_session` AS
WITH raw_data AS (
  SELECT
    FORMAT_DATE('%Y-%m-%d', PARSE_DATE('%Y%m%d', event_date)) AS event_date,
    user_pseudo_id,
    CONCAT(
      user_pseudo_id, '-',
      CAST((SELECT value.int_value FROM UNNEST (event_params) WHERE key='ga_session_id') AS STRING)
    ) AS session_key,
    traffic_source.source AS session_source,
    traffic_source.medium AS session_medium,
    device.category AS device_category,
    CASE
      WHEN (SELECT value.string_value FROM UNNEST (event_params) WHERE key='session_engaged')='1' THEN 1 
      ELSE 0
    END AS is_engaged_session,
    event_name,
    ecommerce.transaction_id AS transaction_id,
    ecommerce.purchase_revenue AS purchase_revenue,
    CASE
      WHEN event_name = 'purchase' THEN (SELECT SUM(quantity) FROM UNNEST(items))
      ELSE 0
    END AS basket_qty
  FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
  WHERE user_pseudo_id IS NOT NULL
),
session_data AS (
  SELECT
    MIN(event_date) AS event_date,
    session_key,
    MAX(user_pseudo_id) AS user_pseudo_id,
    MAX(session_source) AS session_source,
    MAX(session_medium) AS session_medium,
    MAX(device_category) AS device_category,
    MAX(is_engaged_session) AS is_engaged_session,
    MAX(CASE WHEN event_name = 'view_item' THEN 1 ELSE 0 END) AS is_view_item,
    MAX(CASE WHEN event_name = 'add_to_cart' THEN 1 ELSE 0 END) AS is_add_to_cart,
    MAX(CASE WHEN event_name = 'begin_checkout' THEN 1 ELSE 0 END) AS is_begin_checkout,
    MAX(CASE WHEN event_name = 'add_shipping_info' THEN 1 ELSE 0 END) AS is_add_shipping,
    MAX(CASE WHEN event_name = 'add_payment_info' THEN 1 ELSE 0 END) AS is_add_payment,
    MAX(CASE WHEN event_name = 'purchase' THEN 1 ELSE 0 END) AS is_purchase,
    COUNT(DISTINCT transaction_id) AS transaction_count,
    SUM(purchase_revenue) AS purchase_revenue,
    SUM(basket_qty) AS basket_qty,
    1 AS session_count
  FROM raw_data
  WHERE session_key IS NOT NULL
  GROUP BY session_key
)
SELECT * FROM session_data;
