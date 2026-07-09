<div align="center">

<!-- HEADER BANNER -->
<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0D2B4E&height=200&section=header&text=DataCo%20Supply%20Chain%20Intelligence&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Turning%20180%2C519%20orders%20into%20actionable%20supply%20chain%20strategy&descAlignY=58&descColor=85B7EB" />

<br/>

![Status](https://img.shields.io/badge/Status-✅%20Completed-1D9E75?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.11-378ADD?style=for-the-badge&logo=python&logoColor=white)
![PowerBI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-12%20Queries-0D2B4E?style=for-the-badge&logo=sqlite&logoColor=white)
![Records](https://img.shields.io/badge/Records-180K+-EF9F27?style=for-the-badge)
![Markets](https://img.shields.io/badge/Markets-5%20Global-E24B4A?style=for-the-badge)

<br/>

> **"A company shipping $32.39M worth of goods globally — yet 56% of deliveries arrive late and $20.60M of revenue sits at risk. This project finds out why."**

<br/>

</div>

---

## 📋 Table of Contents

| # | Section |
|---|---|
| 01 | [🎯 Problem Statement](#-problem-statement) |
| 02 | [📦 Dataset](#-dataset) |
| 03 | [🛠️ Tools & Technologies](#️-tools--technologies) |
| 04 | [🔬 Methods & Approach](#-methods--approach) |
| 05 | [💡 Key Insights](#-key-insights) |
| 06 | [📊 Dashboard](#-dashboard) |
| 07 | [⚙️ How to Run](#️-how-to-run) |
| 08 | [📈 Results & Conclusion](#-results--conclusion) |
| 09 | [🔭 Future Work](#-future-work) |
| 10 | [👤 Author & Contact](#-author--contact) |

---

## 🎯 Problem Statement

<img align="right" width="340" src="https://img.shields.io/badge/-Supply%20Chain%20Challenge-E24B4A?style=flat-square" />

> **How can a global supply chain company reduce its late delivery rate, identify revenue at risk, and improve profitability — using data it already has?**

DataCo Global operates across **5 international markets** — Europe, LATAM, Pacific Asia, USCA, and Africa — processing over **180,000 orders** across hundreds of products. Despite the scale, the business faced three critical unsolved questions:

```
❓ Why are more than half of all deliveries arriving late?
❓ Which markets, products, and shipping modes are bleeding the most revenue?
❓ Where are fraud orders concentrated and how can they be caught earlier?
```

The existing system was also **silently misreporting** the late delivery rate at ~40% due to 4,423 broken risk flags in the data — meaning the true scale of the problem was hidden from decision-makers.

This project rebuilds the analysis from the ground up: correcting the data quality issues, running 12 deep SQL queries, applying inventory science (EOQ, Safety Stock, ABC classification), and delivering a **5-page interactive Power BI dashboard** that turns raw order data into decisions.

---

## 📦 Dataset

<div align="center">

| Property | Detail |
|:---|:---|
| 📛 **Name** | DataCo Smart Supply Chain for Big Data Analysis |
| 🔗 **Source** | [Mendeley Data — Click to Download](https://data.mendeley.com/datasets/8gx2fvg2k6/5) |
| 📏 **Size** | 180,519 rows × 53 columns |
| 📅 **Period** | January 2015 — January 2016 |
| 🌍 **Markets** | Europe · LATAM · Pacific Asia · USCA · Africa |
| 👥 **Customers** | United States & Puerto Rico only |
| 🧹 **Final columns** | 30 (after removing duplicates, PII, and zero-value columns) |

</div>

### What the data contains

```
📦 Order data        →  Order ID, status, date, quantity, shipping mode
💰 Financial data    →  Sales, discounts, profit, item price, profit ratio  
🌍 Geography         →  Market, region, country, city, latitude, longitude
🚚 Logistics         →  Scheduled vs actual shipping days, delivery status, delay
🛒 Product data      →  Product name, category, department, unit price
👤 Customer data     →  Segment (Consumer / Corporate / Home Office), city
```

### Data quality issues found & fixed

| Issue | Impact | Fix Applied |
|---|---|---|
| 4,423 broken `Late_delivery_risk` flags | System reported 40% late — true rate is **54.8%** | Recalculated from actual shipping vs scheduled dates |
| `PENDING_PAYMENT` duplicating `PENDING` | 9 status values instead of 7 | Merged — confirmed identical financial profile |
| `benefit_per_order` = `order_profit_per_order` | Misleading duplicate column | Dropped — 100% correlation confirmed |
| `sales_per_customer` = `order_item_total` | Misleading duplicate column | Dropped — 100% correlation confirmed |
| Cancelled orders showing full revenue | Revenue inflated by cancelled sales | Zeroed revenue, applied −8% cancellation handling cost as negative profit |
| Floats with 6+ decimal places | Unpresentable in Power BI | All floats rounded to 2 decimal places |
| Date columns stored as plain strings | No time-series analysis possible | Parsed to `datetime`, extracted year/month/quarter/day-of-week |

---

## 🛠️ Tools & Technologies

<div align="center">

| Layer | Tool | Purpose |
|:---|:---|:---|
| 🐍 **Language** | Python 3.11 | Core analysis pipeline |
| 📊 **Data** | Pandas · NumPy | Data cleaning, feature engineering, transformation |
| 📈 **Visualization** | Matplotlib · Seaborn | Exploratory charts in notebook |
| 🗄️ **Analytics** | SQLite (in-memory) | 12 business intelligence queries |
| 📉 **Dashboard** | Power BI Desktop | 5-page interactive executive dashboard |
| 🎨 **Theme** | Custom JSON theme | Ocean Command palette (navy + teal) |
| 📓 **Notebook** | Jupyter Notebook | Reproducible end-to-end pipeline |
| 🐙 **Version Control** | Git + GitHub | Project hosting and portfolio |

</div>

---

## 🔬 Methods & Approach

The project follows an **8-stage analytical pipeline** — each stage builds on the previous one.

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  RAW DATA  →  CLEAN  →  ENGINEER  →  AUDIT  →  SQL  →  INVENTORY  →  EXPORT  →  DASHBOARD │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Stage 1 · Data Cleaning**
Selected 30 of 53 columns — removing PII (email, password, street), duplicate columns, and columns with >80% missing values. Kept geographic columns (Latitude, Longitude, Order Region) for Power BI map visuals.

**Stage 2 · Feature Engineering**
Created `shipping_delay_days` (actual − scheduled), extracted order year/month/quarter/day-of-week, flagged fraud and cancelled orders as binary columns, computed `revenue_at_risk` (sales on delayed orders).

**Stage 3 · Data Quality Audit & Correction**
Identified and corrected the 4,423 broken risk flags. Merged `PENDING_PAYMENT` into `PENDING`. Applied business logic to cancelled orders: revenue → $0, profit → negative (−8% handling cost). Rounded all floats to 2dp.

**Stage 4 · SQL Analysis (12 queries)**

| Query | Business Question |
|---|---|
| Market Revenue | Which market generates the most revenue and profit? |
| Top Categories | Which product categories drive the most sales? |
| Segment Profitability | Which customer segment is most profitable? |
| Payment Method Split | How does revenue break down by payment type? |
| Fraud by Market × Segment | Where is fraud concentrated? |
| Shipping Mode Delays | Which shipping mode fails most often? |
| Late % by Order Region | Which specific regions have the worst delivery performance? |
| Product Revenue at Risk | Which products have the most revenue sitting in delayed orders? |
| Day-of-Week Demand | Which days drive the most orders? |
| Global Seasonality | How does demand change month by month? |
| All-Market Quarterly Trend | How is each market trending quarter by quarter? |
| Cash Flow Waterfall | What is the journey from gross revenue to net profit? |

**Stage 5 · Inventory Science**
- **Safety Stock** — calculated for all high-volume products using: `(Max Daily Demand × Max Lead Time) − (Avg Daily Demand × Avg Lead Time)`
- **EOQ** — Economic Order Quantity calculated in SQL using `SQRT(2 × Annual Demand × Order Cost / Holding Cost)`
- **ABC Classification** — Class A/B/C assigned using **data-driven percentile thresholds** (80th/40th percentile) — not hardcoded values

**Stage 6 · Power BI Dashboard**
5-page dashboard built on the cleaned export. Custom Ocean Command theme (navy `#0D2B4E` + teal `#1D9E75`). F-pattern layout, KPI cards on every page, conditional formatting, slicers on all pages, drill-down date hierarchy.

---

## 💡 Key Insights

### 💰 Revenue & Profitability

<table>
<tr>
<td>

```
Total Revenue      →   $32.39M
Total Profit       →   $3.83M
Profit Margin      →   11.8%
Total Discounts    →   $3.65M given away
Revenue at Risk    →   $20.60M (delayed orders)
```

</td>
<td>

- 🏆 **Europe** generates the highest revenue ($9.6M)
- 📉 **Africa** generates the lowest ($2.0M)
- 🎣 **Fishing** is the top category by both revenue ($6.1M) and profit ($0.73M)
- 🏢 **Corporate segment** has the best profit ratio (32%)
- 🛍️ **Consumer segment** gets the most discounts ($1.9M) but has the lowest margin

</td>
</tr>
</table>

### 🚚 Logistics & Delivery

```
⚠️  Late Delivery Rate      →  56.11%  (system was reporting 40% — 4,423 flags were wrong)
🔴  Worst Shipping Mode      →  First Class — 95.12% of orders arrive late
✅  Best Shipping Mode       →  Standard Class (most used, best balance)
📦  Revenue at Risk          →  $20.60M sitting in delayed shipments
🌍  Root Cause               →  Products shipped internationally — long cross-border transit
👥  Customer Countries       →  Only United States and Puerto Rico
```

> **Paradox finding:** First Class shipping, which customers pay a premium for, has the **highest late delivery rate in the entire dataset at 95.12%.**

### 📅 Seasonal & Market Trends

| Market | Trend |
|---|---|
| 🇪🇺 Europe | Strongest in Q3, consistent year-round |
| 🌎 LATAM | Orders **only in Q1 and Q2** — zero in second half of year |
| 🌍 Africa | No orders at all in Q2 |
| 🌏 Pacific Asia | Significant dip in Q2 and Q3 |
| 🇺🇸 USCA | Most stable throughout the year |
| 📅 Best Month | **January** has the highest revenue |
| 📉 Weakest Quarter | Q4 performs lowest overall — Oct/Nov/Dec orders drop |

### 🔍 Fraud Intelligence

```
🚨  Total fraud cases     →   4,062 orders
💳  Payment method        →   100% via TRANSFER — Debit, Cash, Card show ZERO fraud  
🏙️  Top fraud city        →   Caguas (1,583 cases out of 4,062 total)
🛡️  Protected categories  →   Laptops, machinery — zero fraud (likely have stricter purchase policies)
📊  Fraud rate            →   2.25% of all orders
```

---

## 📊 Dashboard

> **5-page interactive Power BI dashboard — built with the Ocean Command theme**

### Page 1 — Executive Overview
KPI cards (revenue, profit, late rate, discounts), revenue by market, order status donut, top 5 categories. The **landing page** for any stakeholder — answers "how are we doing?" in 5 seconds.

### Page 2 — Logistics & Delivery  
100% stacked bar (delay rate by shipping mode), late delivery by region, product revenue-at-risk table. **Most actionable page** — tells exactly which mode and products to fix first.

### Page 3 — Inventory Intelligence  
ABC class analysis, slow-moving product velocity, EOQ and safety stock reference table. Designed for **supply planners and procurement teams**.

### Page 4 — Revenue & Profit  
Revenue at risk gauge, discount drain by segment, profit by category, revenue by shipping mode. Built for **finance and commercial teams**.

### Page 5 — Market Trends  
Quarterly market trend lines, monthly revenue seasonality, fraud city breakdown. Built for **strategy and regional teams**.

> 📎 Dashboard file: `powerbi/Dataco_supply.pbix`  
> 🎨 Theme file: `powerbi/DataCo_OceanCommand_Theme.json`

---

## ⚙️ How to Run

### Prerequisites
```bash
Python 3.8+
Jupyter Notebook
Power BI Desktop (free — download from microsoft.com/powerbi)
```

### Step 1 — Clone the repository
```bash
git clone https://github.com/yourusername/dataco-supply-chain.git
cd dataco-supply-chain
```

### Step 2 — Install dependencies
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Step 3 — Download the dataset
Download `DataCoSupplyChainDataset.csv` from [Mendeley Data](https://data.mendeley.com/datasets/8gx2fvg2k6/5) and place it in the project root.

### Step 4 — Run the notebook
```bash
jupyter notebook DataCo_SupplyChain_Analysis.ipynb
```
Run all cells from top to bottom. The final cell automatically exports `dataco_powerbi_master.csv`.

### Step 5 — Open the dashboard
```
1. Open Power BI Desktop
2. File → Open → select powerbi/Dataco_supply.pbix
3. If prompted, update data source path to dataco_powerbi_master.csv
4. Click Refresh — all visuals update automatically
```

### Step 6 — Apply the theme (optional)
```
Power BI Desktop → View → Themes → Browse for themes
→ Select powerbi/DataCo_OceanCommand_Theme.json → Apply
```

---

## 📈 Results & Conclusion

This project successfully achieved all three original goals:

### ✅ Goal 1 — Identify why deliveries are late
**Finding:** The root cause is international shipping distances — products are shipped globally but customers are overwhelmingly in the United States and Puerto Rico. First Class shipping, despite its premium pricing, performs worst at **95.12% late rate**, suggesting the mode categorisation does not reflect actual speed.

**Actionable recommendation:** Audit the First Class shipping contracts. Consider switching high-priority orders to Same Day (18% late rate) for US domestic deliveries.

### ✅ Goal 2 — Quantify revenue at risk  
**Finding:** $20.60M of the $32.39M total revenue (63.6%) is associated with delayed orders. The top 3 products alone account for $3.5M in at-risk revenue.

**Actionable recommendation:** Prioritise safety stock buffers for Field & Stream Sportsman and Perfect Fitness Rip Deck — the two products with the highest combined delay frequency and revenue at risk.

### ✅ Goal 3 — Fraud pattern identification  
**Finding:** All 4,062 fraud orders (100%) used the **Transfer payment method**. Caguas, Puerto Rico accounts for 39% of all fraud cases alone. High-value product categories show zero fraud — suggesting existing controls work for expensive items but not mid-range products.

**Actionable recommendation:** Flag all Transfer-method orders from Caguas for manual review. Extend the fraud controls currently applied to high-value products to mid-range categories.

### Overall impact summary

```
📊  Data quality improved    →  4,423 wrong flags corrected; 2 duplicate columns removed
🚨  True late rate revealed  →  54.8% (not 40% as the system reported)
💰  Revenue risk quantified  →  $20.60M identified and attributed to root causes
🔍  Fraud mapped precisely   →  100% Transfer payments; 39% from single city
📦  Inventory optimised      →  EOQ and safety stock calculated for all high-volume SKUs
```

---

## 🔭 Future Work

| Area | Enhancement |
|---|---|
| 🤖 **Machine Learning** | Build a late delivery prediction model (Logistic Regression / Random Forest) using shipping mode, region, product category, and day-of-week as features |
| 📡 **Real-time pipeline** | Connect Power BI to a live database instead of CSV export — enable live dashboard refresh |
| 🗺️ **Geospatial analysis** | Use Latitude/Longitude columns for delivery heat maps — identify geographic clusters of delay |
| 🔮 **Demand forecasting** | Apply time series forecasting (Prophet / ARIMA) to predict Q4 demand dips and plan inventory accordingly |
| 💬 **NLP on customer data** | Analyse order patterns in fraud cities using clustering to build an early-warning fraud scoring system |
| 📊 **Unit economics** | Calculate true cost-per-delivery by shipping mode (including delay cost) to find the most cost-efficient mode per market |

---

## 👤 Author & Contact

<div align="center">

<br/>

**Kumail**  
*Data Analyst · Python · SQL · Power BI*

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/yourprofile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yourusername)
[![Email](https://img.shields.io/badge/Email-Contact-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:your.email@example.com)

<br/>

*If this project helped you or gave you ideas — a ⭐ star means a lot!*

<br/>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0D2B4E&height=100&section=footer" />

</div>
