<div align="center">

# 📊 Superstore Sales Analysis — Power BI Dashboard

### End-to-end retail sales, profitability & customer intelligence dashboard built on the Sample Superstore dataset

[![Tool](https://img.shields.io/badge/Tool-Power%20BI%20Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-50%2B%20Measures-217346?style=for-the-badge&logo=microsoft&logoColor=white)](#-dax-measures)
[![Power Query](https://img.shields.io/badge/Power%20Query-M%20Language-1E7145?style=for-the-badge)](#-etl--data-transformation)
[![Data Model](https://img.shields.io/badge/Data%20Model-Star%20Schema-blue?style=for-the-badge)](#-data-model)
[![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)](#)

<br>

<img src="images/live_dashboard.gif" alt="Dashboard Preview" width="100%">

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Dashboard Preview](#️-dashboard-preview)
- [Data Model](#️-data-model)
- [DAX Measures](#-dax-measures)
- [ETL / Data Transformation](#-etl--data-transformation)
- [Tech Stack](#️-tech-stack)
- [Repository Structure](#-repository-structure)
- [How to Use](#-how-to-use)
- [Business Problem & Insights](#-business-problem--insights)
- [Key Insights (Summary)](#-key-insights-summary)
- [Root Cause Analysis](#-root-cause-analysis)
- [Actionable Recommendations](#-actionable-recommendations)
- [Author](#-author)
- [License](#-license)
---

## 📌 Overview

This project is a **6-page interactive Power BI dashboard** built on the classic **Sample Superstore** dataset (2014–2017, US retail operations). It transforms 9,994 raw transaction records into a decision-ready analytics suite covering **sales performance, profitability, customer behavior, product trends, and shipping operations**.

The goal was to go beyond static charts and build a dashboard that *feels like a product* — dynamic titles that respond to filters, contextual tooltip pages, auto-generated insight callouts, KPI cards with YoY/MoM trends, and a clean, consistent visual design system across every page.

| | |
|---|---|
| 🗂️ **Records analyzed** | 9,994 transactions \| 5,009 orders |
| 💰 **Total Sales** | ~$2.30M |
| 📈 **Total Profit** | ~$286K |
| 👥 **Customers** | 793 unique customers |
| 📦 **Products** | 1,862 unique SKUs across 3 categories / 17 sub-categories |
| 🌎 **Coverage** | 49 states \| 531 cities \| 4 US regions |
| 📅 **Time Range** | Jan 2014 – Jan 2018 |

---

## ✨ Key Features

- **6 fully designed report pages** + 3 dedicated tooltip pages (report-page tooltips on hover)
- **50+ custom DAX measures** — core KPIs, time intelligence (YTD/QTD/MTD, YoY%, MoM%, rolling averages), dynamic titles, and Pareto (80/20) customer analysis
- **Dynamic page titles & subtitles** that update automatically based on slicer/region selection
- **Auto-generated insight callouts** (e.g., top-performing region/category) written entirely in DAX
- **Drill-through & tooltip pages** for Category, State, and Region-level context on hover
- **Consistent design system** — 1280×720 canvas, custom KPI cards, icon-driven navigation buttons across all pages
- **Clean ETL pipeline** in Power Query — type casting, locale-aware date parsing, text trimming/cleaning, and derived columns (Delivery Days, On-Time/Late Flag, Discount Band, Customer Type)

---

## 🖥️ Dashboard Preview

### 1️⃣ Home
Landing page with headline KPIs (Total Sales, Profit, Orders), a **Sales by Region** column chart, **Sales vs Profit Trend** line chart, **Sales by Category** donut, and navigation buttons into every other page.

<img src="images/01-home.png" alt="Home Page" width="100%">

### 2️⃣ Sales Overview
Deep dive into revenue performance: **Sales Trend (Seasonal)**, **Sales by Category**, **Sales by Segment**, a **Sales by State** map, and a **Top 10 Products** table — filterable by Region, Segment, Ship Mode  and Category slicers.

<img src="images/02-sales-performance.png" alt="Sales Overview Page" width="100%">

### 3️⃣ Profitability
Margin and loss analysis: **Profit by Region**, **Profit by Segment**, **Top 10 States by Profit**, a **Loss-Making Products** table, and a **Profit by Ship Mode** combo chart.

<img src="images/03-profit-performance.png" alt="Profitability Page" width="100%">

### 4️⃣ Customer Insights
Customer segmentation and value analysis: **Segment Split**, **Top 10 / Bottom 10 Customers by Sales**, **New vs Returning Customers**, a **Customer Details** table, and Pareto-based Top 20% customer contribution metrics.

<img src="images/04-customer-performance.png" alt="Customer Insights Page" width="100%">

### 5️⃣ Product Deep Dive
Product-level performance: **Top 10 Products by Sales**, **Top 10 Products by Profit**, **Quantity Sold by Sub-Category**, a **Product Detail** table, and **Avg Profit Margin by Category**.

<img src="images/05-product-analysis.png" alt="Product Deep Dive Page" width="100%">

### 6️⃣ Shipping & Operations
Logistics and fulfillment analysis: **Orders by Ship Mode**, **Avg Delivery Days by Ship Mode**, **Delivery Time Trend**, an **Order Detail** table, and **On-Time Delivery Rate by Ship Mode**.

<img src="images/06-shipping-analysis.png" alt="Shipping & Operations Page" width="100%">

---

## 🗃️ Data Model

The model follows a **star-schema** approach with a central fact table (Superstore data table) and supporting dimension/measure table (Date table).

<img src="images/data-model.png" alt="Power BI Data Model View" width="100%">

| Table | Type | Description |
|---|---|---|
| **SuperstoreData** | Fact | 9,994 rows × 27 columns — orders, products, customers, sales, profit, discount, shipping & delivery metrics |
| **DateMaster** | Dimension | Custom calendar table powering all time-intelligence measures (YTD, QTD, MTD, YoY, MoM) |
| **Measure** | Measure table | Dedicated (disconnected) table used purely to organize all 50+ DAX measures outside the fact table |

**Key derived columns** created in Power BI Using DAX:
- `DeliveryDays` — 'Ship Date − Order Date'
- `OnTime&LateThreshold` — Defines the expected delivery-day threshold per Ship Mode (Same Day, First Class, Second Class, Standard Class), used as the benchmark for the OnTime&LateFlag calculation
- `Discount_Band` — categorized discount tiers
- `Customer Type` — New vs. Returning classification

---

## 🧮 DAX Measures

A sample of the 50+ measures used across the report (organized by category):
(Click on triangles)

<details>
<summary><strong>📌 Core KPIs</strong></summary>

```dax
Total Sales = SUM(SuperstoreData[Sales])

Total Profit = SUM(SuperstoreData[Profit])

Total Orders = DISTINCTCOUNT(SuperstoreData[Order ID])

Total Customers = DISTINCTCOUNT(SuperstoreData[Customer ID])

ProfitMargin% = DIVIDE([Total Profit], [Total Sales], 0)

AvgOrderValue = DIVIDE([Total Sales], [Total Orders], 0)
```
</details>

<details>
<summary><strong>📅 Time Intelligence</strong></summary>

```dax
Sales YTD = TOTALYTD([Total Sales], DateMaster[Date])

SalesQTD = TOTALQTD([Total Sales], DateMaster[Date])

SaleMTD = TOTALMTD([Total Sales], DateMaster[Date])

TotalSales YoY% =
VAR __PREV_YEAR = CALCULATE([Total Sales], DATEADD('DateMaster'[Date], -1, YEAR))
RETURN DIVIDE([Total Sales] - __PREV_YEAR, __PREV_YEAR)

Sales Rolling 3Month Avg =
AVERAGEX(
    DATESINPERIOD(DateMaster[Date], MAX(DateMaster[Date]), -3, MONTH),
    [Total Sales]
)

Sales Running Total =
CALCULATE(
    [Total Sales],
    FILTER(ALL(DateMaster[Date]), DateMaster[Date] <= MAX(DateMaster[Date]))
)
```
</details>

<details>
<summary><strong>🎯 Dynamic Titles & Insight Callouts</strong></summary>

```dax
SalesOverviewDynamicTitle =
"SALES PERFORMANCE — " &
IF(ISFILTERED(SuperstoreData[Region]), SELECTEDVALUE(SuperstoreData[Region]) & " Region", "All Regions")

Insight 1 Text =
VAR TopRegion =
    CALCULATE(SELECTEDVALUE(SuperstoreData[Region]), TOPN(1, ALL(SuperstoreData[Region]), [Total Sales]))
VAR TopRegionSales = CALCULATE([Total Sales], SuperstoreData[Region] = TopRegion)
RETURN " 💡 " & TopRegion & " leads all regions with " & FORMAT(TopRegionSales, "$#,##0,") & "K in sales."
```
</details>

<details>
<summary><strong>👥 Customer Analytics</strong></summary>

```dax
RepeatCustomers% =
VAR CustWithMultiOrders =
    CALCULATE(
        DISTINCTCOUNT(SuperstoreData[Customer ID]),
        FILTER(VALUES(SuperstoreData[Customer ID]), CALCULATE(DISTINCTCOUNT(SuperstoreData[Order ID])) > 1)
    )
RETURN DIVIDE(CustWithMultiOrders, [Total Customers], 0)

Top20%Sales =
CALCULATE(
    [Total Sales],
    TOPN([Top20%CustomerCount], VALUES(SuperstoreData[Customer Name]), [Total Sales], DESC)
)

Top20%Sales% = DIVIDE([Top20%Sales], [Total Sales], 0)
```
</details>

<details>
<summary><strong>📦 Product & Shipping Analytics</strong></summary>

```dax
BestSellingProductName =
CALCULATE(SELECTEDVALUE(SuperstoreData[Product Name]), TOPN(1, ALL(SuperstoreData[Product Name]), [Total Sales]))

Loss_Making_Orders_Count =
CALCULATE(DISTINCTCOUNT(SuperstoreData[Order ID]), SuperstoreData[Profit] < 0)

OnTimeDeliveryRate = AVERAGE(SuperstoreData[OnTime&LateFlag])

MostUsedShipMode =
CALCULATE(
    SELECTEDVALUE(SuperstoreData[Ship Mode]),
    TOPN(1, VALUES(SuperstoreData[Ship Mode]), CALCULATE(DISTINCTCOUNT(SuperstoreData[Order ID])))
)
```
</details>

---

## 🔄 ETL / Data Transformation

Data is ingested from a CSV source and cleaned entirely in **Power Query (M)**:

- Type casting for all 21 source columns (dates, currency, integers, text)
- Locale-aware date parsing (`en-US`) for `Order Date` and `Ship Date`
- Text trimming & cleaning on `Customer Name`
- Currency formatting on `Sales` and `Profit`
- Derived fields: `DeliveryDays`, `OnTime&LateFlag`, `Discount_Band`, `Customer Type`

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Report authoring, data modeling, visualization |
| **DAX** | Measures, KPIs, time intelligence, dynamic text |
| **Power Query (M)** | Data cleaning & transformation (ETL) |
| **CSV (Sample Superstore)** | Source dataset |

---

## 📁 Repository Structure

```
Superstore-Sales-Analysis/
│
├── dashboard/
│   └── PowerBI-Superstore-Sales-Project.pbix    # Main Power BI report file
├── raw data/
│   └── Sample - Superstore.csv                  # Source dataset
├── images/
│   ├── 01-home.png
│   ├── 02-sales-performance.png
│   ├── 03-profit-performance.png
│   ├── 04-customer-performance.png
│   ├── 05-product-analysis.png
│   ├── 06-shipping-analysis.png
│   └── data-model.png
└── README.md
```

---

## 🚀 How to Use

1. Open `PowerBI-Superstore-Sales-Project.pbix` in **Power BI Desktop** (free download [here](https://powerbi.microsoft.com/desktop/))
2. If prompted, update the data source path under **Transform Data → Data Source Settings** to point to your local copy of `Sample - Superstore.csv`
3. Click **Refresh** to load the data, then explore the report pages via the in-report navigation buttons

---

## 🧩 Business Problem & Insights

> **Business question:** *"Our company has experienced strong sales growth but declining profitability. Analyze the data and identify the root causes, key issues, and actionable recommendations."*

### The Growth-vs-Profit Gap

Sales grew every single year, but profit **margin did not grow with it** — and actually reversed in the most recent year:

| Year | Sales | Profit | Margin % |
|---|---:|---:|---:|
| 2014 | $484,247 | $49,544 | 10.23% |
| 2015 | $470,533 | $61,619 | 13.10% |
| 2016 | $609,206 | $81,795 | **13.43%** ⬆️ |
| 2017 | $733,215 | $93,439 | **12.74%** ⬇️ |

Between 2016 and 2017, sales jumped **+20.4%**, but margin *contracted* by ~0.7 points — meaning profit grew more slowly than revenue. This is the exact "growth without profitability" pattern the business flagged, and the dashboard traces it to four compounding root causes.

---

## 📈 Key Insights (Summary)

- Sales grew every year, but **margin peaked in 2016 (13.4%) and declined in 2017 (12.7%)** despite a 20%+ sales increase — the core symptom behind the business problem
- **Discounts above 30% are margin-negative**, and discounts above 50% lose far more than the sale is worth (-119.2% margin)
- **The company gave away $322.58K in discounts** — more than the entire $286.82K in profit earned over the same period
- **Furniture — specifically Tables and Bookcases — is the only structurally unprofitable category**, dragging down otherwise healthy Technology and Office Supplies performance, despite receiving the highest average discount (17.4%) of any category
- **Texas, Ohio, Colorado, Illinois, and Pennsylvania** are the biggest loss contributors — Texas by sheer sales volume, Ohio and Colorado by consistently poor margins regardless of size
- **26.3% of all orders (1,318 of 5,009) are unprofitable**, indicating a systemic discounting/pricing issue rather than isolated bad deals
- **Copiers remain highly profitable (19.39% of total profit) despite low discounting (16.2%)**, proving that heavy discounts aren't required to drive strong sales
- The **top 20% of customers** contribute a disproportionately high share of total revenue (Pareto effect)
- **Standard Class** is the most-used shipping mode but also carries the longest average delivery time (5.0 days) and the lowest margin (12.1%) among all ship modes

---

## 🔍 Root Cause Analysis

### 🔴 Root Cause 1 — Discounting is the biggest reason for low profit

Profitability collapses almost linearly as discount depth increases — beyond a ~30% discount, orders become **loss-making on average**:

| Discount Band | Sales | Profit | Margin % |
|---|---:|---:|---:|
| 0% (No Discount) | $1,087,908 | $320,988 | **29.51%** |
| 1–10% | $54,369 | $9,029 | **16.61%** |
| 10–30% | $895,380 | $81,387 | **9.09%** |
| 30–50% | $195,315 | -$48,448 | **-24.80%** |
| 50%+ | $64,229 | -$76,559 | **-119.20%** |

The damage runs deeper than the margin table alone shows:

- **Profit margins turn negative once discounts exceed 30%** — there is no discount band beyond this point that remains profitable
- **1,020 orders (20.4% of all orders) were discounted above 30%** — meaning 1 in every 5 orders sold was sold at a heavily discounted price
- **The company gave away $322.58K in discounts** — more than the entire $286.82K in profit earned over the same period
- **1,318 of 5,009 orders (26%) resulted in a loss**, and loss-making products climb sharply as discount depth increases, with **380 loss-making products** in the 50%+ discount band alone
- **Products discounted 70–80% (e.g. Eureka Disposable at 80%) are almost universally loss-making**, effectively selling at a guaranteed loss on every unit


### 🔴 Root Cause 2 — One category (Furniture) is structurally unprofitable

| Category | Sales | Profit | Avg. Discount | Margin % |
|---|---:|---:|---:|---:|
| Technology | $836,154 | $145,455 | 13.2% | **17.4%** |
| Office Supplies | $719,047 | $122,491 | 15.7% | **17.0%** |
| **Furniture** | $742,000 | **$18,451** | **17.4%** | **2.5%** |

Furniture generates nearly as much revenue as Technology but returns **7x less profit**, driven almost entirely by three sub-categories:

| Sub-Category | Sales | Profit | Avg. Discount | Margin % |
|---|---:|---:|---:|---:|
| **Tables** | $206,966 | **-$17,725** | 26.1% | **-8.6%** |
| **Bookcases** | $114,880 | **-$3,473** | 21.1% | **-3.0%** |
| Supplies | $46,674 | -$1,189 | 7.7% | -2.5% |

**Tables and Bookcases get discounted the most out of any products, and both are sold at a loss.** In other words, the company is losing money to sell more of these items — boosting sales numbers while actually hurting overall profit.

- **Furniture received the highest average discount (17.4%) of any category, but contributed only 6.58% of total profit** despite generating over $742K in sales — the weakest return on discount investment in the dataset
- **Technology received the lowest average discount (13.2%) and generated the highest profit contribution (50.71%)** — the clearest evidence that lighter discounting protects margin
- **Tables generated strong sales but resulted in an overall loss**, driven almost entirely by heavy discounting rather than a fundamentally weak product
- **Copiers generated 19.39% of total profit with a relatively low average discount (16.2%)**, showing that premium, lightly-discounted products remain highly profitable


### 🔴 Root Cause 3 — Losses Are Concentrated in a Few States

Some states lose the company more money than others — a few because they sell a lot (even small losses add up), and others because their margins are consistently bad regardless of sales volume:

| State | Sales | Profit | Margin % |
|---|---:|---:|---:|
| Ohio | $78,258 | -$16,971 | -21.69% |
| Colorado | $32,108 | -$6,528 | -20.33% |
| Tennessee | $30,662 | -$5,342 | -17.42% |
| Illinois | $80,166 | -$12,608 | -15.73% |
| Texas | $170,188 | -$25,729 | -15.12% |
| North Carolina | $55,603 | -$7,491 | -13.47% |
| Pennsylvania | $116,512 | -$15,560 | -13.35% |
| Arizona | $35,282 | -$3,428 | -9.72% |
| Oregon | $17,431 | -$1,190 | -6.83% |
| Florida | $89,474 | -$3,399 | -3.80% |

**Texas is the single largest loss contributor in dollar terms (-$25,729)** despite a mid-table margin, simply due to its high sales volume — while **Ohio and Colorado post the worst margins (-21.69% and -20.33%)** on comparatively modest sales, suggesting a deeper pricing or discounting problem specific to those states rather than a volume issue.

Together, these 10 states account for roughly **-$98.2K in combined losses — over a third of total company profit** ($286.82K), making geography one of the most concentrated and addressable sources of the company's overall profitability problem.

A few additional patterns stand out:

- **Collectively, these 10 states generated $705.7K in sales but returned an overall margin of -13.9%** — meaning this isn't one bad state dragging down an otherwise healthy group; the entire group is structurally unprofitable together
- **Texas alone accounts for ~26% of the combined losses** across all 10 states, making it the single highest-priority market for a margin review
- **The problem isn't limited to large states** — Oregon, the smallest state in this group by sales ($17,431), still posts a meaningful loss (-6.83%), showing the issue exists at small and large scale alike
- **Every state in this list has a negative margin regardless of size** — from Texas at $170K in sales down to Oregon at $17K — indicating a shared, systemic cause (likely discounting, tied to Root Cause 1) rather than isolated one-off pricing mistakes in a single market


### 🔴 Root Cause 4 — A Lot of Orders Are Sold at a Loss

**1,318 out of 5,009 orders (26.3%) lost money.** That means roughly **1 in every 4 orders shipped actually cost the company money**, no matter how much total revenue was growing. Combined with the first three root causes, this suggests the real problem isn't a few bad deals — it's how discounts get approved in the first place.

---


## ✅ Actionable Recommendations

| # | Recommendation | Root Cause Addressed |
|---|---|---|
| 1 | **Cap discount approval at 30%** — require manager sign-off beyond this point, since margin turns negative past 30% and losses accelerate sharply after 50% | Discounting (Cause 1) |
| 2 | **Immediately review all orders discounted above 50%** — this band alone destroys over **$76.5K in profit**, and products like Eureka Disposable at 80% off are sold at a near-guaranteed loss | Discounting (Cause 1) |
| 3 | **Re-price or renegotiate supplier costs for Tables and Bookcases**, or cap their maximum allowable discount specifically — these two sub-categories are structurally loss-making regardless of volume | Furniture losses (Cause 2) |
| 4 | **Use Copiers and Technology as the pricing model to follow** — both stay lightly discounted (13–16%) and remain highly profitable, proving heavy discounting isn't necessary to drive sales | Furniture losses (Cause 2) |
| 5 | **Run a full margin audit on Texas, Ohio, Colorado, Illinois, and Pennsylvania** — Texas drives the largest dollar loss from volume, while Ohio and Colorado post the worst margins regardless of size, pointing to state-level pricing or discount overrides | Geographic losses (Cause 3) |
| 6 | **Investigate why losses appear at every state size** — from Texas ($170K in sales) down to Oregon ($17K) — since a shared negative margin across small and large states alike points to a systemic discount policy issue, not isolated bad deals | Geographic losses (Cause 3) |
| 7 | **Fix the discount *approval process*, not just individual deals** — with 1 in 4 orders (26.3%) unprofitable, the issue is systemic; add a mandatory margin check before an order is confirmed, not just periodic reviews after the fact | Loss-making orders (Cause 4) |
| 8 | **Shift sales incentives from revenue-based to margin-based targets** to stop rewarding discount-driven volume growth that shows up in Sales but not Profit | Growth-profit gap (overall) |
| 9 | **Monitor Sales vs. Profit trend and Margin % as paired KPIs** (already built into the Home and Sales Overview pages) rather than tracking Sales growth alone, so margin erosion is caught earlier next cycle | Growth-profit gap (overall) |

---

## 👤 Author

**Shivanand S. Mathapati**


- 🌐 Portfolio: 
- 💼 LinkedIn: 
- 📺 YouTube: 
- ✉️ Email: 

---

## 📄 License

This project is licensed under the **MIT License** — feel free to use the dashboard structure and DAX patterns for your own learning or portfolio projects. See [LICENSE](LICENSE) for details.

---

<div align="center">

⭐ **If you found this project useful, consider giving it a star!** ⭐

</div>
