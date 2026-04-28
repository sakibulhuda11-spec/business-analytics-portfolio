# Superstore Profitability Analytics
## Multi-Tool Business Intelligence Pipeline

Author: Sakibul Huda
Date: April 2026  
Tools: Python · PostgreSQL · Excel · Power BI



### Project Overview

I analyzed 9,994 retail transactions from a US-based store (2014–2017) to uncover why the company earns only a 12.5% profit margin despite $2.3M in revenue. I ended up identifying unprofitable product lines, destructive discount practices, and loss-making states, then delivered actionable pricing and operational recommendations backed by data.

This project demonstrates an end-to-end analytics pipeline using four industry-standard tools: Python for data cleaning and visualization, PostgreSQL for database querying, Excel for stakeholder reporting, and Power BI for interactive dashboards.



### Key Findings
## What the Data Told Me

When I started this analysis, I expected to find one or two issues — but the picture turned out to be more interesting.

The clearest problem was discounts. Any order with a discount above 20% lost money, and together these orders cost the company around $135K in profit. That was the biggest single drain I found.

I also noticed that three sub-categories — Tables, Bookcases, and Supplies — are unprofitable on their own, accounting for about $22K in losses. Tables alone lost $17,725.

Geography mattered too. Ten states were operating at a net loss, with Texas (-$25,729), Ohio (-$16,971), and Pennsylvania (-$15,560) leading the way.

On the positive side, the Home Office segment stood out as the most valuable per customer, even though it's the smallest segment — that felt like a growth lever worth pulling. And overall sales grew about 20% year over year from 2014 to 2017, which told me the business itself is healthy. The fix needed isn't more sales; it's better margins.



### Recommendations

1. Cap discounts at 20% company-wide — the data shows a clear profitability threshold
2. Review pricing strategy for Tables, Bookcases, and Supplies — heavy discounting on high-cost items drives losses
3. Investigate operations in Texas, Ohio, and Pennsylvania — regional factors like competition or shipping costs may be at play
4. Invest in high-margin sub-categories — Copiers (37% margin), Labels (44%), Paper (43%) deserve more focus
5. Grow the Home Office customer base — highest value per customer of all three segments



### Tools & Skills Demonstrated

## Tools & Skills Used

For this project, I worked across four tools:

- Python (pandas, numpy, matplotlib, seaborn) — Cleaned the data, engineered new features, ran the EDA, and built six visualizations including a correlation analysis.
- PostgreSQL — Wrote seven analytical queries using CTEs, window functions (LAG, RANK, SUM OVER), and CASE WHEN logic, then connected to the database from Python via psycopg2.
- Excel — Exported a four-sheet workbook with category, state, and discount summaries, formatted for stakeholder review.
- Power BI — Built a three-page interactive dashboard with KPI cards, trend charts, a map, and slicers, using custom DAX measures for cross-filtering.


### Methodology

Phase 1 — Python (EDA & Visualization)
- Loaded and cleaned 9,994 transactions (no missing values, no duplicates)
- Converted dates, engineered 8 new features (year, quarter, profit margin, discount buckets, etc.)
- Calculated KPIs: total revenue, profit, margin, order count, customer count
- Created 6 publication-quality charts: quarterly trends, sub-category profitability, discount impact scatter plot, state comparison, segment analysis, correlation heatmap

Phase 2 — PostgreSQL (SQL Analysis)
- Connected Python to PostgreSQL using psycopg2
- Loaded cleaned data into `superstore_analytics` database using bulk COPY method
- Wrote 7 analytical queries demonstrating:
  - KPI aggregation with CASE WHEN
  - Year-over-year growth using LAG() window function
  - Sub-category ranking using RANK() with PARTITION BY
  - Customer tier segmentation using chained CTEs and CASE WHEN
  - Year-to-date cumulative profit using SUM() OVER()
  - Top loss-making products using GROUP BY with HAVING
  - Discount impact analysis using CASE WHEN bucketing

Phase 3 — Excel
- Exported 4-sheet workbook: Category Summary, Sub-Category Detail, State Performance, Discount Impact
- Formatted for non-technical stakeholders

Phase 4 — Power BI
- Imported cleaned CSV and created 7 DAX measures (Total Sales, Total Profit, Profit Margin, Total Orders, Total Customers, Avg Order Value, Profitable Order %)
- Built 3 dashboard pages:
  - Executive Overview: KPI cards + quarterly trend line + category donut chart
  - Product Analysis: Sub-category profit bar chart + detail table + category slicer
  - Geographic Analysis: State map + region comparison bars


### Dashboard Preview

![Executive Overview](powerbi/Dashboard_Screenshots/page1_overview.png)
![Product Analysis](powerbi/Dashboard_Screenshots/page2_products.png)
![Geographic Analysis](powerbi/Dashboard_Screenshots/page3_geography.png)



### Repository Structure

```
superstore-profitability-analytics/
│
├── README.md
├── requirements.txt
│
├── data/
│   └── Sample - Superstore.csv
│
├── notebooks/
│   └── Superstore_Project_FINAL.ipynb
│
├── sql/
│   └── superstore_queries.sql
│
├── outputs/
│   ├── Superstore_Analysis.xlsx
│   ├── Superstore_Cleaned.csv
│   ├── 01_quarterly_trends.png
│   ├── 02_subcategory_profit.png
│   ├── 03_discount_analysis.png
│   ├── 04_state_profitability.png
│   ├── 05_segment_analysis.png
│   └── 06_correlation_matrix.png
│
└── powerbi/
    ├── Superstore_Dashboard.pbix
    └── Dashboard_Screenshots/
        ├── page1_overview.png
        ├── page2_products.png
        └── page3_geography.png
```

---

- How to Run This Project

Prerequisites:
- Python 3.12+ with pip
- PostgreSQL with pgAdmin 4
- Power BI Desktop (Windows)

Step 1: Install dependencies
```bash
pip install pandas numpy matplotlib seaborn openpyxl psycopg2-binary sqlalchemy
```

Step 2: Run the notebook
```bash
cd notebooks/
jupyter notebook Superstore_Project_FINAL.ipynb
```
Run all cells sequentially (Shift+Enter). The notebook will:
- Load and clean the data
- Generate all charts (saved to outputs/)
- Connect to PostgreSQL and run SQL queries
- Export the Excel workbook

Step 3: PostgreSQL setup
- Open pgAdmin 4 → create database `superstore_analytics`
- Update the `DB_PASSWORD` variable in the notebook
- The notebook uploads data and runs all queries automatically

Step 4: Power BI
- Open Power BI Desktop → Get Data → Text/CSV → select `Superstore_Cleaned.csv`
- Create the 7 DAX measures listed in the notebook
- Build the 3 dashboard pages



### Dataset

Source: [Kaggle — Superstore Sales Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)  
Size: 9,994 rows × 21 columns  
Period: January 2014 – December 2017  
Geography: United States (49 states)



### Author

Sakibul Huda  
[LinkedIn Profile](https://www.linkedin.com/in/sakibul-huda-376b62371/) · [Email](mailto:sakibulhuda11@gmail.com)
