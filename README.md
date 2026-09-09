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

---

# 📊 Dashboard

The final dashboard consists of four analytical tabs, providing a comprehensive view of business performance, user acquisition, behavior, and product trends.

[![View Dashboard](https://img.shields.io/badge/View_Dashboard-Looker_Studio-blue?style=for-the-badge&logo=looker)](https://datastudio.google.com/reporting/df6d3336-3e9c-4392-8a82-1099671e074d)

### 1️⃣ Executive Overview
- **Objective:** Evaluate the overall health of the e-commerce business.
  <img width="900" height="677" alt="image" src="https://github.com/user-attachments/assets/0014c9ee-b91b-4060-9948-9a4d6a651029" />
- **Data Insights:**
  - **Revenue Volatility:** A sharp revenue peak was observed in late November to early December, followed by a severe 3x plummet in January ($160.6K down to $57.4K).
  - **Severe Funnel Leakage:** The overall purchase rate sits critically low at ~1.3%. The most significant drop-off occurs at the very beginning of the journey: **79% of sessions end before a single product is viewed**, and only 4% of total sessions result in an "Add to Cart" action.

### 2️⃣ Acquisition & Revenue Drivers
- **Objective:** Identify which channels and traffic sources bring the most valuable users.
<img width="899" height="672" alt="image" src="https://github.com/user-attachments/assets/8775ddcc-968e-44e2-a5a1-6492388a7c81" />

- **Data Insights:**
  - **Volume vs. Value:** `google` (organic) dominates traffic volume with over 112K sessions, making up ~28.9% of total revenue. However, it converts at a lower rate (1.09%).
  - **High-Intent Channels:** `(direct)` and referral traffic (e.g., `shop.googlemerchandisestore...`) rival Google in driving successful conversions, boasting higher conversion rates of 1.29% and 2.03% respectively. 

### 3️⃣ User Behavior & Audience
- **Objective:** Understand demographic profiles and platform preferences.
<img width="893" height="662" alt="image" src="https://github.com/user-attachments/assets/6c14aec6-1a4b-4787-ae56-95da994bc0e0" />

- **Data Insights:**
  - **Demographics:** The United States is overwhelmingly the primary source of active users (118.4K).
  - **Tech Stack Preference:** `desktop` captures the majority of the audience (57.9%), followed by `mobile` (39.8%). Web/Chrome is the dominant platform/browser combination, far exceeding iOS/Safari.
  - **Engagement:** While `page_view` volume is massive (1.4M), actual `view_item` events are significantly lower (386K), reinforcing the bottleneck found in the Executive Overview.

### 4️⃣ Product & Conversion Insights
- **Objective:** Analyze catalog performance and pinpoint where product interest fails to translate into sales.
<img width="894" height="679" alt="image" src="https://github.com/user-attachments/assets/b9419c0e-fcf4-4dd2-8aeb-e49057f50397" />

- **Data Insights:**
  - **Category Monopoly:** The "Apparel" category significantly outperforms all others, generating $171.7K (nearly 50% of the total product revenue), dwarfing the next best categories ("New" and "Bags").
  - **Visibility vs. Purchase Mismatch:** High product visibility does not consistently guarantee purchases. The scatter plot and bar charts reveal that several highly-viewed items yield almost zero purchases, resulting in a staggering 99.42% product-level drop-off rate.

---

# 💡 Key Insights & Recommendations

Based on the dashboard findings, the business exhibits strong top-of-funnel traffic but struggles with severe friction in product discovery and checkout conversion. 

**1. Fix the Top-of-Funnel Bottleneck (The 79% Drop-off)**
- **Insight:** Nearly 8 out of 10 users leave the site without viewing a single product. 
- **Recommendation:** Revamp the homepage and landing page UX. Implement clearer calls-to-action (CTAs), personalized product recommendations above the fold, and simplify site navigation to guide users directly to product catalog pages.

**2. Shift Focus from Traffic Volume to Traffic Quality**
- **Insight:** Google Organic brings the most users, but Direct and Referral channels bring the most *buyers*.
- **Recommendation:** Reallocate a portion of the marketing budget towards nurturing high-intent audiences. Implement email retention campaigns for Direct users and strengthen affiliate/referral partnerships. For Google Organic, audit the SEO strategy to ensure keywords are attracting purchase-intent users rather than informational browsers.

**3. Address the Mobile Experience**
- **Insight:** Desktop traffic dominates at 58%. In modern e-commerce, mobile usually leads. A lower mobile share paired with low overall conversion suggests a clunky mobile experience.
- **Recommendation:** Conduct a rigorous technical and UX audit specifically for mobile devices (iOS/Safari and Android). Ensure the "Add to Cart" and checkout flows are frictionless on smaller screens.

**4. Capitalize on "Apparel" and Audit High-Visibility Losers**
- **Insight:** Apparel is the undeniable cash cow, yet overall product drop-off is 99.42% due to items that get viewed but never bought.
- **Recommendation:** 
  - **Exploit:** Prominently feature the best-selling Apparel items on high-traffic landing pages to capitalize on proven demand.
  - **Explore:** Investigate the high-view/zero-purchase products. Audit their pricing, sizing availability, product imagery, and reviews to identify why users are losing interest after clicking.

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
├── dashboard.pdf
