# Business Case Report

## Executive summary
Our group analyzed Montgomery County public records of warehouse and retail beverage case movements to identify product demand concentration, gaps between supply transfers and retail sales, and the usefulness of a next-month sales forecast. The dataset has **341,037 product-month records** across **27 observed calendar months**, but many calendar months are absent. The findings support assortment and distribution monitoring, while also showing why case volumes must not be confused with sales revenue or profitability.

## Business case and objectives
A distributor and retail network need to know which product groups matter most, whether deliveries are aligned with consumption, and whether the previous month's activity helps plan the next month. Poor alignment can lead to overstock or unfilled demand. This investigation focuses on three questions presented in our original project plan and uses summary visualizations and a product-level regression.

## Data and methods
We used the Montgomery County Open Data “Warehouse and Retail Sales” dataset, supplied in a Tableau-ready CSV. The unit of all three sales/movement metrics is **cases**. We first aggregated beverage product types and suppliers across observed months; next assessed 2019 retail transfer gaps; finally estimated next-month item sales from prior item activity with a held-out 2019 test set. All charts and metrics are reproducible from the included data tables and are explained in `Data_and_Method.md`.

## Question 1 — Product and supplier performance
**Finding.** Across the observed months, LIQUOR had the largest retail case volume (**904,926 cases; 38.2% of the beverage retail total**). E & J GALLO WINERY was the highest ranked named supplier by retail cases (**185,907**). The relative size of categories varies by channel, which means a product group can be important for warehouse sales without being equally important for retail stores.

**Figures:** `01_category_sales.png`, `02_top_suppliers.png`, `03_observed_months.png`. Because the extract lacks many months, the monthly plot should not be read as a continuous growth trend.

**Business interpretation.** Prioritize tracking demand and supplier reliability for high-volume categories, but review margins before making claims about which category is most profitable. Concentration in a few suppliers creates potential delivery exposure.

## Question 2 — Transfer-to-sales alignment
**Finding.** In January–November 2019, the aggregate difference between retail transfers and retail sales across beverage types was **-6,651 cases**. The individual category differences (positive = transfers exceed retail sales) were:

- WINE: -2,652 cases
- BEER: -2,248 cases
- NON-ALCOHOL: -1,768 cases
- LIQUOR: 17 cases
- KEGS: 0 cases

**Figure:** `04_transfer_sales_gap.png`.

**Business interpretation.** Persistent positive differences may warrant stock-level checks, while negative differences may merit replenishment review. But transfer-minus-sales is not an inventory balance: beginning inventory, breakage, returns, and timing are not provided. A gap alone is not proof of overstock, stockouts, or waste.

## Question 3 — Predicting next-month product retail sales
**Finding.** For **20,429** eligible item-month test observations in 2019 (current months September–October, outcomes October–November), the ridge regression achieved **MAE 4.22 cases**, **RMSE 18.73 cases**, and **R² 0.720**. The previous-month-sales baseline achieved **MAE 4.45**, **RMSE 19.03**, and **R² 0.711**.

**Interpretation.** The regression lowers mean absolute error relative to the naive forecast, suggesting some planning value under the restricted sample. Its RMSE is also lower. Prediction quality varies among product types. Figures `05_actual_vs_predicted.png` and `06_regression_error_by_category.png` show the tested predictions and category-specific error.

**Limitations.** The training and test periods are both from 2019, and products missing in the following month are excluded. The model is descriptive/predictive, not causal, and cannot guarantee performance in 2026 or other years. Its performance can be distorted by skewed quantities, unusual items, and extreme sales values.

## Recommendations
1. **Build a monthly category and supplier scorecard.** Review the high-volume categories and suppliers each month to find meaningful changes in case volume, not inferred revenue.
2. **Investigate large repeat transfer gaps.** Compare item-level transfer movements with retail sales, then validate against actual on-hand inventory before deciding whether to change order quantities.
3. **Use simple forecasts as benchmarks.** Evaluate next-month predictions against the prior-month baseline before trusting a more complicated forecasting model; test on additional consecutive periods when available.
4. **Improve available data.** Add stock-on-hand, pricing, promotions, lead times, and complete monthly historical coverage to make distribution and profitability recommendations more actionable.

## Conclusion
The dataset offers useful evidence on which beverage categories dominate case movements and where retail replenishment may need closer attention. Regression can be evaluated as a support tool, but its value depends on whether it beats a simpler baseline on later unseen months. The most important decisions require combining these movement records with inventory and financial data.

## Data source
Montgomery County Open Data, *Warehouse and Retail Sales*: https://data.montgomerycountymd.gov/d/v76h-r7br

## Submission note
The figures were generated from the supplied CSV for this GitHub package, rather than exported from an existing Tableau dashboard. Add your original Tableau dashboard or `.twbx` if your instructor expects Tableau-native evidence.
