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

```
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
│ └── powerBI # Dashboard application
├── requirements.txt
├── README.md
└── LICENSE


```
---

##  Companies Covered

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

>  **Disclaimer:**  
> This project is for educational and analytical purposes only and does not constitute investment advice.

---

##  Data Source

Financial data is sourced programmatically using the **`yahooquery`** Python library, which retrieves structured financial statements from Yahoo Finance.

```python
from yahooquery import Ticker
```
Data includes:

Income statements

Balance sheet items

Cash-flow–related metrics (where available)

---

 Data Cleaning & Preprocessing

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

---

## Financial Ratio Calculation

Ratios are grouped into major analytical categories:

### Profitability

* Return on Assets (ROA)

* Return on Equity (ROE)

* Net Margin

### Leverage & Risk

* Debt-to-Equity

* Interest Coverage

### Liquidity

* Current Ratio

### Efficiency

* Asset Turnover

Each ratio is implemented using standard finance definitions and calculated consistently across companies and years.

---

## Global Normalization (Min–Max Scaling)

Since ratios exist on different numerical scales, global normalization is applied:


- Output range: **0 to 1**
- If `max(x) = min(x)`, the normalized value is set to `NaN`

Ratios with no variation are set to NaN

Normalized value is calculated as:

**Normalized = (x − min(x)) / (max(x) − min(x))**

- Output range: 0 to 1

##  Sector-Based Normalization (Z-Score)

To account for sector-specific characteristics, financial ratios are also normalized **within each sector**.  
This ensures companies are evaluated relative to their industry peers, not just the overall dataset.

### Z-Score Calculation

Z = (x - mean_sector) / std_sector

Where:
- x = company’s ratio value  
- mean_sector = average ratio value within the sector  
- std_sector = standard deviation of the ratio within the sector  

### Z-Score Scaling to 0–1

To prevent extreme outliers from dominating the score, Z-scores are clipped and rescaled:

Sector_Normalized = (clip(Z, -3, 3) + 3) / 6

This maps sector-relative values to a 0–1 range.

---

##  Final Ratio Blending

Each final ratio is a weighted combination of:

- 70% global normalization  
- 30% sector-based normalization  

Final ratio calculation:

final_ratio = 0.7 * global_norm + 0.3 * sector_norm

All final ratios are clipped to the 0–1 range.

---

##  Composite Credit Score Construction

A weighted composite credit score is calculated using the final normalized ratios.

### Weights Used

Metric | Weight
------ | ------
ROA | 15%
Net Margin | 15%
Asset Turnover | 10%
Interest Coverage | 25%
Debt-to-Equity | 20%
Current Ratio | 15%

### Credit Score Formula

credit_score_raw = Σ (final_ratio_i × weight_i)

Python implementation:

df['credit_score_raw'] = (
    df[list(valid_weights.keys())]
      .mul(list(valid_weights.values()))
      .sum(axis=1)
)

### Final Scaling

credit_score = credit_score_raw × 100

- Final score range: 0–100  
- Higher score indicates stronger credit quality  

---

##  Credit Rating Mapping

Composite credit scores are mapped to standard credit rating buckets:

Score Range | Rating
----------- | ------
≥ 80 | AAA
70–79 | AA
60–69 | A
50–59 | BBB
40–49 | BB
30–39 | B
< 30 | CCC

Rating logic:

def score_to_rating(s):
    if s >= 80: return 'AAA'
    if s >= 70: return 'AA'
    if s >= 60: return 'A'
    if s >= 50: return 'BBB'
    if s >= 40: return 'BB'
    if s >= 30: return 'B'
    return 'CCC'

---

## Dashboard And Chart Preview
<img width="2498" height="1223" alt="image" src="https://github.com/user-attachments/assets/f870c82b-d4c0-4351-ab7f-58654b6e3016" />
<img width="2495" height="1209" alt="image" src="https://github.com/user-attachments/assets/3f586b5d-30c5-45c5-b05f-2589fba08695" />
<img width="2496" height="1208" alt="image" src="https://github.com/user-attachments/assets/3d6db041-10a0-4ec9-84d7-d62cb893bfcc" />
<img width="2489" height="1212" alt="image" src="https://github.com/user-attachments/assets/02843010-8787-4be7-93fa-c9bbcb4ed377" />
<img width="1838" height="792" alt="image" src="https://github.com/user-attachments/assets/6ecbee90-9d4d-4d1c-8097-732517d5336c" />
<img width="2386" height="695" alt="image" src="https://github.com/user-attachments/assets/13fe2d6c-5f27-45c1-a5e1-a4bd0b70f289" />
<img width="906" height="522" alt="image" src="https://github.com/user-attachments/assets/65302af3-0010-4efb-9cab-852e19dbe66b" />
<img width="1717" height="671" alt="image" src="https://github.com/user-attachments/assets/48556cad-b820-42fa-8b69-1a7002bed6e8" />






##  Dashboard Insights

The dashboard enables users to:
- Compare risk versus profitability
- Identify high-quality versus leveraged companies
- Analyze sector-wise credit strength
- Track earnings stability and volatility
- Perform portfolio-level risk assessment

The dashboard is designed to support **decision-making**, not just visualization.

---

##  Technology Stack

- Python (pandas, numpy)
- yahooquery (financial data sourcing)
- Plotly / Dash / Power BI (visualization)
- Git & GitHub (version control)

---

##  How to Run the Project

1. Clone the repository:

git clone https://github.com/intheperkofextinction/financial-ratios-credit-scoring.git  
cd financial-ratios-credit-scoring  

2. Install dependencies:

pip install -r requirements.txt  

3. Run the dashboard:

powerBI: https://github.com/intheperkofextinction/financial-ratios-credit-scoring/blob/main/credit%20risk%20analyss.pbix 



---

##  Future Improvements

Potential enhancements include:
- Cash-flow–based ratios
- Time-series trend weighting
- Macroeconomic overlays
- Automated portfolio optimization
- Model validation against default events

---

##  License

This project is licensed under the terms specified in the LICENSE file.

---

##  Author

Amal  
Finance | Analytics | Credit Risk  
GitHub: https://github.com/intheperkofextinction


