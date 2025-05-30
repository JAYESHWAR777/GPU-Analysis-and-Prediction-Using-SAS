🔍 Analysis Workflow
1. Data Import & Cleaning
Use import_and_clean.sas to read Excel files via PROC IMPORT

Handle missing values and standardize variable formats

2. Exploratory Data Analysis (EDA)
Run eda_plots.sas for summary stats (PROC MEANS), frequency tables (PROC FREQ), and visualizations (PROC SGPLOT):

Scatter: Price vs. GamingScore

Bar: GPU counts by manufacturer/year

3. Statistical Analysis
corr_and_ranking.sas computes:

Feature correlations (PROC CORR)

Top-10 rankings for gaming, productivity, and price-performance (PROC RANK)

4. Predictive Modeling
arima_price_forecast.sas applies PROC ARIMA to forecast INR prices through 2030

Use PROC REG / PROC GLM for regression-based performance predictions

5. Advanced Analytics (Clustering & Classification)
clustering_classification.sas runs:

Decision-tree classification (PROC HPSPLIT)

K-means clustering (PROC FASTCLUS)


🔄 How to Reproduce
Open each .sas script in SAS Studio or your SAS environment.

Update file paths if your data folder is in a different location.

Run scripts in sequence:

import_and_clean.sas

eda_plots.sas

corr_and_ranking.sas

arima_price_forecast.sas

clustering_classification.sas

Export results and visualizations as shown in the report.

1. Data Pipeline & ETL
Source Data

Raw .xlsx files containing GPU specs (memory size, clock speeds, shaders), performance benchmarks (gaming/productivity scores), pricing (INR), and derived “future-proof” scores.

Historical data (2018–2022) from Kaggle, extended manually with 2023–2025 releases.

Extraction & Cleaning (SAS)

PROC IMPORT to read Excel into SAS datasets.

Missing-value imputation: replaced missing clock speeds with median values per vendor; flagged outliers beyond 1.5× IQR for manual review.

Standardization: unified column names (e.g. Memory_GB, Boost_MHz), converted INR prices to numeric.

Transformation & Feature Engineering

Derived metrics in SAS:

Price-Performance Ratio = GamingScore / Price_INR

Normalized Future-Proof Score (scaled 0–1) based on VRAM and bus width.

Time-series aggregation by year: average price and performance trends.

Loading to BI

Exported cleaned tables from SAS to CSV via PROC EXPORT.

Loaded CSVs into Power BI Desktop using its built-in Excel/CSV connectors.

2. Dashboard Architecture
Data Model

Star schema:

Fact table: GPU_Facts (one row per GPU spec)

Dimension tables:

Dim_Year (ReleaseYear, YearName)

Dim_Vendor (Manufacturer, Region)

Relationships: Fact → Dimensions (many-to-one).

DAX Measures

Total GPUs: COUNTROWS(GPU_Facts)

Avg. Gaming Score by Year: AvgGaming = AVERAGE(GPU_Facts[GamingScore])

Projected Price (2025–2030): imported forecast table from SAS’ ARIMA output, then

ProjectedPrice = SUM(Forecast[PriceForecast])


Query Folding & Incremental Refresh

Used Power Query M scripts to filter data at source (only 2018+), enabling query folding for performance.

Configured incremental refresh in Power BI Service to fetch only new GPU releases.

3. Visualizations & Interactivity


| Page                   | Visual Type             | Key Features                                                    |
| ---------------------- | ----------------------- | --------------------------------------------------------------- |
| **Overview**           | Card visuals, KPI tiles | Total GPUs, Avg. Price, Avg. Gaming & Productivity              |
| **Performance Trends** | Line & area charts      | Yearly trends of scores and prices; dynamic slicers             |
| **Top GPUs**           | Bar charts, Tables      | Top-10 by category; conditional formatting to highlight winners |
| **Correlation Matrix** | Heatmap (custom visual) | Pearson correlations between spec fields and scores             |
| **Clustering**         | Scatter + cluster map   | K-means clusters plotted on spec dimensions; cluster slicer     |
| **Forecast**           | Line chart with bands   | Historical vs. projected price (with 95% CI)                    |


Interactive Elements:

Slicers for Vendor, Year, Memory Type.

Drill-through from “Top GPUs” table to a detailed “GPU Profile” page.

Bookmarks & buttons to switch between gaming vs. productivity focus.

4. Deployment & Sharing
Power BI Service:

Published workspace with row-level security (RLS) roles to restrict region-specific viewers.

Scheduled dataset refresh every Sunday (pulls any new data from CSV exports).

Access: shared via organizational app; embedded in internal Confluence via Power BI embed link.

5. Tech Stack Summary

| Layer                    | Technology / Tool            |
| ------------------------ | ---------------------------- |
| **Data Storage**         | Excel (.xlsx), CSV           |
| **Data Prep & Modeling** | SAS 9.4 (Base SAS, SAS/STAT) |
| **Data Export**          | PROC EXPORT (SAS → CSV)      |
| **BI & Visualization**   | Power BI Desktop & Service   |
| **Scripting**            |                              |


SAS: data cleaning, EDA, ARIMA forecasting, clustering/classification

Power Query (M): data ingestion, transformation

DAX: advanced measures, calculated columns
| Version Control | Git / GitHub |
| Documentation | Markdown (README.md), PDF/Word reports |6. Additional Resources
SAS Documentation:

PROC SGPLOT & PROC ARIMA guides on support.sas.com

Power BI Learning:

Microsoft Learn modules on Data Modeling and DAX

Cluster Analysis Reference:

“An Introduction to Cluster Analysis” by Kaufman & Rousseeuw







