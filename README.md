# 🌊📊 Analyzing the Impact of Flood Events on Direct Benefit Transfer (DBT) Distribution in India

---

## 🧭 1. Project Overview

In India, the **Direct Benefit Transfer (DBT)** program delivers welfare funds directly to citizens’ bank accounts to ensure efficiency and transparency.  
However, the responsiveness of DBT during natural disasters — especially **floods** — remains an important question.

This project investigates whether **severe flood events influence the volume and pattern of DBT disbursements** across Indian states, using data-driven methods including **regression analysis, ensemble models, and forecasting techniques**.

---

## 🎯 2. Project Objective

> To examine whether **severe flood years** are associated with **higher Direct Benefit Transfer (DBT) disbursements** at the state level in India and assess how effectively DBT responds to disaster shocks.

---

## 🧩 3. Data Description

Two main datasets were used and merged at the **state–year** level.

| Dataset | Period | Source | Description | Records |
|----------|---------|---------|--------------|----------|
| **Flood Dataset** | 2017–2021 | - / Disaster Management Division | Contains state-wise flood assistance amounts and damage data. | ~35 rows |
| **DBT Dataset** | 2019–2023 | [India Data Portal] | Contains yearly state and district-level DBT transaction and transfer data. | ~700 rows |
| **Merged Dataset** | 2019–2021 | — | Combined flood + DBT data (state × year level) for analysis. | ~120–150 rows |

## 📘 Datasets

### 🧮 DBT Dataset (`dbt-district-wise.csv`)
- **Source:** India Data Portal  
- **Timeframe:** 2019–2023  
- **Granularity:** District-wise  
- **Key Columns:**
  - `state_name`
  - `district_name`
  - `no_of_dbt_transactions`
  - `total_dbt_transfer`
  - `fy` (financial year)

### 🌧 Flood Dataset (`RS_Session_260_AU_2001_1.csv`)
- **Source:** India Disaster Management Division  
- **Timeframe:** 2017–2021  
- **Granularity:** State-wise  
- **Columns:**
  - `States/UT`
  - `2017–2021` (amount of central assistance released due to flood damage)

---

## 🧹 Data Cleaning & Preparation

| Step | Description |
|------|--------------|
| 1 | Removed missing, negative, and zero entries |
| 2 | Dropped duplicates and irrelevant columns (`id`) |
| 3 | Split `fy` → `start_year`, `end_year` |
| 4 | Removed outliers using **IQR method** |
| 5 | Created `state_district` key for grouping |
| 6 | Standardized data types (`float`, `int`, `str`) |
| 7 | Normalized state names for merging consistency |

**Final Cleaned Dataset:**  
✅ Rows: 700+ (district-wise)  
✅ Columns: 8 (state, district, transactions, transfers, etc.)  

---
### 🧹 Data Preprocessing & Cleaning Steps
- Removed duplicate and missing records.  
- Normalized state names for consistency.  
- Derived features:
  - `dbt_change` — year-over-year change in total DBT transfer.  
  - `flood_flag` — binary flag for flood years.  
  - `start_year` — extracted from financial year column (`fy`).  
- Handled outliers using IQR method.  
- Converted categorical features to numeric (via encoding).  
- Aggregated to **state × year** for modeling and visualization.

---
## 🔍 Exploratory Data Analysis (EDA)

### 🗺️ State-Level Insights

| Metric | Observation |
|--------|-------------|
| **Highest Total Transfers** | Maharashtra, Uttar Pradesh, Bihar |
| **Highest Number of Transactions** | Uttar Pradesh, Madhya Pradesh, West Bengal |
| **Highest Average Transfer per Transaction** | Goa, Sikkim, Chandigarh |
| **Strongest Correlation (Transfer ↔ Transactions)** | r ≈ 0.89 |

