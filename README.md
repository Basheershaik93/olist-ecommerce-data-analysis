# Brazilian E-Commerce Sales & Customer Insights

## Project Overview

This project looks at order and delivery data from the Brazilian e-commerce marketplace Olist. The aim is to understand how the business performs, how reliably orders reach customers, and what the data can tell us about customer experience and seller performance.

I am building the project step by step, starting with data preparation and then moving into SQL analysis and visualisation. The goal is to turn raw e-commerce data into clear findings that could help a business understand its performance and identify areas for improvement.

## Objectives

The analysis will focus on four areas:

- **Sales performance:** order trends over time, average order value, popular product categories, and seasonal patterns.
- **Delivery and customer satisfaction:** delivery delays, differences between estimated and actual delivery dates, and how delivery experience relates to review scores.
- **Customer behaviour:** repeat purchasing, purchase frequency, spending patterns, and first-purchase cohorts.
- **Product and seller performance:** category performance, seller activity, customer reviews, and freight costs compared with item prices.

These are the planned analysis areas. Findings and recommendations will be added as the analysis is completed and validated.

## Tools and Technologies

- **Python and pandas** — data inspection, cleaning, and preparation.
- **Jupyter Notebook** — documenting the analysis and cleaning decisions.
- **PostgreSQL** — storing the prepared orders data and carrying out SQL analysis.
- **Power BI** — planned for presenting the final findings through an interactive dashboard.
- **Git and GitHub** — tracking project changes and maintaining a public portfolio.

## Dataset

The project uses the Brazilian E-Commerce Public Dataset by Olist. The dataset contains information about orders placed through a Brazilian online marketplace, including order status, purchase timestamps, delivery timestamps, and estimated delivery dates.

The first dataset being prepared for analysis is the orders table.

After the initial cleaning work, the orders dataset contains **99,441 rows and 8 columns**. The columns cover order ID, customer ID, order status, purchase time, approval time, carrier handoff time, customer delivery time, and estimated delivery time.

The source dataset is available through [Kaggle's Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Please refer to the dataset page for its source information and terms.

## Work Completed So Far

### 1. Initial data preparation

- Inspected the orders dataset in Jupyter Notebook.
- Reviewed column names, data types, and missing values.
- Prepared the order timestamps for date-based analysis.
- Investigated missing values in approval and delivery-related timestamps.

### 2. Missing-value review

Missing timestamps were reviewed alongside order status rather than being filled automatically.

- **Approval timestamp:** missing approval values were investigated. For 14 delivered orders, an estimate was made using the purchase timestamp and the average observed purchase-to-approval time. These values are estimates, not confirmed approval times.
- **Carrier handoff timestamp:** missing values were retained where an actual handoff time could not be established.
- **Customer delivery timestamp:** eight orders marked as delivered had no recorded customer delivery timestamp. These values were left missing because the estimated delivery date does not confirm when delivery actually occurred.

These choices are documented as part of the data preparation process. Further validation and sensitivity checks will be carried out before using affected timestamps in performance metrics.

### 3. PostgreSQL setup

- Connected to the `olist_ecommerce_analysis` database using `psycopg` and SQLAlchemy.
- Prepared the `raw` schema for the orders table.
- Saved the cleaned DataFrame locally as `data/processed/cleaned.csv`.

The orders table load and row-count verification should be recorded as complete once the database query confirms that the expected data has been loaded successfully.

## Project Structure

```text
olist-ecommerce-data-analysis/
├── notebooks/
│   └── 01_orders_data_cleaning.ipynb
├── docs/
│   ├── project_report.md
│   └── data_cleaning_log.md
├── data/
│   ├── raw/             # Local files; not committed
│   └── processed/       # Local files; not committed
├── .gitignore
└── README.md
```

The exact contents of the folders may change as the project develops. Local datasets and environment credentials are excluded from version control.

## Current Status

**Completed:** initial orders dataset inspection, timestamp review, documented decisions for selected missing timestamps, local cleaned CSV export, and successful PostgreSQL connection.

**In progress:** verifying the database load and completing data-quality checks.

**Planned:** SQL business analysis, customer and seller analysis, visualisation in Power BI, and a final set of evidence-based findings and recommendations.

## Next Steps

1. Verify the row count and order ID uniqueness in PostgreSQL.
2. Check for duplicate order IDs, inconsistent statuses, and timestamp issues.
3. Build SQL queries to answer the business questions.
4. Analyse sales, delivery performance, customer behaviour, and seller performance.
5. Build a Power BI dashboard and document the final findings.

## Notes

This is a work-in-progress portfolio project. Conclusions and recommendations will be added after the analysis has been completed and checked. The repository is intended to show not only the final results, but also the decisions and validation steps behind the work.
