# 💊⚕️ European Pharmacy Sales & Profitability Analysis (Power BI)

A multipage **Power BI Analytics Dashboard** designed to analyze revenue, profitability, product performance and regional trends across a European distribution network.

This project transforms transactional pharmacy sales data into an interactive business intelligence dashboard that helps stakeholders identify **where revenue comes from, which products drive profit, and which regions perform best.**

🔗 **Medium Article:**
https://medium.com/@kudehinbusamad/european-pharmacy-sales-and-profitability-analysis-79a17824293c

---

# 📌 Project Overview

Pharmacy chains generate large volumes of transaction data across multiple countries, regions, and locations. Without proper visualization and analysis, identifying trends and profitability drivers can be challenging.

This Power BI report was developed to provide insights into: 

- 📊 Revenue performance across countries
- 💰 Profitability across pharmacies
- 🏷️ Product and brand contribution to sales
- 📈 Promotion effectiveness
- 🌍 Geographic distribution of revenue

The report enables **data-driven decision-making for pharmacy operations and strategic planning.**

---

# 🎯 Business Problem

Pharmacy chains often struggle to answer important business questions such as:

- Which **countries or regions generate the most revenue?**
- Which **products contribute most to profit?**
- Do **promotions actually improve profitability?**
- Which **pharmacies are underperforming?**

Without a centralized analytics dashboard, these insights remain hidden inside raw spreadsheets.

This dashboard solves the problem by consolidating all key metrics into an **interactive Power BI report.**

---

# 📂 Dataset

The dataset represents **transaction-level pharmacy sales data from a European pharmacy network** operating across multiple countries.

### 📊 Dataset Scope

- Multi-country pharmacy sales network
- Multiple pharmacy locations
- Transaction-level sales data
- Time period covering **2024–2025**

### 📑 Dataset Fields

| Category | Fields |
|--------|--------|
| Sales Metrics | Revenue, Cost, Units Sold |
| Profitability | Margin, Margin % |
| Product Data | Product Name, Brand, Category |
| Location Data | Country, Region, City |
| Pharmacy Details | Pharmacy Type (Urban, Suburban, Rural) |
| Promotions | Promotion Status |

### 📈 Key Metrics and Data Analysis Expressions (DAX)

