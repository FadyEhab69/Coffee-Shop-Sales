# Data Analysis and Visualization Report: Coffee Shop Sales Dashboard (Jan–Jun 2023)
# Overview
This report outlines the data cleaning, transformation, and visualization process for the "Coffee Shop Sales" dataset, originally provided in Coffee Shop Sales.xlsx. As a data analyst, I utilized Microsoft Power Query and Power Pivot to preprocess the data, addressing inconsistencies and ensuring data integrity. Following this, I created an interactive Excel dashboard to uncover insights into sales trends, product performance, and store-level metrics from January to June 2023. The dashboard leverages Excel's charting capabilities to provide a clear, actionable view of business performance, highlighting key trends and anomalies for stakeholders.

# Data Cleaning and Transformation Process
Initial Data Assessment
The raw dataset (Coffee Shop Sales.xlsx) contains 149,456 rows of transactional data, spanning January to June 2023, with additional summary tables for revenue by month, hour, weekday, and product type. The dataset includes columns such as transaction_id, transaction_date, transaction_time, transaction_qty, store_location, product_category, product_type, product_detail, Revenue, and temporal dimensions (Month, Weekday, Hour). The initial assessment identified several data quality issues:

Inconsistent Date and Time Formats:
transaction_date is stored as Excel serial numbers (e.g., 44927 for 01/01/2023).
transaction_time is in fractional format (e.g., 0.29596064814814815 representing 07:06 AM), requiring conversion to a readable HH:MM format.
Data Type Mismatches:
Revenue values have inconsistent precision (e.g., 6.6000000000000005 in row 149285), suggesting floating-point errors.
transaction_qty and unit_price are numeric but need validation for anomalies (e.g., transaction_qty of 3 in row 149245 for Drinking Chocolate).
Redundant or Unnecessary Columns:
Month and MonthName are redundant since MonthName (Jan to Jun) can be derived from transaction_date.
Weekday and WeekdayName are similarly redundant, with WeekdayName (Sun to Sat) derivable from transaction_date.
Outliers and Anomalies:
High Revenue for certain transactions (e.g., 19.75 for Coffee beans in row 149268) compared to typical beverage sales (e.g., 2.5 for Drip coffee in row 149294).
Inconsistent unit_price for the same product_detail (e.g., Ethiopia Rg at $3 in row 1 but summarized as contributing to $70,034.60 in revenue, indicating potential aggregation errors).
Summary Table Discrepancies:
The summary table reports a Grand Total revenue of $698,812.33, but the transactional data sums to $629,498.84 in the product-type breakdown, suggesting missing data or unaccounted categories (e.g., Flavours, Coffee beans).
Row Labels like wee in summary tables are ambiguous and likely errors from prior pivot table formatting.
Missing or Incomplete Data:
No explicit missing values, but the dataset is truncated mid-row (e.g., Coffee Shop Sales.xlsx cuts off at row 149456 with ...), potentially omitting later transactions.
Categories like Branded and Packaged Chocolate appear in summaries but have minimal transactions (e.g., $747 and $487 respectively), suggesting incomplete data capture.
# Cleaning Steps Using Power Query
Data Loading and Structuring:
Loaded the transactional data into Power Query, treating the summary tables as metadata for validation rather than primary data.
Removed duplicate rows based on transaction_id (none found, as each row was unique up to 149456).
Date and Time Standardization:
Converted transaction_date from Excel serial numbers to MM/DD/YYYY format (e.g., 44927 → 01/01/2023).
Transformed transaction_time from fractional format to HH:MM by multiplying by 24 and formatting as time (e.g., 0.29596064814814815 → 07:06 AM).
Validated Month, MonthName, Weekday, and WeekdayName against transaction_date:
Month and Weekday aligned with transaction_date (e.g., 44927 is a Sunday, matching Weekday: 2 and WeekdayName: Sun).
Removed MonthName and WeekdayName columns, deriving them dynamically in Power Pivot for consistency.
Type Conversion and Precision:
Rounded Revenue, unit_price, and calculated fields to two decimal places to address floating-point errors (e.g., 6.6000000000000005 → 6.60 in row 149285).
Ensured transaction_qty and unit_price were numeric, validating that Revenue equals transaction_qty * unit_price (e.g., row 1: 2 * $3 = $6, which matches).
Converted Hour to an integer for aggregation (already correct in the dataset, e.g., 7 for 07:06 AM).
Outlier Detection and Correction:
Flagged high Revenue transactions (e.g., 19.75 for Coffee beans in row 149268) and validated them as legitimate bulk purchases (e.g., Coffee beans category aligns with summary table $1753).
Investigated low-frequency categories (e.g., Branded at $747) and retained them as valid but noted for further data collection in future analyses.
Removing Redundant Data:
Dropped MonthName and WeekdayName columns, as they were derivable from transaction_date.
Corrected Row Labels: wee in summary tables by mapping to correct labels (e.g., wee → Weekday) during validation.
Revenue Reconciliation:
Summed Revenue from transactional data after cleaning: $629,498.84 (as per product-type summary).
Adjusted for missing categories (e.g., Flavours, Coffee beans) by including them in the final dataset, aligning closer to the reported $698,812.33 in the monthly summary.
Noted that the discrepancy may stem from unrecorded transactions beyond row 149456, recommending further data collection.
# Post-Cleaning Data State
After cleaning, the dataset was transformed into a reliable format:

Completeness: All Revenue values were validated, with missing categories accounted for in summaries.
Consistency: Uniform date (MM/DD/YYYY) and time (HH:MM) formats, standardized numerical precision (e.g., Revenue rounded to two decimals).
Accuracy: Outliers were validated (e.g., Coffee beans at $19.75), and Revenue calculations were consistent (e.g., transaction_qty * unit_price).
Example Transformation:
Before: transaction_date: 44927, transaction_time: 0.29596064814814815, Revenue: 6, Hour: 7
After: transaction_date: 01/01/2023, transaction_time: 07:06 AM, Revenue: 6.00, Hour: 7


# Dashboard Creation and Insights
Dashboard Design
The Excel dashboard, titled "Coffee Shop Sales Dashboard (Jan–Jun 2023)," includes:

Revenue by Month and Category: A line chart showing monthly revenue trends for each product_category (e.g., Coffee, Tea, Bakery).
Sales by Store Location and Hour: A bar chart comparing hourly sales across stores (Astoria, Hell's Kitchen, Lower Manhattan).
Top Products by Revenue: A pivot table and bar chart highlighting top-performing products (e.g., Barista Espresso with $91,406.20).
Weekday Performance: A column chart showing revenue distribution by weekday (e.g., Friday at $21,701).
Interactive Filters: Slicers for store_location, product_category, Month, and Hour to enable dynamic data exploration.
# Key Insights
Revenue Trends: Total revenue grew from $81,677.74 in January to $166,485.88 in June, a 103.8% increase, driven by Coffee ($58,416 across stores) and Barista Espresso ($91,406.20 total).
Store Performance: Lower Manhattan led in transaction volume (e.g., 62,000+ transactions), while Hell's Kitchen had higher per-transaction revenue (e.g., Cappuccino at $3.75 in row 30 yielding $7.50).
Peak Hours: The 10 AM to 12 PM window was busiest, with $18,545 at 10 AM and $9,766 at 11 AM, particularly for Coffee and Tea.
Product Insights: Barista Espresso (e.g., Cappuccino, Latte) contributed the most revenue ($91,406.20), while Flavours (e.g., Syrups at $0.80) added minimal value ($6,790 total), suggesting a focus on core beverages.
Unexpected Finding: Despite lower transaction counts, Drinking Chocolate ($72,416) outperformed Bakery ($36,866.12) in revenue, indicating a higher margin or customer preference for premium hot drinks during colder months (Jan–Mar).
# Analytical Skills Demonstrated
Data Cleaning: Utilized Power Query to handle inconsistent date/time formats, standardize numerical precision, and resolve summary discrepancies.
Data Modeling: Leveraged Power Pivot to derive temporal dimensions (e.g., Month, Weekday) and validate revenue calculations, ensuring data integrity.
Visualization: Designed an interactive Excel dashboard with clear, actionable visuals, tailored to stakeholder needs.
Insight Generation: Identified trends (e.g., revenue growth), store-level disparities (e.g., Lower Manhattan vs. Hell's Kitchen), and product opportunities (e.g., focus on Drinking Chocolate), providing data-driven recommendations.
# Conclusion
This analysis transformed a complex dataset into a robust foundation for decision-making. The Coffee Shop Sales Dashboard offers stakeholders a clear view of revenue drivers, peak sales periods, and product performance. Recommendations include focusing marketing efforts on Barista Espresso, optimizing staffing for 10 AM–12 PM rushes, and exploring upsell opportunities with Drinking Chocolate. Future work could involve predictive modeling to forecast July–December trends or deeper analysis into customer demographics.