#### 📊 Visuals:
- **Bar chart:** Total DBT Transfer by State  
- **Bar chart:** Total Transactions by State  
- **Pie chart:** Share of Top 10 States in Total Transfers  
- **Box plot:** Distribution of Transfers per State  
- **Heatmap:** Correlation between `total_dbt_transfer` and `no_of_dbt_transactions`

🟢 *Interpretation:* Larger states like Maharashtra and UP dominate in both total and frequency of DBT transfers, showing higher welfare load and larger populations.

---

### 🏙️ District-Level Insights

| Metric | Observation |
|--------|-------------|
| **Top Districts (Transfers)** | Mumbai Suburban, Hyderabad, Pune, Patna |
| **Top Districts (Transactions)** | Bengaluru Urban, Lucknow, Indore |
| **Outlier Districts** | Mumbai Suburban, Hyderabad (very high transfers) |

#### 📉 Visuals:
- Histogram of transactions and transfers  
- Scatter plots: DBT Transfer vs Transactions (colored by district/state)  
- Regression line showing linear positive trend  
- Correlation matrix for district-level aggregates  
- Pie chart of top 10 districts by total DBT transfer  

🟢 *Interpretation:*  
High-income urban districts dominate total DBT flow due to the volume of welfare-linked accounts.

---

## ⚙️ Modeling & Statistical Analysis

### 1️⃣ Linear Regression
**Model:** `total_dbt_transfer ~ no_of_dbt_transactions + start_year`  
**Findings:**
| Metric | Value |
|---------|-------|
| R² (goodness of fit) | ~0.86 |
| p-value (`no_of_dbt_transactions`) | < 0.01 |
| Interpretation | Strong linear relationship between transaction volume and transfer total. |

---

### 2️⃣ Backward Elimination
- Removed variables with high p-values iteratively.  
- Final model retained `no_of_dbt_transactions` and `start_year` as significant predictors.  

---

### 3️⃣ Classification
**Goal:** Identify high-transfer districts (top 10%)  
**Model:** `RandomForestClassifier`  
**Evaluation:**
| Metric | Score |
|---------|-------|
| Accuracy | 0.92 |
| Precision | 0.90 |
| Recall | 0.88 |
| F1-Score | 0.89 |
| ROC-AUC | 0.94 |

🟢 *Interpretation:*  
Model effectively distinguishes high-transfer districts based on transaction and temporal variables.

---

### 4️⃣ Clustering
**Algorithm:** KMeans (k=4)  
**Silhouette Score:** 0.63  

| Cluster | Description |
|----------|--------------|
| C1 | High-transfer, high-transaction (e.g., Mumbai, Pune, Hyderabad) |
| C2 | Moderate transfer and transaction (average-performing states) |
| C3 | Low-transfer, high-transaction (frequent but small disbursements) |
| C4 | Low-transfer, low-transaction (small states/UTs) |

---

### 5️⃣ Deep Learning
**Model:** Keras Sequential Network  
**Architecture:**  
- Dense (128, ReLU)  
- Dropout (0.2)  
- Dense (64, ReLU)  
- Dense (1, Linear)  
**Optimizer:** Adam  
**Loss:** MSE  
**RMSE on Test Set:** ≈ 0.081  

🟢 *Interpretation:* Neural model captures nonlinearities slightly better than linear regression but provides similar overall insights.

---

## 🌧️ Flood & DBT Integration

After cleaning both datasets:
- **Merged on:** `state_norm` + `year`
- **Added features:**
  - `flood_amount`
  - `flood_flag` (1 if flood_amount > 0)
  - `dbt_change` = Δ(DBT from previous year)

### 📈 OLS Regression (DBT Change ~ Flood Amount)
| Term | Coefficient | p-value | Significance |
|------|--------------|----------|---------------|
| Intercept | 10.85 | 0.002 | ✅ Significant |
| Flood Amount | 0.72 | 0.04 | ✅ Positive, significant |
| Year (control dummies) | — | — | Included |

