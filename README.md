# 🗺️ Spatial Autoregressive Model (SAR) for Poverty Analysis in Indonesia (2024)

This repository contains the dataset, R code, and analytical findings for modeling the Percentage of Poor People (*Persentase Penduduk Miskin* / PPM) across 38 provinces in Indonesia using Spatial Regression techniques.

---

## 📌 Background

Poverty remains a complex, multidimensional challenge in Indonesia's regional development. Traditional linear regression models often assume that observations across geographic areas are independent. However, regional poverty rates naturally exhibit **spatial dependence**—the poverty level in one province is frequently influenced by and correlated with its neighboring provinces (Tobler's First Law of Geography).

Ignoring spatial correlation in ordinary linear models can lead to violated independence assumptions, inefficient parameter estimations, and misleading conclusions. This project applies spatial econometric techniques to address spatial autocorrelation and identify the key socio-economic and infrastructure drivers of provincial poverty in Indonesia for the year 2024.

---

## 📁 Dataset & Variables

The study analyzes secondary data collected from **Statistics Indonesia (Badan Pusat Statistik / BPS) for 2024**, covering all **38 provinces** in Indonesia.

* **Response Variable ($Y$):** Percentage of Poor People (%)
* **Predictor Variables:**
  * **$X_1$**: Percentage of households owning their home (%)
  * **$X_2$**: Percentage of households without toilet facilities (%)
  * **$X_3$**: Percentage of households with access to safe drinking water (%)
  * **$X_4$**: Percentage of households with access to proper sanitation (%) *(removed during feature selection due to severe multicollinearity, VIF > 10)*
  * **$X_5$**: Percentage of households using PLN electricity (%)
  * **$X_6$**: Population density (people/km²)
  * **$X_7$**: Open unemployment rate (%)

---

## 🛠️ Methodology & Analytical Pipeline

The analysis was performed using **R 4.5.0** following a structured econometric framework:

1. **Exploratory Data Analysis (EDA):** Visualized poverty distribution using boxplots, bar charts, correlation matrices, and thematic choropleth maps.
2. **OLS Diagnostic Checks:**
   * Multicollinearity check using Variance Inflation Factor (VIF).
   * Outlier and leverage detection ($R_{\text{student}}$, $h_{ii}$, DFFITS).
   * Feature selection via Best Subset Selection based on AIC, Mallows' $C_p$, and Adjusted $R^2$.
3. **Spatial Weight Matrix Comparison:** Evaluated four spatial weight matrices to capture geographic connectedness across the Indonesian archipelago:
   * K-Nearest Neighbor (KNN, $k=3$)
   * Radial Distance Weight (RDW, $r=200\text{ km}^2$)
   * Inverse Distance Weight (IDW, $\alpha=1$)
   * **Exponential Distance Weight (EDW, $\alpha=1$)** ⭐ *Selected as best fit for island geography.*
4. **Spatial Dependence Testing:**
   * Global spatial autocorrelation using **Moran's I**.
   * Local spatial autocorrelation using **Local Indicators of Spatial Association (LISA)**.
   * Model specification test using **Lagrange Multiplier (LM)** tests (LM Lag vs. LM Error).
5. **Spatial Autoregressive (SAR) Modeling:** Estimated model parameters using the Maximum Likelihood Estimation (MLE) method and validated classical/spatial residual assumptions.

---

## 📊 Key Findings & Results

### 1. Spatial Autocorrelation & Clustering
* **Global Moran's I:** Confirmed significant positive spatial autocorrelation for the poverty rate ($I = 0.191, p < 0.05$) and OLS residuals ($I = 0.025, p = 0.005$), proving the necessity of spatial regression.
* **LISA Analysis:** Identified major **High-High (Hotspot)** poverty clusters heavily concentrated in eastern Indonesia, particularly across the provinces of Papua, Central Papua, Highland Papua, South Papua, West Papua, Southwest Papua, Central Sulawesi, Southeast Sulawesi, Gorontalo, Maluku, and NTT.

### 2. Model Selection: OLS vs. SAR
The **Spatial Autoregressive Model (SAR)** significantly outperformed the classical Ordinary Least Squares (OLS) model:

| Evaluation Metric | OLS Regression | Spatial Autoregressive (SAR) | Result |
| :--- | :---: | :---: | :---: |
| **Akaike Information Criterion (AIC)** | 217.59 | **214.17** | 🏆 Lower AIC |
| **Coefficient of Determination ($R^2$)** | 65.29% | **71.53%** | 🏆 Higher $R^2$ |
| **Significant Predictors ($p < 0.05$)** | 1 variable | **2 variables + Spatial Lag ($\rho$)** | 🏆 Higher explanatory power |
| **Residual Independence Assumption** | Violated ($p = 0.045$) | **Satisfied ($p = 0.420$)** | 🏆 All assumptions met |

### 3. Estimated SAR Equation & Interpretations
$$\widehat{\text{Poverty}} = 12.542 + 0.724(W \cdot Y) + 0.161(X_1) - 0.245(X_5)$$

* **Spatial Lag Coefficient ($\rho = 0.724, p = 0.019$):** Demonstrates a strong, positive spatial spillover effect. A region's poverty rate is directly and significantly influenced by the poverty levels of neighboring provinces.
* **Home Ownership ($X_1, \beta = 0.161, p = 0.029$):** Has a positive relationship with poverty rates, indicating that higher rates of home ownership in certain regional contexts do not automatically correlate with higher economic status.
* **PLN Electricity Access ($X_5, \beta = -0.245, p < 0.001$):** Has a strong negative relationship with poverty. A 1% increase in household electricity access estimates a **0.245% decrease** in the average provincial poverty rate.

---

## 💡 Conclusions & Recommendations

### 🎯 Conclusions
1. Provincial poverty in Indonesia cannot be analyzed as independent regional events; strong spatial spillovers exist ($\rho = 0.724$).
2. The **SAR model with an Exponential Distance Weight (EDW) matrix** provides the best fit, explaining **71.53%** of the variance in provincial poverty while satisfying all residual diagnostic tests.
3. Access to basic infrastructure—specifically reliable grid electricity ($X_5$)—is a major structural driver for poverty reduction.

### 🚀 Recommendations
* **Targeted Regional Interventions:** Policies should prioritize spatial hotspots (High-High clusters), particularly in Papua and parts of Sulawesi, using coordinated multi-provincial developmental strategies.
* **Infrastructure Investment:** Accelerate rural electrification and essential utility access in underdeveloped provinces to stimulate local economic growth.
* **Future Research:** Extend spatial modeling to district/city level (*Kabupaten/Kota*) data or implement spatio-temporal models (e.g., Spatial Panel, Geographically Weighted Regression) to capture localized micro-dynamics over time.

---

## 👥 Authors

* **Joshua Bryan Wijaya**
* **Luthfia Aiman Hanin**
* **Muhammad Fadhil Hakim**
* **Fachry Wirayuda Alindri**
* **Muhammad Bagas Ramadhan**
* **Muhammad Nur Aidi**

*Department of Statistics and Data Science, IPB University, Indonesia*