- Total Revenue
```
TOTAL REVENUE = SUM(FactSales[Revenue])
-----------------------------------------------------------------
TOTAL REVENUE MoM% = VAR CurrentPeriod = [TOTAL REVENUE]
VAR LastYear = CALCULATE(
    [TOTAL REVENUE],
    PREVIOUSMONTH('Calendar'[Date])
)
RETURN
DIVIDE(
    CurrentPeriod - LastYear,
    LastYear,
    BLANK()
)
--------------------------------------------------------------------
TOTAL REVENUE MoM% WITH ARROW = VAR MoMValue = [TOTAL REVENUE MoM%]
RETURN
IF(
    ISBLANK(MoMValue),
    "--",
    IF(
        MoMValue >= 0,
         FORMAT(MoMValue, "0.00%") & "▲ ",
         FORMAT(MoMValue, "0.00%") & "▼ "
    )
)
```
- Total Cost
```
TOTAL COST = SUM(FactSales[Cost])
-------------------------------------------------------------
TOTAL COST MoM% = VAR CurrentPeriod = [TOTAL COST]
VAR LastYear = CALCULATE(
    [TOTAL COST],
    PREVIOUSMONTH('Calendar'[Date])
)
RETURN
DIVIDE(
    CurrentPeriod - LastYear,
    LastYear,
    BLANK()
)
------------------------------------------------------------------
TOTAL COST MoM% WITH ARROW = VAR MoMValue = [TOTAL COST MoM%]
RETURN
IF(
    ISBLANK(MoMValue),
    "--",
    IF(
        MoMValue >= 0,
         FORMAT(MoMValue, "0.00%") & "▲ ",
         FORMAT(MoMValue, "0.00%") & "▼ "
    )
)
```
- Total Margin
```
TOTAL MARGIN = SUM(FactSales[Margin])
----------------------------------------------------------------------
TOTAL MARGIN MoM% = VAR CurrentPeriod = [TOTAL MARGIN]
VAR LastYear = CALCULATE(
    [TOTAL MARGIN],
    PREVIOUSMONTH('Calendar'[Date])
)
RETURN
DIVIDE(
    CurrentPeriod - LastYear,
    LastYear,
    BLANK()
)
------------------------------------------------------------------------
TOTAL MARGIN MoM% WITH ARROW = VAR MoMValue = [TOTAL MARGIN MoM%]
RETURN
IF(
    ISBLANK(MoMValue),
    "--",
    IF(
        MoMValue >= 0,
         FORMAT(MoMValue, "0.00%") & "▲ ",
         FORMAT(MoMValue, "0.00%") & "▼ "
    )
)
```
- Margin Percentage
```
MARGIN % = DIVIDE(SUM(FactSales[Margin]),SUM(FactSales[Cost]))
----------------------------------------------------------------------
MARGIN % MoM% = VAR CurrentPeriod = [MARGIN %]
VAR LastYear = CALCULATE(
    [MARGIN %],
    PREVIOUSMONTH('Calendar'[Date])
)
RETURN
DIVIDE(
    CurrentPeriod - LastYear,
    LastYear,
    BLANK()
)
-----------------------------------------------------------------------------
MARGIN % MoM% WITH ARROW = VAR MoMValue = [MARGIN % MoM%]
RETURN
IF(
    ISBLANK(MoMValue),
    "--",
    IF(
        MoMValue >= 0,
         FORMAT(MoMValue, "0.00%") & "▲ ",
         FORMAT(MoMValue, "0.00%") & "▼ "
    )
)
```
- Units Sold
```
TOTAL UNITS SOLD = SUM(FactSales[UnitsSold])
----------------------------------------------------------------------
TOTAL UNITS SOLD MoM% = VAR CurrentPeriod = [TOTAL UNITS SOLD]
VAR LastYear = CALCULATE(
    [TOTAL UNITS SOLD],
    PREVIOUSMONTH('Calendar'[Date])
)
RETURN
DIVIDE(
    CurrentPeriod - LastYear,
    LastYear,
    BLANK()
)
---------------------------------------------------------------------
TOTAL UNITS SOLD MoM% WITH ARROW = VAR MoMValue = [TOTAL UNITS SOLD MoM%]
RETURN
IF(
    ISBLANK(MoMValue),
    "--",
    IF(
        MoMValue >= 0,
         FORMAT(MoMValue, "0.00%") & "▲ ",
         FORMAT(MoMValue, "0.00%") & "▼ "
    )
)
```
- Pharmacies below Target Margin
```
PHARMACIES BELOW TARGET MARGIN = 
COUNTX(
    VALUES(DimPharmacy[PharmacyID]),
    IF([MARGIN %] < 0.38, 1)
)
```
- Top Pharmacy Revenue
```
TOP PHARMACY REVENUE = 
CALCULATE(
    [TOTAL REVENUE],
    TOPN(1, ALL(FactSales[PharmacyID]), [TOTAL REVENUE], DESC)
)
```
- Sales Volume
```
SALES VOLUME = COUNT(FactSales[SalesID])
```
- Product Count
```
PRODUCT COUNT = DISTINCTCOUNT(DimProduct[ProductID])
```
The dataset structure supports **hierarchical analysis from country → region → pharmacy → product.**

---

# 🧹 Data Cleaning & Preparation

Data preparation was completed before visualization using **Power Query and DAX**.

Key steps included:

- Validating revenue calculations
- Ensuring **Revenue = Cost + Margin**
- Standardizing location hierarchy
- Cleaning inconsistent product categories
- Structuring the dataset for analytical reporting

The final dataset was optimized for **fast dashboard performance and drill-down analysis.**

---

# 🧠 Data Model

The dashboard follows a **dimensional data model**.

### ⭐ Fact Table
- Sales Transactions

### 📚 Dimension Tables
- Products
- Pharmacies
- Locations
- Date
- Calendar Table
```
Calendar = 

VAR _minYear = YEAR(MIN(DimDate[Date]))
VAR _maxYear = YEAR(MAX(DimDate[Date]))
VAR _fiscalStart = 4 

RETURN
ADDCOLUMNS(

    CALENDAR(

                DATE(_minYear,1,1),

                DATE(_maxYear,12,31)

),
       "Year", YEAR ( [Date] ),
    "Month Number", MONTH ( [Date] ),
    "Month Name", FORMAT ( [Date], "M"),
    "Month Name (3)", FORMAT ( [Date], "MMM" ),
    "Month Name Full", FORMAT ( [Date], "MMMM" ),
    "Year-Month", FORMAT ( [Date], "YYYY-MM"),
    "Qtr", "Q" & QUARTER ( [Date] ),
    "Week Number", WEEKNUM ( [Date], 2 ),   
    "Day", DAY ( [Date] ),
    "Day of Week", WEEKDAY ( [Date], 2 ),   
    "Day Name", FORMAT ( [Date], "dddd" )
)
```
<img width="1340" height="683" alt="Data Model" src="https://github.com/user-attachments/assets/202138c6-f82e-4501-8c68-feeb20975b54" />

