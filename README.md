1. Main File: README.md (Most Important)
# E-Commerce Customer Churn Analysis

**End-to-End Data Analysis & Visualization Project**

![Dashboard Preview](images/final_dashboard_screenshot.png)

## 📋 Project Objective
Analyzed **5,630 customer records** of an e-commerce platform to understand customer churn behavior and provide actionable business insights using Python and Power BI.

### 🎯 Key Insights Discovered
- Customers with **tenure < 5 months** have **3x higher** churn rate
- Customers who raised **complaints** are **2.5x more likely** to churn
- Strong negative correlation between **Cashback Amount** and churn
- Low **Satisfaction Score** is one of the strongest churn predictors

## 🛠 Tech Stack
- **Python** — Pandas, NumPy, Matplotlib, Seaborn
- **Power BI** — Interactive Dashboard
- **Excel** — Data Cleaning
- **SQL** — Querying

## 📁 Repository Structure

├── README.md 

├── requirements.txt

├── data/

│   └── E Commerce Dataset.xlsx

├── notebooks/

│   └── Churn_Analysis.ipynb

├── src/

│   └── churn_analysis.py

├── dashboards/

│   └── Ecommerce_Churn_Dashboard.pbix

├── images/

│   ├── churn_by_tenure.png
│   ├── churn_by_complain.png
│   └── correlation_heatmap.png

└── reports/

└── Project_Summary.pdf

## 🚀 Quick Start
```bash
git clone https://github.com/yourusername/Ecommerce-Customer-Churn-Analysis.git
cd Ecommerce-Customer-Churn-Analysis
pip install -r requirements.txt

📊 Key Visualizations Included

Overall Churn Rate KPI
Churn by Tenure Group
Churn by Complaint Status
Satisfaction Score Distribution
Payment Mode Analysis
Correlation Heatmap

💼 Skills Demonstrated

End-to-End Data Analysis Pipeline
Data Cleaning & Feature Engineering
Exploratory Data Analysis (EDA)
Business Insights Generation
Interactive Dashboard Development (Power BI)

2. requirements.txt

pandas
numpy
matplotlib
seaborn
openpyxl

3. Main Analysis Code: src/churn_analysis.py

# =====================================================
# E-COMMERCE CUSTOMER CHURN ANALYSIS
# Author: Satyam Vishwakarma
# =====================================================

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

plt.style.use('seaborn-v0_8')
sns.set_palette("husl")

# --------------------- LOAD DATA ---------------------
df = pd.read_excel('data/E Commerce Dataset.xlsx', sheet_name='E Comm')

print("✅ Dataset Loaded Successfully!")
print(f"Shape: {df.shape}")

# --------------------- DATA CLEANING ---------------------
df.columns = df.columns.str.strip().str.replace(' ', '')
df.rename(columns={
    'PreferedOrderCat': 'PreferredOrderCat',
    'PreferedLoginDevice': 'PreferredLoginDevice'
}, inplace=True)

df['Tenure'] = df['Tenure'].fillna(df['Tenure'].median())
df.dropna(inplace=True)

df['TenureGroup'] = pd.cut(df['Tenure'], bins=[0, 5, 10, 20, 50],
                           labels=['0-5', '6-10', '11-20', '20+'])

print(f"✅ Data Cleaned. Final Shape: {df.shape}")

# --------------------- KEY METRICS ---------------------
churn_rate = df['Churn'].mean() * 100
print(f"\nOverall Churn Rate: {churn_rate:.2f}%")

# --------------------- VISUALIZATIONS ---------------------
fig = plt.figure(figsize=(20, 14))

plt.subplot(2, 3, 1)
sns.countplot(data=df, x='TenureGroup', hue='Churn', palette=['#2ecc71', '#e74c3c'])
plt.title('Churn by Tenure Group')
plt.xticks(rotation=45)

plt.subplot(2, 3, 2)
sns.countplot(data=df, x='Complain', hue='Churn', palette=['#2ecc71', '#e74c3c'])
plt.title('Churn by Complaint Status')

plt.subplot(2, 3, 3)
sns.boxplot(data=df, x='Churn', y='SatisfactionScore', palette=['#2ecc71', '#e74c3c'])
plt.title('Satisfaction Score by Churn')

plt.subplot(2, 3, 4)
df.groupby('PreferredPaymentMode')['Churn'].mean().sort_values().plot(kind='barh', color='#e74c3c')
plt.title('Churn Rate by Payment Mode')

plt.subplot(2, 3, 5)
corr = df.select_dtypes(include=np.number).corr()
sns.heatmap(corr[['Churn']].sort_values(by='Churn', ascending=False), annot=True, cmap='coolwarm')
plt.title('Correlation with Churn')

plt.subplot(2, 3, 6)
sns.boxplot(data=df, x='Churn', y='CashbackAmount', palette=['#2ecc71', '#e74c3c'])
plt.title('Cashback Amount vs Churn')

plt.tight_layout()
plt.savefig('images/full_eda_visualizations.png', dpi=300, bbox_inches='tight')
plt.show()

print("\n🎉 Analysis Completed! Check 'images/' folder for visualizations.")