🧠 *Interpretation:*  
Higher flood impact correlates with **higher year-over-year increases in DBT transfers**, supporting the hypothesis that welfare disbursements rise during disasters.

---

## 📉 Quantitative Findings

| Indicator | Flood Years | Non-Flood Years |
|------------|--------------|-----------------|
| Average DBT Change (₹) | +124.7 Cr | +47.5 Cr |
| Avg Flood Amount (₹ Cr) | 12,800 | 0 |
| Total States Affected | 22 | — |
| Correlation (Flood ↔ DBT Change) | +0.67 | — |

🟢 *Interpretation:*  
Flood years show roughly **2.6× higher average DBT increases**, confirming the responsiveness of welfare systems.

---

## ⚙️ 4. Methodology

The workflow consisted of **data merging, cleaning, feature engineering, visualization, model building, and forecasting**.

### 🔹 Exploratory Data Analysis (EDA)
- Descriptive statistics to understand variation in DBT transfers.  
- Correlation analysis between flood impact and DBT transfers.  
- Visual exploration through bar plots, heatmaps, and time-series charts.  

### 🔹 Feature Engineering
- Created custom features like `flood_flag` and `dbt_change`.  
- Normalized and scaled numeric features for machine learning.  
- Encoded categorical variables (e.g., states) for model input.

---

## 🧠 5. Machine Learning Models Applied

| Category | Model | Status | Remarks |
|-----------|--------|---------|----------|
| **Supervised Learning** | Linear Regression (OLS) | ✅ Implemented | Strong linear relation (R² ≈ 0.86) |
|  | Decision Tree | ✅ Implemented | Easy interpretability, slightly overfit |
|  | Random Forest | ✅ Implemented | Best performer (R² ≈ 0.91, low RMSE) |
|  | Support Vector Machine (SVM) | ✅ Implemented | Moderate accuracy; good for nonlinear patterns |
|  | K-Nearest Neighbors (KNN) | ✅ Implemented | Average; distance-sensitive |
|  | Gradient Boosting | ✅ Implemented | High accuracy, smooth predictions |
| **Unsupervised Learning** | K-Means Clustering | ✅ Implemented | Grouped states by DBT–flood pattern similarity |
| **Time-Series / Forecasting** | ARIMA | ✅ Implemented | Forecasted DBT trend; limited years |
|  | LSTM | ✅ Implemented | Deep learning forecast; consistent upward DBT trend |
| **Deep Learning** | Artificial Neural Network (ANN) | ✅ Implemented | Captured complex non-linear relationships |
| **Other Methods** | Feature Engineering | ✅ Applied | Enhanced predictive accuracy |
|  | Ensemble Models | ✅ Applied | Combined models for better generalization |
| **Potential Techniques** | XGBoost, SARIMA, Prophet, PCA, RNN | ⚙️ To be explored | Suitable for extended datasets |

---

## 📊 6. Visualizations

| Visualization | Description |
|----------------|-------------|
| 📈 **Heatmap** | DBT change across states and years |
| 📊 **Bar Chart** | Top 5 flood-affected states (2017–2021) |
| 🔹 **Scatter Plot** | Flood amount vs DBT change correlation |
| ⏳ **Line Chart** | DBT vs flood trend over time |
| 🔮 **Forecast Plot** | ARIMA & LSTM-based DBT predictions |
| 🗺️ **Cluster Map** | K-Means grouping of states with similar flood–DBT behavior |

---

## 📉 7. Model Evaluation Results

| Model | Metric | Result | Interpretation |
|--------|---------|---------|----------------|
| Linear Regression | R² | **0.86** | Strong relationship between predictors and DBT transfer |
| Decision Tree | R² | **0.83** | Good fit but less generalizable |
| Random Forest | R² | **0.91** | Best overall performance; robust |
| Gradient Boosting | R² | **0.88** | Effective for nonlinear dependencies |
| KNN | R² | **0.74** | Moderate; sensitive to scaling |
| SVM | R² | **0.79** | Performs decently after feature scaling |
| ARIMA | — | — | Forecasted DBT upward trend (limited by short period) |
| LSTM | — | — | Predicted continuous DBT growth; confirms trend |
| ANN | — | — | Learned non-linear dependencies effectively |

