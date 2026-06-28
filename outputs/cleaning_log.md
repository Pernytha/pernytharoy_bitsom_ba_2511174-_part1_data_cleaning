# Cleaning Log: Retail Order Data

## 1. List of Issues Found
* **Text Formatting:** Inconsistent casing and trailing/leading spaces in categorical fields (`region`, `segment`, `category`).
* **Date Integrity:** Significant volume of missing, malformed, or illogical dates (e.g., `ship_date` occurring before `order_date`).
* **Duplicates:** 64 duplicate `order_id` entries found.
* **Calculation Mismatches:** Discrepancies between raw `sales`/`profit` figures and values calculated using `unit_price`, `quantity`, and `discount`.
* **Business Rule Violations:** Instances of negative discounts, discounts exceeding 1.0 (100%), and various invalid order statuses (Cancelled, Failed, Returned).

## 2. Cleaning Actions Performed
* **Standardization:** Applied `TRIM()` and `PROPER()` to standardize all text fields.
* **Date Validation:** Standardized all dates to a uniform format; created a `shipping_delay_days` column.
* **Financial Recalculation:** Created `calculated_sales` and `calculated_profit` columns to override unreliable raw financial fields.
* **Duplicate Management:** Flagged duplicates for review using a `duplicate_check` column rather than deleting them, preserving the audit trail.
* **Imputation:** Filled missing `region` and `ship_mode` entries with "Unknown" to maintain data continuity in pivot reports.

## 3. Business Rules Applied 
| Rule Area                  | Action Taken                                                                 |
| :------------------------- | :--------------------------------------------------------------------------- |
| **Missing Region**         | Filled with "Unknown" and flagged in the data quality report.                |
| **Missing Ship Mode**      | Filled with "Unknown" and flagged in the data quality report.                |
| **Missing Discount**       | Treated as 0 (provided all other sales fields were valid).                   |
| **Negative Discount**      | Flagged as `INVALID` in the `discount_flag` column.                          |
| **Discount > 1.0**         | Flagged as `INVALID` (out of allowed range).                                 |
| **Cancelled Orders**       | Excluded from the "Final Completed Sales" summary via `sales_summary_status`.|
| **Failed Payments**        | Excluded from the "Final Completed Sales" summary.                           |
| **Returned Orders**        | Excluded from completed sales summary and reported separately                |
| **Ship Date < Order Date** | Flagged as `INVALID_SHIPPING` in `data_quality_flag`.                        |

## 4. Assumptions Made
* **Realized Revenue:** Assumed "Cancelled" and "Failed Payment" statuses represent $0 in realized revenue and are excluded from financial performance summaries.
* **Data Reliability:** Where raw cost-per-unit data was missing, profit was estimated based on historical margin trends for the respective sub-category.

## 5. Records Removed
* **Count:** 0
* **Rationale:** To maintain an absolute audit trail for management review, all records were retained. Issues were managed via flagging and segregation rather than deletion.

## 6. Records Flagged
* **Total Flagged:** 156 records.
* **Categories:** Includes `DATE ERROR`, `INVALID_SHIPPING`, `WARNING` (discount issues), and `DUPLICATE_ORDER_ID`.

## 7. Limitations of Cleaning Process
* **Historical Context:** Some missing geographic/shipping data could not be recovered and was marked as "Unknown," limiting regional granularity for those specific records.
* **Discrepancy Source:** The discrepancy between raw and calculated profit likely stems from missing cost/overhead data in the original internal export, which remains unrecoverable.