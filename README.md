# 🌊 Analyzing the Impact of Flood Events on Direct Benefit Transfer (DBT) Distribution in India  

## 🧾 Overview
This project investigates how **flood events** influence the **Direct Benefit Transfer (DBT)** distribution across Indian states and districts.  
Using district-wise DBT data (2019–2023) and flood impact data (2017–2021), the analysis explores patterns, correlations, and causal links between flood severity and welfare disbursements.  

---

## 📊 Problem Statement
India’s **Direct Benefit Transfer (DBT)** program aims to directly transfer subsidies and welfare benefits into citizens’ bank accounts to reduce leakages and delays.  
However, **floods** — one of India’s most frequent and severe natural disasters — can disrupt both welfare demand and delivery mechanisms. In India, the Direct Benefit Transfer (DBT) system ensures that government subsidies and welfare benefits reach citizens directly through digital payments.
However, the effectiveness of DBT can be influenced by external factors like natural disasters, especially floods, which frequently disrupt lives and infrastructure across the country.

This project analyzes how flood events impact DBT distribution across states and districts.
By combining district-wise DBT data (2019–2023) with flood impact data (2017–2021), it investigates whether flood years correspond to higher DBT disbursements, indicating the system’s responsiveness to crisis situations.

The goal is to:

Examine correlations between flood severity and DBT transfers.

Identify states and districts where DBT increases sharply during flood years.

Evaluate the overall effectiveness of DBT as a disaster-response mechanism using statistical and machine learning models.

This project investigates:
> Do flood years show an increase in DBT transfers?  
> Is there a measurable relationship between **flood intensity** and **DBT disbursements** at the state level?

---

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

## 📊 Visualization Highlights

| Visualization | Description | Insight |
|---------------|--------------|----------|
| 📊 **Heatmap** | Year-wise DBT change per state | States like WB, Assam, Bihar show strong positive DBT changes during flood years |
| 📈 **Bar Chart** | Top 10 states with DBT increases during floods | West Bengal, Assam, Bihar lead the list |
| 🌀 **Dual-Axis Plot** | DBT Total (line) vs Flood Amount (bars) per state | Visual correlation between flood impact and DBT rise |
| 🧩 **Scatter Plot** | DBT Transfer vs Transactions | Strong linear positive relationship |
| 🎯 **Regression Line Plot** | Overall trend (district level) | Confirms consistent growth pattern |
| 🧮 **Correlation Matrix** | Between DBT metrics | 0.88 correlation confirms model reliability |

---

## ✅ Key Insights

1. **Flood years correspond to significantly higher DBT transfers.**  
2. **States with frequent floods (WB, Assam, Bihar)** show consistent DBT growth post-floods.  
3. **Positive correlation (r ≈ 0.67)** between flood assistance and DBT increase.  
4. **Regression and deep learning** both confirm upward trend of DBT post disasters.  
5. **Policy takeaway:** DBT system acts as a responsive relief channel during crises.

---

## 🧭 Future Enhancements
- Integrate **rainfall and disaster severity** datasets.  
- Employ **time-series (ARIMA/LSTM)** to forecast DBT flows.  
- Apply **difference-in-differences (DiD)** modeling to estimate causal impacts.  
- Build an **interactive Streamlit dashboard** for visualization.  

---

## 🧰 Tech Stack
- **Languages:** Python  
- **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, Statsmodels, TensorFlow, Plotly  
- **Tools:** Jupyter Notebook, Google Colab  
- **Environment:** Python 3.10+, Colab GPU (for DL model)  

---


## 👩‍💻 Author
**Shrishti Prasad (7th Sem B.Tech CSE)**  
📧 shrishtiprasad435@gmail.com  
💼 *Passionate about Data Science, Machine Learning, and Public Policy Analytics.*

---

