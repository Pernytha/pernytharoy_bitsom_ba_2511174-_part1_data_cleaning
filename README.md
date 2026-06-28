# Data Cleaning & Validation Report

## 1. Problem Summary
This project involved cleaning and validating a raw retail dataset of 932 entries. The primary objective was to transform inconsistent, duplicate, and fragmented raw data into a reliable, analysis-ready format for financial reporting and business intelligence.

## 2. Dataset Description
* **Total Records:** 932
* **Source:** Raw export from internal retail systems.
* **Key Fields:** Order ids, Order dates, ship date, customer id customer name,segment,region, state, city,category, subcategory,product name , ship mode,quantity, unit price, discount, sales,cost, profit, payment status, order status.

## 3. Tools Used
* **Microsoft Excel:** Data cleaning (Power Query, `TRIM`, `PROPER`), validation, and creation of pivot summaries.
* **Markdown:** Project documentation and cleaning log creation.

## 4. Cleaning Steps Performed
1. **Text Standardization:** Applied `TRIM` and `PROPER` functions to resolve leading/trailing spaces and case inconsistencies in category and region fields.
2. **Date Validation:** Standardized `order_date` and `ship_date`, and calculated `shipping_delay_days`. Flagged invalid or missing dates as `DATE ERROR`.
3. **Duplicate Management:** Used `duplicate_check` logic to identify and flag redundant records without deleting them, preserving the audit trail.
4. **Calculated Columns:** Generated standardized financial metrics (`calculated_sales`, `calculated_profit`, `profit_margin`) to ensure consistency.

## 5. Business Rules Applied 
| Rule Area | Action Taken |
| :--- | :--- |
| **Missing Region/Ship Mode** | Filled with "Unknown" and flagged in the data quality report. |
| **Missing Discount** | Treated as 0 (provided all other sales fields were valid). |
| **Negative/Invalid Discount** | Flagged as `INVALID` in the `discount_flag` column. |
| **Cancelled Orders** | Excluded from the "Final Completed Sales" summary. |
| **Failed Payments** | Excluded from the "Final Completed Sales" summary. |
| **Refunded Orders** | Excluded from completed sales summary and reported separately. |
| **Ship Date < Order Date** | Flagged as `INVALID_SHIPPING` in `data_quality_flag`. |

## 6. Summary of Data Quality Issues
* **Duplicates:** 64 duplicate Order ID records identified and flagged.
* **Shipping:** 76 records identified where the ship date preceded the order date.
* **Calculations:** 818 records showed mismatches between raw and calculated profit.
* **Categorical:** 27 records had missing regions; 23 had missing ship modes.

## 7. Summary of Final Pivot Reports
* **Regional Performance:** Sales and profit aggregation by region (North, South, East, West, Unknown).
* **Product Hierarchy:** Sales and profit by category and sub-category.
* **Operations:** Order counts by ship mode and shipping delay trends.
* **Trend Analysis:** Monthly sales trends for 2024–2025.
* **Segment Profitability:** Average profit margin analysis by customer segment.

## 8. Key Business Insights
* **Regional Disparity:** The South region leads in order volume, but profit margins remain highly sensitive to discount levels across all regions.
* **Operational Friction:** A significant number of orders are classified as `Returned` or `Cancelled`, suggesting a need to investigate product quality or logistics bottlenecks.

## 9. Assumptions and Limitations
* **Assumptions:** "Cancelled" and "Failed" status orders are assumed to contribute $0 to net realized revenue.
* **Limitations:** The cleaning process could not resolve all historical data missingness, requiring the use of "Unknown" placeholders for missing geographic and shipping fields.

## 10. Screenshots
* **Raw Data Preview:** `screenshots/raw_data_preview.png`
* **Cleaned Data Preview:** `screenshots/cleaned_data_preview.png`
* **Pivot Summary 1 (Regional Sales):** `screenshots/pivot_summary_1.png`
* **Pivot Summary 2 (Segment):** `screenshots/pivot_summary_2.png`