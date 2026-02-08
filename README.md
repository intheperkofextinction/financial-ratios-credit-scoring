#  Financial Ratios & Credit Scoring Dashboard

A Python-driven financial analytics project that computes key financial ratios, builds a **custom composite credit score**, assigns credit ratings, and visualizes portfolio-level and company-level insights through an interactive dashboard.

This project is designed to demonstrate **fundamental analysis, credit risk thinking, and data-driven decision-making** using real financial statement data.

---

##  Project Overview

Financial ratio analysis and credit assessment are core skills in:

- Equity Research  
- Credit Risk & Lending  
- Portfolio Management  
- Corporate Finance  
- Investment Analysis  

This project builds a **full analytical pipeline** that:

1. Extracts financial data programmatically  
2. Cleans and standardizes raw financial statements  
3. Computes profitability, leverage, liquidity, and efficiency ratios  
4. Normalizes metrics globally and sector-wise  
5. Constructs a weighted composite credit score  
6. Maps scores to familiar credit ratings  
7. Visualizes results in an interactive dashboard  

The goal is to move beyond static Excel analysis and create a **scalable, repeatable, and explainable credit scoring framework**.

---

##  Repository Structure

financial-ratios-credit-scoring/
├── data/
│ ├── raw/ # Raw financial data
│ └── processed/ # Cleaned & ratio-ready datasets
├── notebooks/
│ └── exploration.ipynb # EDA and ratio validation
├── scripts/
│ ├── data_collection.py # Data extraction using yahooquery
│ ├── data_cleaning.py # Cleaning & preprocessing
│ ├── ratio_calculation.py # Financial ratios
│ ├── normalization.py # Global & sector normalization
│ └── credit_scoring.py # Composite score & ratings
├── dashboard/
│ └── app.py # Dashboard application
├── requirements.txt
├── README.md
└── LICENSE


---

## 📌 Companies Covered

The analysis is performed on a sample portfolio of large-cap Indian companies across multiple sectors:

- Reliance Industries  
- Tata Consultancy Services (TCS)  
- Infosys  
- HDFC Bank  
- ICICI Bank  
- State Bank of India  
- ITC  
- Adani Ports  
- Larsen & Toubro  
- Bajaj Finance  

> ⚠️ **Disclaimer:**  
> This project is for educational and analytical purposes only and does not constitute investment advice.

---

## 🔗 Data Source

Financial data is sourced programmatically using the **`yahooquery`** Python library, which retrieves structured financial statements from Yahoo Finance.

```python
from yahooquery import Ticker
```
Data includes:

Income statements

Balance sheet items

Cash-flow–related metrics (where available)

🧹 Data Cleaning & Preprocessing

Raw financial statement data often contains missing values, duplicates, and inconsistent formats.

Key Cleaning Steps
1. Year Extraction
```python
income_clean["year"] = pd.to_datetime(income_clean["asOfDate"]).dt.year
```
2. Column Selection & Renaming

Only relevant financial fields are retained and renamed for clarity:

Revenue

Cost of Revenue

Gross Profit

Operating Income

Net Income

3. Missing Value Handling
```python
income_clean = income_clean.dropna(
    subset=["revenue", "cost_of_revenue", "gross_profit", "operating_income", "net_income"],
    how="all"
)

```
Rows with all key metrics missing are removed

Partial NaNs are handled downstream

Duplicate Removal
```python
income_clean = income_clean.drop_duplicates(
    subset=["ticker", "year"], keep="first"
)
```
5. Data Validity Filters

Revenue must be greater than zero

Numeric columns cast to float

Data sorted by company and year

Financial Ratio Calculation

Ratios are grouped into major analytical categories:

Profitability

Return on Assets (ROA)

Return on Equity (ROE)

Net Margin

Leverage & Risk

Debt-to-Equity

Interest Coverage

Liquidity

Current Ratio

Efficiency

Asset Turnover

Each ratio is implemented using standard finance definitions and calculated consistently across companies and years.

Global Normalization (Min–Max Scaling)

Since ratios exist on different numerical scales, global normalization is applied:

```
Normalized
=
𝑥
−
min
⁡
(
𝑥
)
max
⁡
(
𝑥
)
−
min
⁡
(
𝑥
)
Normalized=
max(x)−min(x)
x−min(x)
	​


Output range: 0 to 1
```
Ratios with no variation are set to NaN
