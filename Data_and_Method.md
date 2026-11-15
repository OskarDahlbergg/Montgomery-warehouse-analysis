# Data & Method

## Source and scope
Original public source: [Montgomery County, Maryland — Warehouse and Retail Sales](https://data.montgomerycountymd.gov/d/v76h-r7br). Source agency: Montgomery County Alcohol Beverage Services. Analysis file: `warehouse_retail_part_1.csv` and `warehouse_retail_part_2.csv` (combined). The file has **341,037 records and 10 columns**: 9 original fields and an added `MONTH DATE` field.

Observed months: 27 months across 2017, 2018, 2019, 2020, 2026. Missing months mean that connecting points on the trend chart is visual guidance, *not evidence that intervening months were observed*.

## Field dictionary
| Field | Definition |
|---|---|
| YEAR | Calendar year |
| MONTH | Calendar month number |
| SUPPLIER | Reported supplier |
| ITEM CODE | Product identifier, handled as text |
| ITEM DESCRIPTION | Product label |
| ITEM TYPE | Product category |
| RETAIL SALES | Cases sold at retail outlets |
| RETAIL TRANSFERS | Cases transferred to retail outlets |
| WAREHOUSE SALES | Cases sold from warehouse to licensed buyers |
| MONTH DATE | Derived first-day-of-month date |

## Cleaning and grouping
- Treat sales/transfer/warehouse measures as quantities of **cases**, not revenue or profit.
- Preserve the original data in `data/` rather than overwrite or silently remove values.
- For summary charts, filter to beverage types: BEER, WINE, LIQUOR, KEGS, NON-ALCOHOL; exclude REF, DUNNAGE, STR_SUPPLIES and blank type to avoid interpreting operational movements as consumer demand.
- Missing retail-sales observations are excluded by pandas aggregations (not assumed to be actual zero transactions).
- Supplier rankings exclude missing supplier names.
- Annual and supplier rankings cover only months present in the extract; differences in temporal coverage distort simple year-to-year comparisons.

## Research approach
**Question 1:** Sum retail and warehouse cases by beverage type; rank supplier total retail cases. Chart totals for all observed periods.

**Question 2:** Restrict to 2019 January–November, all observed consecutively, then calculate `sum(RETAIL TRANSFERS) - sum(RETAIL SALES)` for each beverage category. This is a **net flow difference**, not direct inventory, stockout or profit data.

**Question 3:** Within 2019, aggregate to a unique product (`ITEM CODE`)-month record. Create a next-month outcome only by matching that item to the **following calendar month**. Remove negative and missing numeric modeling observations, which may represent adjustments or returns; retain nonnegative zeros. Train with predictor months January–August and test on September–October (predict October–November). Predict next-month cases from current retail sales, retail transfers, warehouse sales, month number, and one-hot-encoded item type. Model: log1p-transformed nonnegative numeric predictors, then standardized features and ridge linear regression on `log(1 + next-month cases)` with fixed alpha 1000; reverse-transform and clamp negative forecasts to zero. This is a penalized multiple linear regression used for an interpretable model, not a claim of causality. The time-based holdout minimizes future information leakage. Compare with a naive baseline predicting next-month sales equal to current-month sales. Report MAE, RMSE, and R² on the held-out rows.

Modeling sample sizes: **100,868 matched, eligible item-month rows; 80,439 train; 20,429 test**. Some products can be absent in a next month and hence not enter this matched sample; conclusions may not generalize to disappearing items.

## Reproducibility
The original dataset is included. All aggregate results are also exported as CSVs in `analysis/`. The provided figures were created with Python from that data, not exported from Tableau; the original `.twbx` should be added separately if requested.

## Working with the split CSV data
The two CSV parts are consecutive rows from the original dataset, each with an identical header. In Tableau, connect to the two CSVs and create a **union** (not a join) to analyze all 341,037 rows. Alternatively, combine them by keeping the header from part 1 and appending all rows after the header from part 2. Do not count the second header as data. The files are split only to stay below GitHub browser upload limits.
