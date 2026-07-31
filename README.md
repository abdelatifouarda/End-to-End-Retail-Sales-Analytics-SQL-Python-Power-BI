# End-to-End-Retail-Sales-Analytics-SQL-Python-Power-BI
End-to-end Retail Sales Analytics project using PostgreSQL, SQL, Python, Machine Learning, and Power BI to transform transactional data into business insights and executive dashboards.



# Retail Sales Analytics — Ecuador Retail Chain

## Executive Summary
This project analyzes 4.5 years (2013–2017) of transactional sales data from a 54-store retail chain in Ecuador. The core analysis was built in Python (data integration, exploratory analysis, and predictive modeling), visualized through a 4-page Power BI executive dashboard (Star Schema data model), and independently cross-validated with a set of PostgreSQL business-question queries. All three tools converge on the same core figures, which are documented and reconciled throughout this README.

## Business Problem
A multi-store retail chain needs to understand where and when sales are strongest, which product categories drive revenue, and how promotions and holidays affect demand, in order to guide inventory, staffing, and marketing decisions across its store network.

## Business Objectives
- Consolidate sales, store, transaction, and holiday data into a unified analytical view.
- Identify the strongest and weakest stores, cities, and product categories.
- Quantify the relationship between promotions and sales.
- Understand true seasonal demand patterns.
- Build a predictive model for daily sales at the store/category level.

## Dataset
Sales transactions from Corporación Favorita's Ecuador retail chain (Kaggle "Store Sales — Time Series Forecasting" dataset): **3,000,888 sales records** across 54 stores and 33 product families, from January 1, 2013 to **August 15, 2017** (note: 2017 is a partial year, ending mid-August — this matters for any year-over-year comparison). Supplementary tables include store attributes (city, state, type, cluster), daily transaction counts per store, and a holiday/event calendar. No missing values or duplicates were found in the raw source files.

## Project Workflow
1. **Data Cleaning** — Verified data types, missing values, and duplicates across all four source files (sales, stores, transactions, holidays).
2. **Zero-Sales Investigation** — 31.3% of sales records are zero. Rather than treating this as a data quality issue, it was investigated across holiday type, store, and product family, and confirmed to reflect legitimate business patterns (low-demand categories like Books and Baby Care have structurally higher zero-sales rates) rather than missing or erroneous data.
3. **Data Integration** — Merged sales with store attributes, daily transactions, and holiday data using validated many-to-one joins, producing a single analytical table with no data loss or duplication.
4. **Exploratory Analysis** — Analyzed revenue by year, month, store, city, product family, store cluster, and holiday type; examined the relationship between promotions and sales, and between transactions and sales.
5. **Predictive Modeling** — Trained and compared Linear Regression, Random Forest, and XGBoost models to predict daily sales at the store/product-family level, using store number, product family, promotion count, and date components as features.
6. **Business Intelligence Dashboard** — Built a 4-page Power BI dashboard (Executive Summary, Store Performance, Product & Promotion Analysis, Time Intelligence) on a Star Schema data model with DAX time-intelligence measures.
7. **SQL Cross-Validation** — Loaded the dataset into PostgreSQL and independently reproduced the core KPIs (total sales, category concentration, top/bottom performers) using SQL business-question queries, to cross-check the Python and Power BI results from a third, independent angle.

## Technologies Used
- **Python**, **Pandas**, **NumPy**
- **Scikit-learn** (Linear Regression, Random Forest), **XGBoost**
- **Matplotlib**, **Seaborn**, **Plotly**
- **Microsoft Power BI** (Star Schema modeling, DAX)
- **PostgreSQL / SQL** (business-question queries, cross-validation)

## Results
- **Total sales**: **$1,073,644,952** (confirmed identically in Python and in the Power BI dashboard), with steady growth from $140M (2013) to $289M (2016); 2017 shows $194M through mid-August only, which is a partial-year figure, not a year-over-year decline.
- **Total transactions**: **≈ 141 million**, giving an average sale of **$7.59 per transaction** — both figures confirmed in Power BI after a DAX fix (see *Development Notes* below).
- **Category concentration**: Grocery I is the top category at roughly **36%** of total revenue, followed by Beverages (~23%) and Produce (~13%) — confirmed identically in Python and Power BI. The top 3 categories account for over 65% of total sales.
- **Geographic concentration**: Quito accounts for the largest share of revenue among all cities, consistent with it being the capital and having the highest-volume stores (top stores 44, 45, 47 are all in Quito).
- **Seasonality**: When properly normalized for the fact that 2017 only contributes partial-year data (Sept–Dec have one fewer year of history than Jan–Aug), **December is the strongest month on average**, followed by July, November, and October — February and August are the softest months.
- **Predictive modeling**: XGBoost achieved the best performance (R² = 0.888, MAE = 104.7), ahead of Random Forest (R² = 0.845) and Linear Regression (R² = 0.535), using only store, product family, promotion, and date features.
- **SQL cross-validation**: An independent PostgreSQL calculation of total sales returned $1,073,644,950 — within $3 of the Python total ($1,073,644,952) once column precision was corrected (see *Development Notes*), confirming the result across all three tools.

## Business Insights
- Sales are highly concentrated in a small number of categories (Grocery I, Beverages, Produce) and a small number of high-volume Quito stores — both are natural focal points for inventory and staffing priority.
- December is the genuine seasonal peak once the data's partial final year is properly accounted for — earlier month-over-month comparisons that named July as the peak did not correct for this and should be revised.
- Promoted items show a measurable sales lift, and promotional intensity varies meaningfully by category, suggesting some categories convert promotional spend into sales more efficiently than others.
- Zero-sales records are concentrated in inherently low-demand categories, not a data or operational problem.

## Business Recommendations
- Prioritize inventory and staffing planning around December (the true seasonal peak) and the top 3 product categories, which together drive the majority of revenue.
- Re-evaluate promotional budget allocation by category efficiency (sales generated per unit of promotion), rather than by promotion volume alone.
- Use the Quito store cluster as a template for operational best practices, given its consistent high performance, while investigating root causes at consistently underperforming stores.
- Extend the sales-prediction model with store type, cluster, holiday type, and transaction-count features (already collected during data integration but not yet used in the model) to improve forecast accuracy.

## Author
Data Analyst — Business Intelligence & Machine Learning.
