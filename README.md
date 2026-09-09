# 📊 GA4 E-commerce Analytics | SQL & Looker Studio

> **From Raw Event Data → 10 SQL Queries → 3 Fact Tables → Dashboard → Business Insights**

<!-- Add project cover image here -->
<!-- ![Project Cover](assets/project_cover.png) -->

---

## 📌 Overview

This project analyzes e-commerce performance and user behavior using the **Google Analytics 4 (GA4) public sample dataset** in BigQuery.

The project follows an end-to-end analytics workflow:

**Raw GA4 Event Data → Exploratory Analysis → Data Modeling → Dashboard → Insights & Recommendations**

### 🎯 Key Business Questions

1. How is the e-commerce business performing overall?
2. Which channels drive traffic, conversion, and revenue?
3. Who are the users and how do they interact with the website?
4. Which products perform best, and where do users drop off in the conversion journey?

---

## 📂 Dataset

- **Source:** Google Analytics 4 Public Sample Dataset
- **Dataset:** `bigquery-public-data.ga4_obfuscated_sample_ecommerce`
- **Main Table:** `events_*`
- **Platform:** Google BigQuery
- **Data Type:** Event-based data

GA4 data contains nested and repeated fields such as:

- `event_params`
- `items`
- `device`
- `geo`
- `traffic_source`
- `ecommerce`

Key SQL techniques used include `UNNEST()`, CTEs, conditional aggregation, window functions, and `SAFE_DIVIDE()`.

---

# 🔎 Exploratory Analysis: 10 SQL Queries

The dataset was explored through 10 business-driven queries:

| # | Business Question |
|---|---|
| 01 | Overall e-commerce performance and key metrics |
| 02 | Website event analysis |
| 03 | Traffic source and medium performance |
| 04 | Acquisition performance across the funnel |
| 05 | Active users by device |
| 06 | Active users by country |
| 07 | Users by operating system and browser |
| 08 | Top-performing pages |
| 09 | Product funnel and revenue performance |
| 10 | Checkout funnel performance by device |

📁 **[View SQL Queries](./sql/)**

---

# 🏗️ Data Modeling: 3 Fact Tables

Instead of connecting individual exploratory queries directly to the dashboard, the analysis was consolidated into three reusable fact tables.

| Fact Table | Grain | Purpose |
|---|---|---|
| `fact_session` | 1 row / session | Traffic, funnel, purchase, revenue |
| `fact_user_behavior` | 1 row / event | User behavior, device, geography, pages |
| `fact_product` | 1 row / event × item | Product funnel and revenue |

### Data Flow

```text
Raw GA4 Event Data
        ↓
10 Business Queries
        ↓
┌───────────────────────────────────┐
│ fact_session                      │
│ fact_user_behavior                │
│ fact_product                      │
└───────────────────────────────────┘
        ↓
Looker Studio Dashboard
        ↓
Insights & Recommendations
---
```

# 📊 Dashboard

The final dashboard consists of four analytical tabs.

### 1️⃣ Executive Overview
- **Question:** How is the business performing overall?
- **Metrics:** Sessions, Revenue, Purchases, Purchase Rate, AOV, Revenue Trend, E-commerce Funnel.

### 2️⃣ Acquisition & Revenue Drivers
- **Question:** Which channels and factors drive traffic, conversion, and revenue?
- **Metrics:** Source / Medium, Sessions, Purchases, Conversion Rate, Revenue, Revenue per Session, Checkout Funnel by Device.

### 3️⃣ User Behavior & Audience
- **Question:** Who are the users and how do they interact with the website?
- **Metrics:** Active Users, Total Events, Device, Country, OS / Browser, Top Events, Top Pages.

### 4️⃣ Product & Conversion Insights
- **Question:** Which products perform best and where does conversion drop off?
- **Metrics:** Product Revenue, Product Views, Add-to-Carts, Checkouts, Purchases, Product Funnel.

---

# 💡 Key Insights & Recommendations

Based on the dashboard findings, the analysis focuses on identifying opportunities to:

- Optimize high-traffic but low-converting channels, pages, or products.
- Prioritize acquisition channels with stronger conversion and revenue performance.
- Identify major drop-off points in the e-commerce funnel.
- Improve products with high user interest but weak purchase conversion.

*Note: Insights and recommendations are based on the final dashboard analysis and should be interpreted within the context of the selected GA4 sample dataset.*

---

# 🛠️ Tools & Skills

- **Tools:** BigQuery · SQL · Looker Studio · GitHub
- **Skills:**
  - SQL & Data Exploration
  - Nested & Repeated Data (`UNNEST`)
  - CTEs & Window Functions
  - Funnel Analysis
  - E-commerce Analytics
  - Data Modeling & Fact Table Design
  - Dashboard Development
  - Business Insight Generation

---

# 📁 Project Structure

```text
ga4-ecommerce-analytics/
│
├── README.md
├── sql/
│   ├── explore/
│   │   ├── 01_overview_metrics.md
│   │   ├── ...
│   │   └── 10_checkout_funnel_device.md
│   │
│   └── fact_table/
│       ├── fact_session.md
│       ├── fact_user_behavior.md
│       └── fact_product.md
│
├── dashboard/
│   ├── executive_overview.png
│   ├── acquisition_revenue.png
│   ├── user_behavior.png
│   └── product_conversion.png
