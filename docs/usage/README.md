# Usage & BI Integration Guide

This guide walks you through taking the output of the data pipeline and integrating it into your BI visualization tool (Power BI / Tableau).

## 1. Connecting Data to Your BI Tool

After running `notebooks/data_pipeline.ipynb`, you will have your Star Schema output in `data/processed/`.

**In Power BI / Tableau:**
1. Connect to the SQLite Database (`execpulse_star_schema.db`) via ODBC **OR** load the 4 CSV files directly (`Fact_Sales.csv`, `Dim_Product.csv`, `Dim_Customer.csv`, `Dim_Date.csv`).
2. Go to the **Model View** (Power BI) or **Data Source Interface** (Tableau).
3. Establish the 1-to-Many relationships as specified in the [Architecture ERD](../architecture/README.md).
    * `Dim_Customer.customer_id` (1) $\rightarrow$ `Fact_Sales.customer_id` (*)
    * `Dim_Product.product_id` (1) $\rightarrow$ `Fact_Sales.product_id` (*)
    * `Dim_Date.date_key` (1) $\rightarrow$ `Fact_Sales.date_key` (*)
4. **Ensure cross-filter direction is set to "Single" (filtering from Dim to Fact).**

## 2. Recommended KPI Measures (DAX / LOD Examples)

According to the system specifications, hardcoding values on the visual layer is forbidden. Use these formulas to create dynamic measures.

### Total Revenue
**DAX (Power BI):**
```dax
Total Revenue = SUM(Fact_Sales[total_amount])
```
**LOD (Tableau):**
```text
{ SUM([total_amount]) }
```

### Total Profit
**DAX (Power BI):**
*Note: Calculate Cost from the related Dimension table first using SUMX or related columns.*
```dax
Total Cost = SUMX(Fact_Sales, Fact_Sales[quantity] * RELATED(Dim_Product[unit_cost]))
Total Profit = [Total Revenue] - [Total Cost]
Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)
```

### Year-over-Year (YoY) Growth
**DAX (Power BI):**
```dax
Revenue Last Year = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dim_Date[full_date]))
YoY Growth % = DIVIDE([Total Revenue] - [Revenue Last Year], [Revenue Last Year], 0)
```

### Rolling 30-Day Sales
**DAX (Power BI):**
```dax
Rolling 30D Revenue =
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(Dim_Date[full_date], MAX(Dim_Date[full_date]), -30, DAY)
)
```

## 3. Dashboard Interactivity Check
- Ensure that clicking on a "Product Category" in a Bar Chart accurately cross-filters the Time Series and KPI Summary Cards.
- Verification threshold: Load latency must remain $< 3\text{ seconds}$ upon slicer selections.
