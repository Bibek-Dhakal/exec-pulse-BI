# Usage & BI Integration Guide

This guide walks you through connecting the output of the data pipeline into your BI visualization tool. You can either
use the pre-built template provided in this repository or build your own from scratch.

## Option A: Using the Pre-built Power BI Dashboard (Recommended)

We have provided a fully configured Power BI dashboard template located at **`dashboards/ExecPulse_Dashboard.pbix`**.
Because absolute file paths differ between computers, you will need to map the dashboard to your locally generated data.

1. Ensure you have run `notebooks/data_pipeline.ipynb` so that your `data/processed/` folder is populated.
2. Open `dashboards/ExecPulse_Dashboard.pbix` in Power BI Desktop.
3. On the Home ribbon, click **Transform Data** $\rightarrow$ **Data source settings**.
4. Select the current data source (e.g., the ODBC DSN or the CSV folder path) and click **Change Source**.
5. Browse and point it to your local absolute path for `data/processed/execpulse_star_schema.db` (or your processed
   CSVs).
6. Click **Close & Apply**, then click **Refresh** on the Home ribbon.

---

## Option B: Connecting Data from Scratch

If you prefer to map the data yourself:

1. Load the 4 CSV files (one by one) directly (`Fact_Sales.csv`, `Dim_Product.csv`, `Dim_Customer.csv`, `Dim_Date.csv`)
   into Power BI. **OR** Connect to the SQLite Database (`execpulse_star_schema.db`) via ODBC:
   [Loading DB file into Power BI](loading-db-file-into-power-BI.md)

2. Go to the **Model View** (Power BI). By default, the points below are often auto-detected, but you should manually
   verify:
    - Establish the 1-to-Many relationships as specified in the [Architecture ERD](../architecture/README.md).
        * `Dim_Customer.customer_id` (1) $\rightarrow$ `Fact_Sales.customer_id` (*)
        * `Dim_Product.product_id` (1) $\rightarrow$ `Fact_Sales.product_id` (*)
        * `Dim_Date.date_key` (1) $\rightarrow$ `Fact_Sales.date_key` (*)
    - **Ensure cross-filter direction is set to "Single" (filtering from Dim to Fact).**

## 2. Recommended KPI Measures (DAX / LOD Examples)

According to the system specifications, hardcoding values on the visual layer is forbidden. If you are building from
scratch, use these formulas to create dynamic measures. *(Note: These are already included in the pre-built `.pbix`!)*

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

When finalizing your visualization, verify the following:

- Ensure that clicking on a "Product Category" in a Bar Chart accurately cross-filters the Time Series and KPI Summary
  Cards.
- Verification threshold: Load latency must remain $< 3\text{ seconds}$ upon slicer selections. The Star Schema ensures
  this is effortlessly achieved.