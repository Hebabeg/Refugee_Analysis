# Global Refugee and Asylum-Seeker Analysis

A comprehensive data-analytic workflow on global refugee and asylum-seeker data (2019–2024). We performed data ingestion, cleaning, exploratory data analysis (EDA), time-series forecasting, and advanced analytics (e.g., host‐burden indexing, clustering, regression). Our forecasting model uses Facebook prophet on annual counts, projecting trends two years beyond 2024. Key findings highlight increasing host burdens in specific countries, seasonal patterns, and drivers of refugee movements.

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

Converted integer Year → datetime (Year-01-01) for prophet compatibility.

### 6.2 Model Choice: Facebook prophet

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

The dataset contains refugee and asylum information across multiple years (2019-2024) with details on:

Refugees under UNHCR's mandate
Asylum seekers
Returned refugees
Internally displaced persons (IDPs)
Stateless persons
For 2024, the data shows:

Total Refugees: 31,956,584
Total Asylum Seekers: 7,996,077
Total IDPs: 67,053,895
Total Stateless Persons: 4,368,258

The global refugee crisis shows significant patterns across multiple dimensions, as illustrated in the time series visualization. This chart demonstrates the concurrent trends of refugee populations, asylum seekers, and internally displaced persons (IDPs) over the study period.

![image](https://github.com/user-attachments/assets/d066e57b-90d3-4592-a1fa-7977e021b1cc)

The heatmap reveals intense concentrations of refugee movements between specific regions and countries. The darker areas indicate higher refugee populations, showing major displacement corridors.

![image](https://github.com/user-attachments/assets/46469926-13b1-4898-9f4b-764bfe6008cc)

The Venezuela-US corridor represents the largest asylum-seeking situation with 620,074 cases in 2024
Peru has been another major destination for Venezuelan asylum seekers

![image](https://github.com/user-attachments/assets/094b6b17-0ad4-4b49-a8dc-192f4d4491af)

This visualization maps the relationship between countries of origin and asylum, highlighting the primary migration routes and refugee hosting patterns.

### Detailed Conclusions
Temporal Trends
The data shows a consistent upward trend in global displacement numbers
There's been a notable acceleration in asylum applications post-2021
IDP numbers have shown the most dramatic increases, indicating intensifying internal conflicts

### Geographic Patterns
Major refugee-hosting regions are concentrated in neighboring countries of conflict zones
There's a significant disparity in refugee distribution, with some countries bearing disproportionate responsibility
Cross-continental movement is limited compared to regional displacement

### Policy Implications
The increasing numbers suggest current international protection frameworks are under unprecedented strain
There's a clear need for more equitable responsibility-sharing among nations
The high number of stateless persons (4.3 million) indicates a persistent gap in international legal protection

### Humanitarian Considerations
The large IDP population (67 million) suggests internal conflicts remain a primary driver of displacement
The ratio between refugees and asylum seekers indicates processing backlogs in many asylum systems
The returned refugee numbers (433,527) are relatively low, suggesting prolonged displacement situations

### Future Projections
Current trends suggest continued growth in global displacement
The Venezuelan crisis remains a significant factor in hemispheric migration patterns
The high number of stateless persons indicates a long-term challenge requiring sustained international attention

## Recommendations
#### Strengthen international responsibility-sharing mechanisms
#### Develop more efficient asylum processing systems
#### Enhance support for major refugee-hosting countries
#### Create targeted programs for protracted refugee situations
#### Address root causes of displacement through preventive diplomacy
#### Develop comprehensive solutions for statelessness
#### Increase resources for IDP protection and assistance
#### Strengthen early warning systems for potential displacement crises
#### Improve coordination between humanitarian and development actors

This analysis reveals a complex global displacement situation requiring sustained, coordinated international response. The data suggests that traditional approaches to refugee protection may need revision to address the scale and complexity of contemporary displacement challenges. The significant numbers across all categories (refugees, asylum seekers, IDPs, and stateless persons) indicate that forced displacement remains one of the most pressing global challenges of our time.
