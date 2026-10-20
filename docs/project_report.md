# Project Report: Brazilian E-Commerce Sales & Customer Insights

**Project stage:** Data preparation and database setup  
**Dataset:** Brazilian E-Commerce Public Dataset by Olist  
**Tools used:** Python, pandas, Jupyter Notebook, PostgreSQL, psycopg, SQLAlchemy

## 1. Introduction

The purpose of this project is to analyse Brazilian e-commerce data and identify patterns in sales, order delivery, customer experience, and seller performance.

The work begins with preparing the orders dataset so that later analysis is based on data that has been inspected and understood. Rather than treating data cleaning as a purely technical task, I have reviewed missing timestamps in the context of order status and considered whether missing values can be reliably recovered.

At this stage, the project is focused on data preparation. Business findings and recommendations will be developed after the data has been validated and the SQL analysis has been completed.

## 2. Dataset Overview

The orders dataset contains 99,441 rows and eight columns after the initial preparation work.

| Column | Description |
|---|---|
| `order_id` | Identifier for an order |
| `customer_id` | Identifier for the customer associated with the order |
| `order_status` | Recorded status of the order |
| `order_purchase_timestamp` | Time the order was placed |
| `order_approved_at` | Recorded approval timestamp |
| `order_delivered_carrier_date` | Recorded date the order was handed to the carrier |
| `order_delivered_customer_date` | Recorded customer delivery timestamp |
| `order_estimated_delivery_date` | Estimated delivery date |

The timestamp columns are important for analysing order timelines, delivery performance, and differences between estimated and actual delivery dates.

## 3. Data Preparation

I inspected the dataset in Jupyter Notebook, reviewed its structure and data types, and examined missing values in the approval and delivery timestamp columns.

The main cleaning decisions so far are described below.

### 3.1 Missing approval timestamps

A total of 146 missing approval timestamps were identified during the review. Fourteen of these belonged to orders marked as delivered.

For those 14 orders, an estimated approval timestamp was calculated using the purchase timestamp and the average observed time between purchase and approval, rounded to the nearest second.

This is an imputation based on an average, not a recovery of the original event time. The distinction matters because estimated timestamps can affect calculations involving approval delays. These records should be flagged where possible, and the impact of the estimates should be checked before drawing conclusions from approval-time metrics.

### 3.2 Missing carrier handoff timestamps

There were 1,783 missing carrier handoff timestamps. The missing values occurred across several order statuses, including unavailable, canceled, invoiced, processing, created, approved, and delivered.

These values were retained as missing because there was not enough evidence to establish the actual carrier handoff time. Filling them with estimated delivery dates or unrelated timestamps would create information that the dataset does not confirm.

### 3.3 Missing customer delivery timestamps

There were 2,965 missing customer delivery timestamps. Most were associated with orders that had not reached a recorded delivered status, although eight rows marked as delivered also lacked this timestamp.

Of those eight rows, seven had a recorded carrier handoff timestamp, while one had neither a carrier handoff timestamp nor a customer delivery timestamp. Estimated delivery dates were available, but they did not establish the actual delivery date.

For this reason, the eight missing customer delivery timestamps were left missing. The estimated date will not be treated as an actual delivery event.

## 4. PostgreSQL Setup

A PostgreSQL database named `olist_ecommerce_analysis` was configured for the project. The connection was tested successfully using both `psycopg` and SQLAlchemy.

The intended table location is `raw.olist_orders`, within the `raw` schema. The cleaned dataset has also been exported locally to `data/processed/cleaned.csv`.

The database load must be verified separately by querying the table row count and checking the uniqueness of `order_id`. A successful database connection alone does not confirm that the data has been loaded correctly.

## 5. Data Quality Checks Still Required

Before starting the main business analysis, the following checks remain important:

- Confirm that the PostgreSQL table contains the expected 99,441 rows.
- Check whether `order_id` is unique.
- Review timestamp ordering for impossible or suspicious sequences.
- Recheck missing values and order statuses after the cleaning decisions.
- Keep track of the approval timestamps that were estimated.
- Confirm that the cleaned CSV and database table represent the same version of the dataset.

These checks will help reduce the risk of reporting misleading sales or delivery metrics.

## 6. Planned Analysis

Once data validation is complete, the project will examine four areas.

**Sales performance:** monthly order trends, average order value, product category performance, and seasonality.

**Delivery and customer satisfaction:** differences between estimated and actual delivery dates, delivery delays, on-time delivery rates, and review scores.

**Customer behaviour:** repeat purchasing, purchase frequency, spending patterns, and first-purchase cohorts.

**Product and seller performance:** category performance, seller activity, reviews, and freight costs relative to item prices.

The findings will be supported by SQL queries and, where useful, Python analysis. Power BI will be used later to present the results in a dashboard.

## 7. Current Limitations

This report documents preparation work rather than completed business analysis. No conclusions about sales growth, customer retention, delivery performance, or seller rankings have been established yet.

The imputed approval timestamps are estimates and need to be treated cautiously. Missing carrier and delivery timestamps also limit which records can be used for specific delivery metrics. Any future analysis should state the relevant denominator and explain how records with missing dates were handled.

## 8. Conclusion and Next Steps

The initial orders dataset has been inspected, selected missing timestamp cases have been reviewed, and the cleaned data has been saved locally. PostgreSQL connectivity has also been confirmed.

The next priority is to verify the database load and complete data-quality checks. After that, the project can move into SQL-based business analysis, followed by Power BI visualisation and evidence-based recommendations.

The report will be updated as the project progresses, so the final version reflects the work actually completed and the results supported by the data.
