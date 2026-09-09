### Query 09: For each product, how many views, add-to-cart actions, checkouts, units purchased, and how much revenue did it generate?

* SQL code

```sql
SELECT
  item.item_name AS item_name,
  COUNTIF(event_name = 'view_item') AS views,
  COUNTIF(event_name = 'add_to_cart') AS added_to_cart,
  COUNTIF(event_name = 'begin_checkout') AS checked_out,
  COUNTIF(event_name = 'purchase') AS items_purchased,
  SUM(CASE WHEN event_name = 'purchase' THEN item.price * item.quantity ELSE NULL END) AS item_purchase
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`,
UNNEST(items) AS item
GROUP BY 1
ORDER BY 2 DESC;
```
* Query results
  <img width="564" height="125" alt="{13A31975-B148-42F1-A7C0-E168AC4BC253}" src="https://github.com/user-attachments/assets/86e8ce0a-752e-421c-bdb5-618c8df78f6b" />
