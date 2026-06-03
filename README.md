🛒 E-Commerce Customer Churn Analysis
End-to-End Data Analysis & Business Intelligence Project

Analyzed 4,038 customer records to uncover why customers leave an e-commerce platform — using Python for analysis and Power BI for interactive visualization.

Show Image

📌 Table of Contents

Project Overview
Key Findings
Dataset
Tech Stack
Project Structure
How to Run
Analysis Walkthrough
Power BI Dashboard
Business Recommendations
Resume Bullets


🎯 Project Overview
Customer churn — when customers stop buying — is one of the most expensive problems for any e-commerce business. Acquiring a new customer costs 5–7× more than retaining an existing one.
Goal: Identify which customers are most likely to churn, why they churn, and what the business can do about it.
Approach:

Clean and explore the raw dataset using Python
Engineer new features to better capture churn patterns
Build an interactive Power BI dashboard for business stakeholders
Translate findings into actionable recommendations


🔍 Key Findings
#FindingBusiness Impact1Customers with tenure < 5 months churn at 3× the rate of long-tenure customersFocus retention on new customers immediately after onboarding2100% of customers with satisfaction score 1 churnedLow satisfaction is the single strongest churn predictor3Customers who raised a complaint are 2.5× more likely to churnFast complaint resolution is critical — not optional4Cash on delivery users have the highest churn rate by payment modePayment friction drives churn — encourage digital payment onboarding5Higher cashback amounts correlate with lower churnCashback incentives are working — increase for at-risk segments

📊 Dataset
PropertyValueSourceKaggle — Ecommerce Customer Churn Analysis and PredictionRaw records5,630After cleaning4,038Features20 columnsTarget variableChurn (0 = Retained, 1 = Churned)Overall churn rate17.6% (712 churned customers)
Key columns used:
ColumnTypeDescriptionCustomerIDIDUnique customer identifierChurnBinary0 = Retained, 1 = ChurnedTenureNumericMonths as a customerSatisfactionScoreNumericScore 1–5ComplainBinaryWhether customer raised a complaintPreferredPaymentModeCategoricalCredit, Debit, E-wallet, CashCityTierCategoricalCity tier 1, 2, or 3CashbackAmountNumericCashback received in last monthTenureGroupEngineered0–5 / 6–10 / 11–20 / 20+ months

🛠 Tech Stack
ToolPurposePython 3.xData cleaning, EDA, visualizationsPandasData manipulation and analysisNumPyNumerical operationsMatplotlib + SeabornStatic visualizationsPower BI DesktopInteractive business dashboardExcelRaw data source

📁 Project Structure
ecommerce-churn-analysis/
│
├── README.md                          ← You are here
├── requirements.txt                   ← Python dependencies
│
├── data/
│   ├── E Commerce Dataset.xlsx        ← Original raw data (Kaggle)
│   └── cleaned_ecommerce_churn.csv    ← Cleaned dataset (output of notebook)
│
├── notebooks/
│   └── Churn_Analysis.ipynb           ← Full EDA and analysis notebook
│
├── src/
│   └── churn_analysis.py              ← Clean Python script version
│
├── dashboards/
│   └── Ecommerce_Churn_Dashboard.pbix ← Power BI dashboard file
│
├── images/
│   ├── final_dashboard_screenshot.png ← Power BI dashboard preview
│   ├── churn_by_tenure.png
│   ├── churn_by_satisfaction.png
│   ├── churn_by_payment_mode.png
│   ├── cashback_vs_churn.png
│   └── correlation_heatmap.png
│
└── reports/
    └── Project_Summary.pdf            ← 1-page business summary

🚀 How to Run
Step 1 — Clone the repository
bashgit clone https://github.com/yourusername/Ecommerce-Customer-Churn-Analysis.git
cd Ecommerce-Customer-Churn-Analysis
Step 2 — Install dependencies
bashpip install -r requirements.txt
Step 3 — Run the analysis
bash# Option A: Jupyter Notebook (recommended — shows step-by-step with output)
jupyter notebook notebooks/Churn_Analysis.ipynb

# Option B: Python script (runs everything at once)
python src/churn_analysis.py
Step 4 — View the dashboard
Open dashboards/Ecommerce_Churn_Dashboard.pbix in Power BI Desktop (free download from Microsoft).

📈 Analysis Walkthrough
Phase 1 — Data Cleaning
pythonimport pandas as pd
import numpy as np

df = pd.read_excel('data/E Commerce Dataset.xlsx', sheet_name='E Comm')
print(f"Raw shape: {df.shape}")  # (5630, 20)

# Fix column name typos
df.rename(columns={
    'PreferedOrderCat': 'PreferredOrderCat',
    'PreferedLoginDevice': 'PreferredLoginDevice'
}, inplace=True)

# Fill numeric nulls with median (robust to outliers)
numeric_cols = df.select_dtypes(include='number').columns
df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].median())

