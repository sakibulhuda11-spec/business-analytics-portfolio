# Dynamic Pricing Optimization for E-commerce
## Using Regression Analysis & Causal Inference

Author: Sakibul Huda  
Date: April 2026  
Tools: Python · statsmodels · PostgreSQL · Excel



## Project Overview

This project answers a question every retailer eventually faces which is - if we change our prices, what actually happens to revenue? Using 541,909 transactions from a UK-based online retailer (Dec 2010 – Dec 2011), I estimated price elasticity of demand, tested whether promotions truly drive incremental sales, and ran counterfactual simulations to identify the revenue-maximizing pricing strategy.

I deliberately used classical statistical methods like multiple regression, hypothesis testing, and causal inference rather than black-box machine learning. The goal wasn't to predict the future; it was to understand the relationships in the data well enough to make defensible pricing recommendations with statistical evidence behind them.



## The Business Problem

I framed the project around four questions a real pricing analyst would face:

1. How sensitive are customers to price changes?
2. Do promotions actually increase revenue, or just cannibalize margins?
3. Should pricing differ by season?
4. What is the optimal pricing strategy to maximize revenue?

Every answer I produced comes with confidence intervals, p-values, and a clear business interpretation not just a number.



## Dataset

I used the UCI Online Retail Dataset, which contains 541,909 transactions from a UK-based online retailer between December 2010 and December 2011. After cleaning (removing cancelled orders, missing customer IDs, and extreme outliers), I aggregated the data to the product-week level — a standard approach in pricing analytics that smooths out transactional noise while preserving meaningful price variation.

Source: [UCI Machine Learning Repository — Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/online+retail)

---

## What I Found

After running the full regression with robust standard errors and validating the model assumptions, the results told a clear story:

Price elasticity of demand was statistically significant, meaning customers did respond meaningfully to price changes. The exact elasticity estimate (with its 95% confidence interval) is documented in the notebook's regression output. This single number drives nearly every other recommendation in the project.

Promotions had a measurable effect on quantity sold, but I was careful not to confuse "more units sold" with "more profit earned." Whether a promotion is worth running depends on whether the volume lift compensates for the margin loss and the elasticity estimate gives a way to calculate that break-even point.

Seasonality mattered a lot. Q4 (especially October–November) showed dramatically higher demand even after controlling for price. This is the classic holiday pre-ordering pattern in B2B retail, and it has direct implications for when to discount and when to hold prices firm.

The counterfactual simulations were the most useful output for stakeholders. By running the regression coefficients through different pricing scenarios, I could show exactly what the model predicts would happen to revenue at price changes of -20%, -10%, +10%, and so on. That's the kind of output a pricing manager can actually act on.



## Methodology

I worked through the project in seven stages, each building on the last.

Stage 1 — Exploratory Data Analysis. 
I started by understanding the data: distributions, correlations, time trends, and seasonality patterns. The price and quantity variables were heavily right-skewed, which immediately told me I'd need log transformations for the regression.

Stage 2 — Feature Engineering. 
I aggregated the raw transactions to the product-week level, then engineered features that captured what I actually wanted to study: log-transformed price and quantity (so the coefficient becomes elasticity directly), a promotion flag (defined as a >10% week-over-week price drop), seasonal dummies, peak-season indicators, and interaction terms to test whether price sensitivity changes during promotions.

Stage 3 — OLS Regression. 
I estimated a log-log demand model using `statsmodels`. The base model produced an elasticity estimate, and the full model added interaction terms to test whether the price-quantity relationship shifts during promotions or peak season. I used heteroscedasticity-consistent (HC1) robust standard errors throughout, which is best practice in applied econometrics.

Stage 4 — Diagnostics. 
A regression is only as trustworthy as its assumptions, so I tested all four:
- Multicollinearity using Variance Inflation Factors (VIF)
- Heteroscedasticity using the Breusch-Pagan test
- Normality of residuals using QQ-plots and Shapiro-Wilk
- Influential outliers using Cook's Distance

Stage 5 — Hypothesis Testing. 
I formally tested whether each coefficient was statistically significant, reported p-values and confidence intervals, and confirmed the model was jointly significant via the F-test.

Stage 6 — Causal Inference & Counterfactuals. 
This is where the analysis becomes useful for business decisions. I discussed the limitations honestly — we can't claim strict causation without randomized experiments — and then ran counterfactual simulations to estimate revenue under different pricing strategies.

