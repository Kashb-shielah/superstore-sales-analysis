# Indian Superstore Sales Analysis
### Business Analytics Case Study | Microsoft Excel

![Dashboard Preview](dashboard-screenshot.png)

> **Replace `dashboard-screenshot.png` with your actual screenshot filename after uploading it to the repo**

---

## Project Overview

This project analyzes 12 months of transactional sales data from an Indian retail superstore (April 2018 – March 2019) across three product categories: Clothing, Electronics, and Furniture. The goal was to surface actionable business insights around profitability, geographic performance, sales target achievement, and customer health delivered through a structured Excel dashboard.

The dataset comprised three separate CSV files that required relational joining, date cleaning, and enrichment before any analysis could begin mirroring a real-world data analyst workflow.

---

## Dataset

| File | Rows | Description |
|---|---|---|
| `List_of_Orders.csv` | 560 | Order-level data: Order ID, Order Date, Customer Name, State, City |
| `Order_Details.csv` | 1,500 | Line-item data: Order ID, Amount, Profit, Quantity, Category, Sub-Category |
| `Sales_target.csv` | 36 | Monthly sales targets by category (Clothing, Electronics, Furniture) |

**Source:** [Kaggle E-Commerce Sales Dataset](https://www.kaggle.com/datasets/benroshan/ecommerce-data)

**Key relationship:** `Order Details` and `List of Orders` are joined on `Order ID`. `Sales Target` is matched to actuals on `Month-Year + Category`.

---

## Tools Used

- **Microsoft Excel Online** full analysis and dashboard
- Pivot Tables & Pivot Charts
- XLOOKUP, SUMIFS, IF, TEXT, DATE, LEFT, MID, RIGHT
- Conditional Formatting
- Manual calculated columns (Profit Margin %, Year-Month key, Quarter, Year)
- Dashboard design (KPI cards, bar charts, line charts)

---

## Data Preparation

The raw CSVs required significant preparation before analysis:

- **Date format inconsistency**  `Order Date` contained two mixed formats (`D/M/YYYY` with slashes, `DD-MM-YYYY` with dashes). Excel Online auto-converted the slash-format dates using US locale (MM/DD), silently swapping day and month. Fixed using a nested `IF(ISNUMBER())` formula combining `DATE/YEAR/DAY/MONTH` for auto-converted values and `DATE/RIGHT/MID/LEFT` for text value ensuring all 560 dates were correctly interpreted as DD/MM/YYYY (Indian convention).

- **Cross-table relationship**  `Order Details` (line items) had no customer or location data. Used `XLOOKUP` to bring `Order Date`, `CustomerName`, `State`, and `City` from `List of Orders` into `Order Details` via `Order ID`, creating a fully self-sufficient analytical table.

- **Month-Year key** Created a `Year-Month` column using `TEXT(date, "YY-MMM")` to match the `Sales Target` table's format (e.g. "18-Apr"), enabling `SUMIFS`-based actual-vs-target comparisons.

- **Analytical helper columns added to Order Details:**
  - `Profit Margin %` `=Profit/Amount` per line item
  - `Year`  extracted via `YEAR()`
  - `Quarter`  derived via `=ROUNDUP(MONTH(date)/3, 0)`
  - `Year-Month`  text key for target matching

- **Data quality checks** confirmed zero duplicates across all three tables, zero blank values in key columns, and no inconsistent category/state spellings.

---

## Analysis Structure

| Part | Question |
|---|---|
| Performance vs Target | Are we hitting monthly sales targets by category? |
| Profitability | Which categories and sub-categories make vs. lose money? |
| Geography | Where is the business strong vs. weak? |
| Customers | Is the customer base healthy? Any concentration risk? |
| Trends | What does the overall profit trajectory look like over time? |

---

## Key Findings

### 1. The Business Lost Money for Six Consecutive Months ,Then Turned Around

Every month from **April through September 2018 was profit-negative** (total losses: ~₹21,795 across six months). October 2018 was the exact inflection point first profitable month (+₹3,093) and the business remained profitable every single month through March 2019, peaking at ₹11,619 in November 2018 and ₹10,077 in March 2019.

This is not just a "performance vs target" story it reflects real profit/loss, suggesting a structural operational improvement around October 2018 (better margin discipline, reduced discounting, or a shift away from loss-making products).

### 2. Tables Is the Only Major Loss-Making Sub-Category

| Sub-Category | Profit Margin % |
|---|---|
| T-shirt | 20.32% |
| Shirt | 14.97% |
| Accessories | 16.38% |
| Printers | 10.24% |
| ... | ... |
| Electronic Games | -3.16% |
| **Tables** | **-17.74%** |

**Tables** loses money across almost every state it's sold in (9 of 11 states show negative margin). **Electronic Games** is also marginally loss-making (-3.16%). All other 15 sub-categories are profitable.

A data-quality check confirmed Tamil Nadu's dramatic -136.66% Tables margin rests on a **single transaction** (Order B-25608: 5 tables, ₹1,364 revenue, -₹1,864 profit) flagged as an anomaly, not a regional pattern.

### 3. Four States Are Operating at a Loss

| State | Profit Margin % | Note |
|---|---|---|
| Tamil Nadu | -36.41% | Single-order anomaly |
| Andhra Pradesh | -3.74% | Sustained loss |
| Punjab | -3.63% | Sustained loss |
| Bihar | -2.48% | Sustained loss |

The sustained losses in **Punjab, Andhra Pradesh, and Bihar** are real patterns across multiple orders and warrant pricing or cost-structure review in those markets.

### 4. Revenue Is Geographically Concentrated

**Madhya Pradesh** (₹105,140) and **Maharashtra** (₹95,348) together represent **~46% of total revenue** nearly half the business rests on two states. This is a concentration risk: strong regional dependency with limited diversification across India's remaining 20+ states in the dataset.

| State | Revenue | Margin |
|---|---|---|
| Madhya Pradesh | ₹105,140 | 5.28% |
| Maharashtra | ₹95,348 | 6.48% |
| Delhi | ₹22,531 | 13.26% |
| Uttar Pradesh | ₹22,359 | 14.48% |

### 5. Customer Base Is Healthy ,No Concentration Risk

- **332 unique customers** across 560 orders
- **Top 10 customers = 15.94% of total revenue** (no dangerous dependency on a small number of buyers)
- Top customers show genuine repeat engagement (double-digit line-item counts), not single large orders

---

## Dashboard

The final dashboard delivers all five findings in one view:

- **KPI Cards** Total Revenue (₹431,502), Total Profit (₹23,955), Overall Margin (5.55%), Total Customers (332)
- **Line Chart** "Six Months of Losses, Then a Turnaround" monthly profit trajectory Apr 2018–Mar 2019
- **Bar Chart** "Profit Margin % by Sub-Category" ranked worst to best, loss-makers highlighted in red
- **Bar Chart** "Profit Margins Vary Widely Across States" loss-making states highlighted in red
- **Customer Health Summary** text callout confirming healthy customer distribution

![Dashboard](dashboard-screenshot.png)

---

## Business Recommendations

Based on this analysis, three immediate priorities stand out:

1. **Review Tables pricing and cost structure** this sub-category is losing money in 9 of 11 states. Either the product is being discounted too heavily, or landed costs (shipping, damage) make it structurally unprofitable. Recommend a full margin review before continuing to stock/promote Tables.

2. **Investigate Punjab, Andhra Pradesh, and Bihar losses** these three states show sustained negative margins across multiple orders and categories. A market-specific pricing or cost audit is warranted.

3. **Reduce geographic revenue concentration** with ~46% of revenue from two states, the business is exposed. A deliberate push into higher-margin, lower-volume states like Uttar Pradesh (14.48% margin) and Himachal Pradesh (7.57%) would both diversify risk and improve overall blended margin.

---

## Repository Structure
