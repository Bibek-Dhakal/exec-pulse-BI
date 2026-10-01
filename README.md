# ExecPulse-BI

**Interactive Sales & Operations BI Dashboard built on Star-Schema Data Modeling and DAX/LOD KPI Calculations.**

## Overview

ExecPulse-BI models raw transactional data into a dimensional Star Schema (1 Fact Table linked to multiple Dimension
Tables) using Python and creates interactive business dashboards using BI platforms like Power BI / Tableau. It is
designed to tackle scattered data sources by pushing heavy row-level transformations into programmatic ETL steps,
allowing executives to seamlessly analyze sales performance and customer metrics dynamically.

## Key Features

* **Automated Dimensional Modeling:** Python ETL pipeline that generates `Fact_Sales`, `Dim_Customer`, `Dim_Product`,
  and `Dim_Date` tables from raw chaotic logs.
* **Optimized for BI:** Pre-aggregated, strongly typed Star Schema designed to load instantly and handle complex DAX /
  LOD metrics natively.
* **Pre-Built Dashboard (`.pbix`):** Includes a ready-to-use Power BI dashboard with pre-configured relational models
  and dynamic DAX metrics.
* **Extensible Architecture:** SQLite / CSV output formats ensuring compatibility with Power BI, Tableau, Looker, or raw
  SQL queries.

## Quick Start

To generate the Star Schema database, CSV extracts, and view the dashboard:

```bash
# 1. Setup Environment
python -m venv venv
source venv/bin/activate
pip install -e ".[dev,notebooks]"

# 2. Run Data Pipeline
# Open the Jupyter notebook and run all cells to generate local data:
jupyter notebook notebooks/data_pipeline.ipynb
```

*Data outputs will be saved in the `data/processed/` directory.*

**3. View the Dashboard**
Open `dashboards/ExecPulse_Dashboard.pbix` in Power BI Desktop. You can seamlessly re-point the data source to your
newly generated local `execpulse_star_schema.db` or CSVs.

## Documentation Index

- [Architecture & ERD](docs/architecture/README.md)
- [Usage & BI Integration](docs/usage/README.md)
- [Code Quality & Setup](docs/CODE_QUALITY.md)

## Evaluation Criteria Addressed (SST 2)

- **Dimensional Data Model:** Complete Star Schema (Fact and Dimensions) architecture implemented.
- **Dynamic Measure Calculations:** Refer to the `docs/usage/README.md` for required Power BI DAX formulas.
- **Dashboard Load Latency:** Pre-calculated relationships in the schema ensure instantaneous visualization rendering.