This model enables **efficient filtering, drill-down analysis, and high performance in Power BI.**

---


# 📊 Dashboard Structure

The report consists of **three analytical pages.**

---

# 📈 Page 1 — Performance Overview

### 🎯 Purpose

Provide a high-level snapshot of pharmacy network performance.

<img width="1349" height="826" alt="page 1" src="https://github.com/user-attachments/assets/6060a283-075a-4cb5-8b06-393e22ec36ea" />

### 📌 KPIs

- 💰 Total Revenue
- 💵 Total Margin
- 💳 Total Cost
- 📊 Margin Percentage 
- 📦 Units Sold 

### 📊 Visuals

- KPI cards
- Monthly KPIs trend using field parameters
- Geographic revenue distribution map
- Top 5 revenue by country
- Revenue by promotion

# 🌍 Page 2 — Regional Performance

### 🎯 Purpose

Analyze revenue and profitability across pharmacy locations.

<img width="1349" height="828" alt="page 2" src="https://github.com/user-attachments/assets/bee0379c-4c09-4de0-80f2-2b10147054a7" />

### 📊 Visuals

- Margin distribution map
- Revenue by region
- Pharmacy type comparison
- Regional drill-down analysis

# 🧪 Page 3 — Product Performance

### 🎯 Purpose

Evaluate product categories, brands, and promotional strategies.

<img width="1352" height="823" alt="page 3" src="https://github.com/user-attachments/assets/2983c86a-2950-4f94-8cc9-db4c98d26a38" />

### 📊 Visuals

- Revenue by product category
- Top brands by revenue
- Top brands by margin
- Volume vs Margin scatter plot
- Promotion impact analysis

---
# ✨ Key Dashboard Features

- Interactive slicers and filters
- Geographic revenue map
- Drill-down capability
- Multi-page report navigation
- Product profitability analysis
- Promotion performance tracking

---

# 💡 Key Insights

Major insights discovered:

- Sales exhibit **clear seasonal demand patterns**
- Revenue is concentrated in a **few high-performing countries**
- Urban pharmacies dominate total sales volume
- Product profitability varies significantly
- Some promotions may reduce margins

These insights help businesses **optimize inventory, pricing, and marketing strategies.**

---

# 🚀 Strategic Recommendations

### 📍 Optimize Geographic Strategy
Focus investments on high-performing countries and improve underperforming regions.

### 🏪 Tailor Pharmacy Strategies
Urban, suburban, and rural pharmacies should use different sales strategies.

### 📦 Improve Product Mix
Balance **high-volume products with high-margin products**.

### 📢 Optimize Promotions
Focus promotions on products that maintain healthy margins.

### ⭐ Prioritize High-Value Products
Products with **both high margin and high sales volume** should receive additional marketing attention.

# 🔚 Conclusion
This pharmacy analytics report transformed raw transaction data into a strategic tool. Instead of wondering where the business makes money, stakeholders now see it clearly: which countries drive profit, which pharmacy types are sustainable, which products to push, and which regions need attention.

For pharmacy chains operating across Europe with complex product portfolios and diverse locations, this kind of analysis is essential. It transforms the question from “Are we making money?” to “Where should we make more?”

---

# 🔗 Links

- 📊 [View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiMjIzNTc0ODctZmM2MS00ODRmLTk0Y2UtMGUwNmVjN2EyMWVhIiwidCI6IjgxMTQ1ZWNkLTc5NTAtNDk4Ny1hOGFmLTJhMDY1YTgwMWVhYyJ9)
- 📝 [View My Portfolio](https://sites.google.com/view/abdulsamadportfolio/home)
- 💼 [Connect with me on LinkedIn](https://www.linkedin.com/in/abdulsamad-kudehinbu/)
