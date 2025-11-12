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
| **Flood Dataset** | 2017–2021 | [Data.gov.in](https://data.gov.in) / Disaster Management Division | Contains state-wise flood assistance amounts and damage data. | ~35 rows |
| **DBT Dataset** | 2019–2023 | [India Data Portal](https://www.indiadataportal.com) | Contains yearly state and district-level DBT transaction and transfer data. | ~700 rows |
| **Merged Dataset** | 2019–2021 | — | Combined flood + DBT data (state × year level) for analysis. | ~120–150 rows |

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