Stage 7 — Recommendations. 
I translated the statistical results into specific, actionable pricing strategy recommendations and exported them to Excel for stakeholder review.



## Tools & Skills Used

I deliberately built this project around statistical rigor rather than predictive accuracy.

I used Python with `pandas` and `numpy` for data manipulation, and `matplotlib` and `seaborn` for visualization. The core analytical work was done in `statsmodels`, specifically the formula API (`smf.ols`), which gives R-style regression syntax with full statistical output including coefficient tests, confidence intervals, and diagnostic statistics. For the diagnostic tests, I used `variance_inflation_factor` for multicollinearity, `het_breuschpagan` for heteroscedasticity, and `scipy.stats` for the normality tests.

For exporting results to stakeholders, I used `openpyxl` to write a multi-sheet Excel workbook containing the regression output, revenue scenarios, and VIF diagnostics — formatted so a non-technical reader could pick it up and follow along.

I chose `statsmodels` over `scikit-learn` deliberately. For a pricing analysis, statistical interpretability matters more than prediction accuracy as we need p-values, confidence intervals, and diagnostic tests to defend a pricing recommendation, and `statsmodels` gives you all of that out of the box.


## Repository Structure

dynamic-pricing-analysis/
├── README.md
├── requirements.txt
├── Dynamic_Pricing_Analysis_Project.ipynb   ← Main analysis notebook
├── 01_distributions.png                     ← Variable distributions
├── 02_time_series.png                       ← Daily revenue, price, quantity trends
├── 03_correlation_matrix.png                ← Correlation heatmap
├── 04_residual_diagnostics.png              ← QQ-plot & residual histogram
├── 05_cooks_distance.png                    ← Influential observations check
├── 06_actual_vs_predicted.png               ← Model fit visualization
├── 07_elasticity_curves.png                 ← Demand & revenue curves
├── Regression_Results.csv                   ← Coefficient estimates with CIs
├── Revenue_Scenarios.csv                    ← Counterfactual simulations
└── VIF_Diagnostics.csv                      ← Multicollinearity check

## How to Run This Project

Prerequisites:
- Python 3.10 or newer
- Jupyter Notebook or VS Code with the Jupyter extension

Step 1 — Install dependencies:
```bash
pip install -r requirements.txt
```

Step 2 — Open the notebook:
```bash
jupyter notebook Dynamic_Pricing_Analysis_Project.ipynb
```

Step 3 — Run all cells. The notebook will:
- Download the UCI Online Retail dataset directly from the source
- Clean and aggregate the data to the product-week level
- Run the full regression analysis with diagnostics
- Generate all seven visualizations (saved as PNGs)
- Export the results to CSV files

The full pipeline runs in about 2–3 minutes on a standard laptop.



## Limitations & Honest Caveats

I want to be transparent about what this analysis can and can't tell us.

The estimates show association, not strict causation. To make hard causal claims about pricing, we'd need randomized A/B tests or instrumental variables neither of which were available in this dataset. The model also doesn't observe competitor pricing, marketing spend, or product lifecycle changes, all of which could confound the price-quantity relationship.

The data is from 2010–2011, so the specific elasticity numbers don't directly apply to today's market. What does generalize is the methodology, the same framework can be applied to any modern pricing dataset.

Finally, aggregating to product-week level smooths out within-week variation. For products with rapid price changes (flash sales, dynamic pricing), a finer time resolution would be more appropriate.

Despite these caveats, the analysis provides directional guidance that a pricing team could use as a starting point — and the confidence intervals make the uncertainty explicit rather than hidden.

---

## What I Learned

Working through this project taught me that the hardest part of pricing analytics isn't the math, it's the discipline of asking what the numbers actually mean. A regression coefficient is just a number until one can explain why, it has the sign and magnitude it does, whether the assumptions behind it hold, and what a business should actually do differently because of it.

I also learned to take model diagnostics seriously. It's easy to fit a regression and report the R². It's much harder to check VIF, run Breusch-Pagan, examine Cook's Distance, and use robust standard errors, and skipping those steps is how analysts produce confident conclusions that fall apart under scrutiny.



## Author

Sakibul Huda  
LinkedIn: (https://www.linkedin.com/in/sakibul-huda-376b62371/) · Email: (mailto:sakibulhuda11@gmail.com)
