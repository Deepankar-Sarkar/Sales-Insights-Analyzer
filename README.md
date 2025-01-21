# Sales-Insights-Analyzer [Power BI | SQL]    
Dashboard Link - https://app.powerbi.com/view?r=eyJrIjoiNjVmYTc0MzYtYjBkNy00ZTk4LWI4MjMtOWU2MGY2ODgwYmM2IiwidCI6IjIwZTJjNWU1LTVhNDctNGM4ZC1hZTRhLWU2YjlhZGRhZWU2ZSJ9

---

AtliQ Hardware, a leading supplier of computer hardware and peripherals in India, is experiencing challenges in tracking and understanding its sales performance amidst a dynamic market. The company, headquartered in Delhi, operates across the country through regional offices.

The Challenge:

The Sales Director faces difficulties in:

* Tracking Sales Trends: Accurate and timely sales data is unavailable, hindering effective analysis of market trends and identifying growth opportunities.
* Regional Performance Monitoring: Relying on verbal reports from regional managers provides an incomplete and inconsistent picture of regional sales performance.
* Data-Driven Decision Making: The lack of a centralized data repository and insightful visualizations limits the ability to make informed decisions regarding sales strategies, product offerings, and resource allocation.

The Solution:

This project aims to develop a user-friendly Power BI dashboard that will:

- Centralize Sales Data: Gather and consolidate sales data from all regions into a single, accessible source.
- Visualize Key Performance Indicators (KPIs): Present key sales metrics such as total sales, regional sales breakdowns, product-wise sales, customer segmentation, and year-over-year growth trends through interactive charts and graphs.
- Enable Data-Driven Decision Making: Empower the Sales Director and regional managers with actionable insights to identify sales bottlenecks, optimize sales strategies, and drive revenue growth.

Expected Outcomes:

- Improved Sales Visibility: Enhanced understanding of sales performance across regions and product lines.
- Data-Driven Decision Making: Informed decisions regarding sales strategies, marketing campaigns, and resource allocation.
- Enhanced Sales Forecasting: More accurate sales predictions based on historical data and current market trends.
- Increased Efficiency: Streamlined reporting and data analysis processes, freeing up valuable time for strategic planning.
By leveraging the power of Power BI, AtliQ Hardware can transform its sales operations, gain a competitive edge, and achieve sustainable growth.


---

**Data Analysis Using MySQL:**

**Steps to Import Data:**
1. Download the SQL database dump file (db_dump.sql).
2. Import the data into MySQL Workbench for analysis.

**Observations and Cleaning Needs:**
- The `market` table contained incorrect values.
- The `transactions` table included negative and USD values that required conversion to INR.

**Key SQL Queries for Analysis:**
1. Find all customer records:
   ```sql
   SELECT * FROM sales.customers;
   ```
2. Total number of customers:
   ```sql
   SELECT COUNT(*) FROM sales.customers;
   ```
3. Transactions for Chennai market (Mark001):
   ```sql
   SELECT * FROM sales.transactions WHERE market_code='Mark001';
   ```
4. Distinct product codes sold in Chennai:
   ```sql
   SELECT DISTINCT product_code FROM sales.transactions WHERE market_code='Mark001';
   ```
5. To find transactions where currency is US dollars
     ```sql
      SELECT * from sales.transactions where currency="USD";`
     ```
6. Transactions :
   ```sql
   SELECT sales.transactions.*, sales.date.*
   FROM sales.transactions
   INNER JOIN sales.date
   ON sales.transactions.order_date = sales.date.date
   WHERE sales.date.year = 2020;
   ```
7. Total revenue :
   ```sql
   SELECT SUM(sales.transactions.sales_amount)
   FROM sales.transactions
   INNER JOIN sales.date
   ON sales.transactions.order_date = sales.date.date
   WHERE sales.date.year = 2020;
   ```

---

**Data Cleaning and ETL (Extract, Transform, Load):**

1. **Data Loading:**
   - Connect MySQL database to Power BI Desktop.
   - Load tables into Power BI to form a star schema for analysis.

2. **Data Transformation in Power Query:**
   - **Market Table:** Filter out rows with null or blank values.
   - **Transactions Table:**
     - Remove negative and zero values.
     - Convert USD amounts to INR using:
       ```
       = Table.AddColumn(#"Filtered Rows", "norm_sales_amount", each if [currency] = "USD" then [sales_amount]*75 else [sales_amount])
       ```
     - Remove duplicate currency values (e.g., USD vs. USD/r).

---

**Data Modeling:**
After cleaning and transforming the dataset, it was modeled into a structured schema to support robust analysis.

---

**Data Analysis (DAX):**

**Key Measures Used:**
1. **Profit Margin Percentage:**
   ```
   Profit Margin % = DIVIDE([Total Profit Margin], [Revenue], 0)
   ```
2. **Revenue Contribution:**
   ```
   Revenue Contribution % = DIVIDE([Revenue], CALCULATE([Revenue], ALL('sales products', 'sales customers', 'sales markets')))
   ```
3. **Revenue Last Year:**
   ```
   Revenue LY = CALCULATE([Revenue], SAMEPERIODLASTYEAR('sales date'[date]))
   ```
4. **Profit Target Difference:**
   ```
   Target Diff = [Profit Margin %] - 'Profit Target1'[Profit Target Value]
   ```

---

**Dashboard Development:**
Using Microsoft Power BI, we created an intuitive and interactive dashboard that:
- Visualizes key sales metrics.
- Highlights trends and regional performance.
- Supports real-time decision-making for the Sales Director and stakeholders.

**Outcome:**
The dashboard provided actionable insights, saved time, and enabled the company to make informed, data-driven decisions, improving overall efficiency and potentially raising revenue by at least 7% in the next quarter. 

---

