# Montgomery County Warehouse & Retail Sales Analysis

**Business Analytics Final Group Project — Lenoir-Rhyne University**

This project investigates which beverage categories and suppliers account for the largest case volumes, whether transfers to retail outlets align with retail sales, and whether historical activity can predict next-month sales.

## Research questions
1. Which beverage categories, products, and suppliers account for the highest sales volumes?
2. How closely do transfers to retail locations align with actual retail sales?
3. Can sales history, transfers, warehouse activity, and product type predict next-month retail sales?

## What is in this repository
- `data/warehouse_retail_part_1.csv` and `data/warehouse_retail_part_2.csv` — the complete dataset split into two files for GitHub browser uploads (341,037 original records in total; 10 columns including added month date).
- `visualizations/` — six labeled PNG charts of observed results.
- `analysis/` — summary tables and test-set regression predictions.
- `report/Business_Case_Report.md` — full business case, findings, limitations, recommendations.
- `report/Data_and_Method.md` — data dictionary, source, cleaning rules, modeling method, reproduction notes.

## Key findings
- Highest-volume retail beverage category: **LIQUOR**, representing **38.2%** of the beverage retail cases in observed months.
- Leading retail supplier by case volume in the extract: **E & J GALLO WINERY**.
- 2019 total transfer-minus-retail-sales difference for beverage categories: **-6,651 cases** (net aggregate gap; does not establish stock levels).
- Held-out next-month sales regression: **MAE 4.22 cases, RMSE 18.73 cases**; naive current-month baseline: **MAE 4.45, RMSE 19.03**. See the report for model restrictions.

## Visualizations

![Beverage category volumes](visualizations/01_category_sales.png)
![Leading retail suppliers](visualizations/02_top_suppliers.png)
![Monthly sales observations](visualizations/03_observed_months.png)
![Transfer versus sales gaps](visualizations/04_transfer_sales_gap.png)
![Regression prediction comparison](visualizations/05_actual_vs_predicted.png)
![Regression error by category](visualizations/06_regression_error_by_category.png)

## Source
[Montgomery County Open Data: Warehouse and Retail Sales](https://data.montgomerycountymd.gov/d/v76h-r7br). The downloaded extract analyzed here includes 27 observed months over 2017, 2018, 2019, 2020 and 2026; it is **not** a complete monthly time series.

## Tableau note
The PNG charts above were recreated directly from the project data for publication. They are not exports of the original Tableau workbook. If the course requires Tableau-native deliverables, add your final `.twbx` packaged workbook or Tableau dashboard screenshots to `visualizations/` before submitting.

## Attribution
Prepared for a group course project. Each student should submit the URL of their own accessible GitHub repository, according to the assignment instructions.

## Working with the split CSV data
The two CSV parts are consecutive rows from the original dataset, each with an identical header. In Tableau, connect to the two CSVs and create a **union** (not a join) to analyze all 341,037 rows. Alternatively, combine them by keeping the header from part 1 and appending all rows after the header from part 2. Do not count the second header as data. The files are split only to stay below GitHub browser upload limits.