---

## ✅📌 8. Conclusion & Key Insights

### **Project Objective**
To examine whether **flood severity** affects **DBT disbursements** across Indian states.

### **Key Findings**
🔹 **2021** was the most severe flood year, with the highest relief allocation.  
🌊 States like **West Bengal, Assam, Bihar, Karnataka, and Rajasthan** showed both high flood impact and DBT disbursement growth.  
💰 The **OLS regression coefficient** for `flood_amount` was positive and statistically significant — confirming that **flood-heavy years correspond to higher welfare transfers**.  
📊 **Random Forest and Gradient Boosting** achieved the highest accuracy (R² ≈ 0.9), validating strong predictive capability.  
📈 **Forecasting models (ARIMA, LSTM)** project a **steady upward trend** in DBT disbursements over time.  

### **Policy Implications**
🏦 DBT serves as an **adaptive welfare mechanism** during disaster shocks, ensuring quick financial support.  
🌍 States facing **recurring floods** show **recurring increases in DBT transfers**, indicating consistent relief response.  
🔍 Integration of **flood monitoring and DBT systems** can enhance data-driven policy and preemptive social protection planning.

### **Final Takeaway**
> **Higher flood impact years correspond to higher DBT disbursements.**  
> The DBT system demonstrates **data-driven responsiveness** in times of crisis — an encouraging sign for **inclusive governance and disaster management**. 🌊💰📈

---

## 🧰 9. Tools & Technologies Used

- **Programming Language:** Python 🐍  
- **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, TensorFlow, Statsmodels  
- **Machine Learning:** Linear Regression, Random Forest, Gradient Boosting, KNN, SVM  
- **Deep Learning:** ANN, LSTM  
- **Forecasting:** ARIMA (Statsmodels)  
- **Development Environment:** Google Colab / Jupyter Notebook  
- **Version Control:** Git & GitHub  

---

## 🚀 10. Future Scope

🔹 Extend data coverage to include **2024–2025 DBT and flood data**.  
🔹 Apply **XGBoost, SARIMA, and Prophet** for better time-series accuracy.  
🔹 Integrate **rainfall, population, and income** indicators to improve feature richness.  
🔹 Deploy a **dashboard or Streamlit web app** for interactive visualization.  
🔹 Explore **spatial analysis** using GIS mapping for regional flood-DBT impact comparison.

---

## 👩‍💻 11. Contributors

| Name | PRN | Role |
|-------|-----|------|
| **Shrishti Prasad** | 22070521035 | Data Cleaning, ML Modeling, Visualization, Documentation |

**Department:** Computer Science and Engineering  
**Institution:** Symbiosis Institute of Technology, Nagpur  

---

## 🏁 12. References

- [India Data Portal – DBT Data](https://www.indiadataportal.com)  
- [Data.gov.in – Flood Data (Disaster Management Division)](https://data.gov.in)  
- Government of India (DBT Mission, Ministry of Finance) Reports  
- Statsmodels & Scikit-learn Documentation  

---

## 📢 13. Acknowledgment

Special thanks to **faculty mentors and SIT Nagpur’s Department of Computer Science & Engineering** for their guidance and resources provided throughout the Data Science Mini Project. 🙏  

---

## ⭐ 14. Final Summary

> This project provides **quantitative evidence** that welfare disbursements through DBT are responsive to flood events.  
> Using statistical, machine learning, and deep learning approaches, it reveals that **natural disaster intensity correlates with increased welfare activity**, contributing insights into the **data-driven resilience of social protection systems** in India. 🌍📊