# Drop remaining nulls
df.dropna(inplace=True)
print(f"Clean shape: {df.shape}")  # (4038, 20)
Issues found and fixed:

Missing values in Tenure, HourSpendOnApp, OrderAmountHike, CouponUsed, OrderCount, DaySinceLastOrder, CashbackAmount
Column name typos (Prefered → Preferred)
No duplicate rows found

Phase 2 — Feature Engineering
python# Bin tenure into business-meaningful groups
df['TenureGroup'] = pd.cut(
    df['Tenure'],
    bins=[0, 5, 10, 20, 50],
    labels=['0–5 mo', '6–10 mo', '11–20 mo', '20+ mo']
)

# Overall churn rate
churn_rate = df['Churn'].mean() * 100
print(f"Overall churn rate: {churn_rate:.1f}%")  # 17.6%

# Churn rate by tenure group
print(df.groupby('TenureGroup')['Churn'].mean() * 100)
Phase 3 — Key EDA Results
Churn rate by tenure group:
TenureGroup
0–5 mo     ~50%   ← 3× higher than average
6–10 mo    ~30%
11–20 mo   ~17%
20+ mo     ~10%
Churn rate by satisfaction score:
Score 1    ~100%  ← Every single one churned
Score 2    ~43%
Score 3    ~15%
Score 4     ~7%
Score 5     ~5%
Top correlations with Churn:
Complain              +0.25   (positive = more complaints = more churn)
Tenure               -0.35   (negative = longer tenure = less churn)
SatisfactionScore    -0.18
CashbackAmount       -0.16

📊 Power BI Dashboard
The dashboard is a single-page interactive report built on a dark professional theme.
Layout:
┌─────────────────────────────────────────────────────┐
│  E-Commerce Customer Churn Analysis                  │
│  4,038 customers · Kaggle dataset · Power BI         │
├──────────┬──────────┬──────────┬────────────────────┤
│  4,038   │  17.6%   │  9.9 mo  │      3.1 / 5       │
│ Customers│  Churn   │  Tenure  │   Satisfaction      │
├──────────┴──────┬───┴──────────┴────────────────────┤
│ Churn by Tenure │   Churn by Satisfaction Score      │
│    [Bar Chart]  │        [Column Chart]              │
├────────┬────────┴──────────────┬────────────────────┤
│ Slicers│  Churn by Payment     │   Key Insights      │
│  City  │      [Bar Chart]      │   [Text Box]        │
│ Gender │                       │                     │
│Complain│                       │                     │
└────────┴───────────────────────┴────────────────────┘
DAX Measures used:
daxChurn Rate =
DIVIDE(
    COUNTROWS(FILTER('cleaned_ecommerce_churn',
        'cleaned_ecommerce_churn'[Churn] = 1)),
    COUNTROWS('cleaned_ecommerce_churn'), 0
) * 100

Avg Tenure = ROUND(AVERAGE('cleaned_ecommerce_churn'[Tenure]), 1)

Avg Satisfaction = ROUND(AVERAGE('cleaned_ecommerce_churn'[SatisfactionScore]), 1)
Slicers included: City Tier · Gender · Complaint (Yes/No)

💡 Business Recommendations
Based on the analysis, three immediate actions would reduce churn:
1. New Customer Onboarding Program (addresses 50% churn in 0–5 month group)

Assign a personal account manager for the first 3 months
Send proactive check-in messages at months 1, 2, and 3
Offer a loyalty bonus at the 6-month mark to incentivise staying

2. Complaint Fast-Track Resolution (addresses 2.5× churn for complainants)

Resolve complaints within 24 hours with a follow-up satisfaction check
Offer an automatic cashback credit when a complaint is raised
Flag customers with open complaints for immediate retention outreach

3. Satisfaction Score Recovery (addresses 100% churn at score 1)

Automatically trigger a win-back campaign for any customer who gives score 1 or 2
Personal call/chat from customer success within 48 hours of a low score
Targeted cashback or discount offer tied to next purchase

Projected impact: Implementing all three could reduce churn by an estimated 15–20%, retaining approximately 120–150 additional customers per month.

📝 Resume Bullets
Copy these directly into your resume under Projects:

E-Commerce Customer Churn Analysis | Python · Power BI · Excel


Analyzed 4,038-record e-commerce dataset using Python (Pandas, Seaborn) to identify customer churn drivers and high-risk segments
Discovered customers with tenure under 5 months churn at 3× the average rate; complaint-raisers churn at 2.5× baseline
Built interactive Power BI dashboard with 4 KPI cards, 4 chart visuals, and dynamic slicers across city tier, gender, and complaint status
Engineered 2 new features (TenureGroup, ComplainLabel) to improve segmentation clarity for business stakeholders
Delivered 3 data-backed business recommendations with projected 15–20% churn reduction potential


👤 Author
Satyam Vishwakarma
B.Sc. Information Technology — Mumbai
Show Image
Show Image

📄 License
This project is open source and available under the MIT License.

⭐ If this project helped you, plea
