### Fact Table: Product
- **Grain:** 1 row / event × item[cite: 1].
- **Purpose:** Phân tích hiệu quả sản phẩm qua product funnel và product revenue[cite: 1].
- **Mapping:** Tab 04 - Product & Conversion Insights[cite: 1].

* SQL code

```sql
CREATE TABLE `k51-dac-sql.GA4_ANALYSIS.fact_product` AS
SELECT
  FORMAT_DATE('%Y-%m-%d', PARSE_DATE('%Y%m%d', event_date)) AS event_date,
  user_pseudo_id,
  event_name,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS ga_session_id,
  CONCAT(
    user_pseudo_id, '-',
    CAST((SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS STRING)
  ) AS session_key,
  item.item_id,
  item.item_name,
  item.item_category,
  item.price,
  item.quantity,
  CASE WHEN event_name = 'view_item' THEN 1 ELSE 0 END AS view_item_count,
  CASE WHEN event_name = 'add_to_cart' THEN 1 ELSE 0 END AS add_to_cart_count,
  CASE WHEN event_name = 'begin_checkout' THEN 1 ELSE 0 END AS begin_checkout_count,
  CASE WHEN event_name = 'purchase' THEN 1 ELSE 0 END AS purchase_count,
  CASE
    WHEN event_name = 'purchase' THEN item.price * item.quantity
    ELSE 0
  END AS purchase_revenue
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`,
UNNEST(items) AS item
WHERE user_pseudo_id IS NOT NULL;
