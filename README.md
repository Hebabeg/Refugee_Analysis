# Global Refugee and Asylum-Seeker Analysis

A comprehensive data-analytic workflow on global refugee and asylum-seeker data (2019–2024). We performed data ingestion, cleaning, exploratory data analysis (EDA), time-series forecasting, and advanced analytics (e.g., host‐burden indexing, clustering, regression). Our forecasting model uses Facebook Prophet on annual counts, projecting trends two years beyond 2024. Key findings highlight increasing host burdens in specific countries, seasonal patterns, and drivers of refugee movements.

## 1 Introduction

### 1.1 Problem Statement

Forced displacement continues to challenge humanitarian systems worldwide. Understanding historical trends and projecting future refugee flows helps policymakers allocate resources efficiently and plan for emerging crises.

### 1.2 Objectives

Describe the dataset and preprocessing steps.

Explore distributions, correlations, and geographic patterns.

Decompose seasonal trends and forecast near-term refugee counts.

Analyze country-level burdens, clustering profiles, and regression drivers.

## 2 Dataset Description

### 2.1 Source and Scope

File: Refugee_Asylum_data.xlsx containing yearly sheets RA_2019 through RA_2024.

Fields: Country of origin/asylum, Year, Refugees under UNHCR's mandate, Asylum-seekers, Returned refugees, IDPs of concern, Stateless persons, Others of concern, Host community, and more.

### 2.2 Data Dimensions

Years: 6 annual snapshots (2019–2024).

Records: ~X,XXX rows after concatenation.

Geographies: ~Y countries of asylum, Z countries of origin.

## 3 Data Preprocessing

### 3.1 Loading

Read all Excel sheets with pandas.read_excel (engine=openpyxl).

Verified sheet names; concatenated RA_2019–RA_2024 into single DataFrame.

### 3.2 Cleaning

Stripped whitespace in column names.

Standardized numeric fields via pd.to_numeric(errors='coerce').

Handled a small typo in 'Returned IDPss' vs 'Returned IDPs'.

### 3.3 Missing Values

Computed per-column missing counts; dropped or imputed only where necessary.

## 4 Exploratory Data Analysis (EDA)

### 4.1 Summary Statistics

Described central tendencies and spreads using .describe().

### 4.2 Correlations

Heatmap of numeric variables (Refugees, Asylum-seekers, etc.) to identify strong pairwise relationships.

Rationale: Correlation helps detect multicollinearity before modeling.

### 4.3 Distributions

Histograms (linear and log scales) for key metrics; KDE to visualize skew.

### 4.4 Pairwise Scatterplots

sns.pairplot on top metrics to reveal joint distributions and potential clusters/outliers.

![image](https://github.com/user-attachments/assets/d6a1a2f6-954b-482f-978f-19b5a62ce1ea)


## 5. Seasonal Decomposition

Applied STL decomposition on monthly-aggregated refugee counts (when ≥24 observations).

Trend, seasonal, and residual components plotted to detect periodicity and anomalies.

Why STL?  Robust to irregular series lengths; separates components nonparametrically.

## 6. Time-Series Forecasting

### 6.1 Data Preparation

Aggregated annual totals by Year.

![image](https://github.com/user-attachments/assets/09d5400f-7add-4101-9611-479a7de67b1b)

Converted integer Year → datetime (Year-01-01) for Prophet compatibility.

### 6.2 Model Choice: Facebook Prophet

Handles seasonality (yearly), trend changepoints, and holidays.

Simple API for uncertainty intervals.

### 6.3 Implementation

Fit with yearly_seasonality=True, linear growth.

Forecast two future years at annual frequency.

![image](https://github.com/user-attachments/assets/5ee70af3-e6b9-4857-a4e8-05aa3d45b4b8)

Format x-axis to show discrete years.

### 6.4 Results & Validation

Projected totals for 2025–2026.

Uncertainty bounds visualized.

## 7. Host Burden Index

Computed refugees per 1,000 national population using UNHCR totals and country populations.

![image](https://github.com/user-attachments/assets/e12f8a76-969c-491f-a03b-c43f30c1b58a)


Bar chart of top 10 host burdens to highlight disproportionate impacts.

![image](https://github.com/user-attachments/assets/34a5dd20-7729-438d-924f-f213b6b3f97b)

Metric rationale: Normalizing raw counts by population size allows fair cross-country comparisons.

## 8. Clustering of Asylum Countries

### 8.1 Feature Engineering

Aggregated sum of four metrics (refugees, asylum-seekers, returned, stateless).

Standardized with StandardScaler.

### 8.2 Elbow Method

Evaluated WCSS across k=1–9 clusters to pick optimal k (here, k=4).

![image](https://github.com/user-attachments/assets/34aff360-01f5-417a-8b83-23217c4d65e8)

### 8.3 K-Means & PCA Visualization

Labeled clusters; plotted in 2D via PCA.

Annotated high-volume countries for interpretability.

Origin–Asylum Patterns

![image](https://github.com/user-attachments/assets/168a3413-626a-410b-9c26-f61ea3dfd0b8)

Top origin→asylum flows by total asylum-seekers.

![image](https://github.com/user-attachments/assets/921e39af-e06f-4a4e-97d1-c74df1dbb2eb)

Heatmap and line trends for top 5 pairs to reveal shifting migration corridors.

![image](https://github.com/user-attachments/assets/a3b88492-4e0d-41b8-8e56-582e65425703)

## 9. Other Analyses

Returned Refugees: Trends and heatmaps of returns.

![image](https://github.com/user-attachments/assets/8798968f-f441-4743-9346-0972bb2f2eb1)

![image](https://github.com/user-attachments/assets/4c71218a-3777-47f2-b7d2-78d417c3dfbf)

Stateless Persons: Top host countries and annual trends.

Developed Countries: Focus on OECD nations’ refugee shares.

![image](https://github.com/user-attachments/assets/b9806ce5-6246-4442-9e28-98652082631b)

![image](https://github.com/user-attachments/assets/dfc5a7c7-94ca-4606-a2e5-8a73a3a6e136)

Bubble Chart: Gapminder-style view of asylum vs. refugee counts by year.

![image](https://github.com/user-attachments/assets/4f9728fe-eab2-4200-aeb4-f9d7d670d85e)

GDP vs Refugees: bar charts linking economic wealth to refugee burden.

![image](https://github.com/user-attachments/assets/61bbc14b-c066-4bac-b03a-fee71e10dd9c)

## 10. Conclusions and Recommendations

Rising Burdens in countries like Lebanon and Jordan warrant increased international support.

Seasonality & Trends: Refugee inflows show modest annual cycles but primarily upward linear growth—planning should assume continued increases.

Economic & Stability Drivers: GDP per capita and stability inversely correlate with refugee intake, suggesting wealthier, more stable nations host more.

Policy Implications: Data-driven resource allocations, preemptive capacity building in high-burden states, and targeted aid based on predictive forecasts.
