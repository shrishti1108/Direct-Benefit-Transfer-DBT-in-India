# 🌊 Analyzing the Impact of Flood Events on Direct Benefit Transfer (DBT) Distribution in India  

## 🧾 Overview
This project investigates how **flood events** influence the **Direct Benefit Transfer (DBT)** distribution across Indian states and districts.  
Using district-wise DBT data (2019–2023) and flood impact data (2017–2021), the analysis explores patterns, correlations, and causal links between flood severity and welfare disbursements.  

---

## 📂 Project Structure

---

## 📊 Problem Statement
In India, the **Direct Benefit Transfer (DBT)** system directly transfers subsidies and benefits to citizens’ accounts.  
Natural disasters, especially **floods**, disrupt both demand for welfare support and government delivery mechanisms.  
This study analyzes whether DBT disbursements increase during flood years, indicating responsiveness of welfare systems to disasters.

---

## 📘 Datasets Used

### 1️⃣ DBT Dataset
- **File:** `dbt-district-wise.csv`  
- **Source:** India Data Portal – DBT Performance  
- **Period:** 2019–2023  
- **Level:** District-wise  
- **Columns:**
  - `state_name`, `district_name`
  - `no_of_dbt_transactions`
  - `total_dbt_transfer`
  - `fy` (Financial Year)

### 2️⃣ Flood Dataset
- **File:** `RS_Session_260_AU_2001_1.csv`  
- **Period:** 2017–2021  
- **Level:** State-wise  
- **Columns:**
  - `States/UT`
  - `2017–2021` flood damages / assistance released  

---

## 🧹 Data Cleaning Process

| Step | Description |
|------|--------------|
| 1 | Removed missing, negative, and zero values |
| 2 | Dropped duplicates and irrelevant `id` column |
| 3 | Split `fy` into `start_year` and `end_year` |
| 4 | Applied **IQR method** to remove outliers |
| 5 | Created composite key `state_district` |
| 6 | Standardized data types and naming conventions |

---

## 🔍 Exploratory Data Analysis (EDA)

### 🗺️ State-wise Analysis
- Mean, median, mode computed for each state  
- Correlation heatmap between **total_dbt_transfer** and **no_of_dbt_transactions**  
- Visualization:
  - **Bar plots:** Total DBT transfer & transactions by state  
  - **Box plots:** Distribution of transfers across states  
  - **Pie charts:** Top 10 states’ share in DBT transfers  

### 🏙️ District-wise Analysis
- Distribution of transactions and transfers per district  
- Identification of **outlier districts** (e.g., Mumbai, Pune, Hyderabad)  
- Scatter and regression plots showing DBT trends  
- Correlation heatmaps of district aggregates  

---

## ⚙️ Modeling & Statistical Analysis

### 🔹 Linear Regression
Used **StatsModels OLS** and **Scikit-learn LinearRegression** to estimate the relationship between DBT transfers and predictors like:
- `no_of_dbt_transactions`
- `start_year`

### 🔹 Backward Elimination
Automatic variable selection based on p-values to optimize model simplicity and significance.

### 🔹 Classification
- Used **RandomForestClassifier** to classify **high_transfer** districts (top 10%).  
- Evaluation metrics: Precision, Recall, F1-Score, ROC-AUC.

### 🔹 Clustering
- **KMeans** clustering (k=4) to group districts by DBT transfer & transaction similarity.  
- Evaluated using **Silhouette Score**.

### 🔹 Deep Learning
- Built a small **Keras Sequential Neural Network** with 2 hidden layers for regression.
- Optimizer: Adam | Loss: MSE | Metric: RMSE  
- Visualized actual vs. predicted values & residual distribution.

---

## 🌧️ Merging with Flood Data

Steps performed:
1. Cleaned flood dataset (melted to long format: state–year–amount).  
2. Normalized state names to match DBT dataset.  
3. Aggregated DBT to **state × year** level.  
4. Merged DBT and flood data → `merged_state_year_dbt_flood.csv`.  
5. Added:
   - `flood_flag` (1 if flood_amount > 0)
   - `dbt_change` (year-over-year DBT difference)

---

## 📈 Statistical Findings

- **OLS regression:**  
  - Positive (and weakly significant) relationship between `flood_amount` and `dbt_change`.  
  - Year dummies control for national trends.  

- **Mean DBT Change:**
  - Flood years ⬆️ ≈ higher DBT transfers  
  - Non-flood years ➡️ smaller or stable changes  

- **States most affected:**
  - 🌊 **West Bengal**, **Assam**, **Bihar**, **Karnataka**, **Rajasthan**  

---

## 📊 Visualization Summary

| Plot Type | Insight |
|------------|----------|
| **Scatter plots** | DBT vs. Flood intensity correlation |
| **Heatmaps** | DBT change (YoY) across states & years |
| **Bar charts** | Top 10 states with DBT rise during floods |
| **Dual-axis line charts** | DBT vs Flood trends (per state) |
| **Pie charts** | DBT share distribution |
| **Regression lines** | Direction & strength of correlation |

---

## 🧠 Key Observations
- Strong positive correlation between **transaction count** and **transfer amount**.  
- **High flood years** generally show increased DBT transfers.  
- Right-skewed distributions indicate a few districts dominate DBT activity.  
- Top DBT states: **Maharashtra**, **Uttar Pradesh**, **Bihar**.  
- Top flood-affected states: **West Bengal**, **Assam**, **Karnataka**.

---

## ✅ Conclusion
The project finds **consistent evidence that flood events correspond to higher DBT disbursements**, suggesting that the DBT mechanism in India plays a key role in **post-disaster response** and **social protection**.

> 💡 *Policy implication:* Strengthening DBT infrastructure in disaster-prone regions can ensure faster and fairer financial relief distribution.

---

## 🧭 Future Scope
- Add rainfall and crop-loss datasets for richer causal inference.  
- Use **panel data regression** or **difference-in-differences** models.  
- Explore **spatial mapping** (GeoPandas / Folium).  
- Automate visual dashboards using **Plotly Dash / Streamlit**.

---

## 🧰 Tech Stack
- **Python**
- **Pandas**, **NumPy**
- **Matplotlib**, **Seaborn**, **Plotly**
- **Scikit-learn**
- **Statsmodels**
- **TensorFlow / Keras**
- **Jupyter / Google Colab**

---

## 👩‍💻 Author
**Shrishti Prasad (7th Sem B.Tech CSE)**  
📧 shrishtiprasad435@gmail.com  
💼 *Passionate about Data Science, Machine Learning, and Public Policy Analytics.*

---

