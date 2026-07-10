# 📊 Global Superstore Analytics Dashboard

An end-to-end, executive-ready Power BI Business Intelligence solution that transforms raw, uncleaned multinational retail transactions into clear, actionable business insights.

---

## 🚀 Project Overview
This project delivers a multi-perspective interactive analytics application utilizing the **Global Superstore Dataset**. It addresses the challenges of analyzing uncleaned, multi-year operational rows and converts them into cross-filterable visual dashboards. Designed for executive leadership and regional operations managers, this dashboard tracks organizational efficiency, highlights profitability leakage, and surfaces key growth metrics globally.

### 📈 Core Business Metrics Analyzed
* **Total Revenue:** $12.64 Million
* **Total Profit:** $1.47 Million
* **Total Volumes Transacted:** 178,312 Units
* **Total Unique Products Managed:** 10,292 active SKUs
* **Global Customer Base:** 4,873 unique clients

---

## 🛠️ Data Architecture & Pipeline

### 1. Inherent Challenges in Raw Dataset
The raw uncleaned dataset (`51,290 rows`) presented several modeling roadblocks that were structurally treated within Power Query before visualization:
* **Malformed Dates:** Order and shipping metrics were encoded as unparsed variant strings (`0.00.00`) preventing out-of-the-box time intelligence metrics.
* **Redundant Entities:** Duplicate geographic columns (`Market` vs. `Market2`) threatened model storage and dimensional filtering integrity.
* **Implicit Data Types:** Floating currency variables (`Sales`, `Profit`, `Shipping Cost`) required fixed decimal coercion to guarantee zero arithmetic variance.

### 2. ETL (Extract, Transform, Load) Implementation Steps
* **Schema Enforcement:** Configured hard type properties for textual attributes (`City`, `State`, `Segment`) and accurate currency definitions.
* **Date Reconstruction:** Formatted and normalized calendar strings into absolute datetime stamps to establish dynamic period-over-period comparisons.
* **Text Hygiene:** Applied string trimming and casing filters to clear blank padding and eliminate duplicate unique keys for products and customers.
* **Model Pruning:** Purged system overhead files and non-value indexing keys (`ji_lu-shu`, redundant row metrics) to optimize query speed.

---

## 📐 Data Modeling (Star Schema Design)
To guarantee fast query responses and filter propagation, the final project model was converted from a flat uncleaned table into an optimized **Star Schema Layout**:
* **Fact Table:** `Fact_Sales_Transactions` (Holds numeric values: Sales, Profit, Quantity, Discount, Shipping Cost, and foreign keys).
* **Dimension Tables:**
  * `Dim_Products`: (Product ID, Product Name, Category, Sub-Category)
  * `Dim_Geography`: (City, State, Country, Region, Market)
  * `Dim_Customers`: (Customer ID, Customer Name, Segment)
  * `Dim_Calendar`: (Custom date dimensions supporting advanced dynamic YTD/MoM metrics)

---

## 🧮 Advanced DAX Formulations Engineered
The dashboard uses optimized Data Analysis Expressions (DAX) to calculate core business metrics dynamically:

```dax
-- Total Top-line Operational Sales
Total Sales = SUM(Fact_Sales_Transactions[Sales])

-- Total Net Bottom-line Profit
Total Profit = SUM(Fact_Sales_Transactions[Profit])

-- Dynamic Operational Profit Margin Percentage
Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)

-- Global Moving Average Quantity per Transaction Order
Avg Order Quantity = AVERAGE(Fact_Sales_Transactions[Quantity])
