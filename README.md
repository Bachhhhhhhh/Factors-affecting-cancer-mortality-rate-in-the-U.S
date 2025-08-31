# Factors-affecting-cancer-mortality-rate-in-the-U.S

## 1.0 Project Overview

This project investigates the relationship between various socio-economic, demographic, and health-related factors and lung cancer mortality rates across counties in the United States. A multiple linear regression model was developed using the `statsmodels` library in Python to identify key predictors of cancer mortality.

The primary goal is to build a statistically significant model that explains the variance in cancer mortality rates, providing insights into which public health and economic indicators are most strongly correlated with this critical health outcome.


## 2.0 Methodology

The project followed a structured data science workflow, from data acquisition and cleaning to model building and interpretation.

### 2.1 Data Acquisition and Preparation
*   **Data Loading:** Loaded 50+ individual CSV files from various directories.
*   **Data Cleaning:**
    *   Standardized and padded county FIPS codes to ensure a consistent 5-digit format for accurate merging.
    *   Renamed columns from technical Census IDs (e.g., `B17001_002`) to descriptive names (e.g., `All_Poverty`).
    *   Handled missing or suppressed data, which were represented by non-numeric characters like `*`. For instance, `Mortality_Rate` entries with `*` (indicating fewer than 16 annual cases) were filtered out to ensure model integrity.
    *   Imputed a single null value in `Med_Income` with the column's median.

### 2.2 Feature Engineering
*   **Per Capita Conversion:** Converted absolute counts for poverty and health insurance into per-capita rates (per 100,000 population) to normalize the data and make them comparable to the `Mortality_Rate` target variable. These new features were suffixed with `_PC`.
*   **Dummy Variables:** Transformed the categorical `Recent Trend` variable (with values 'rising', 'falling', 'stable') into numerical dummy variables (`Rising`, `Falling`) to be used in the regression model.
*   **Log Transformation:** Applied a natural log transformation to `Med_Income` to address non-linearity observed in its relationship with the mortality rate during exploratory analysis.

### 2.3 Exploratory Data Analysis (EDA)
*   Visualized relationships between key variables using scatterplots and correlation heatmaps (`seaborn.pairplot`, `seaborn.heatmap`).
*   This analysis revealed high multicollinearity between gender-specific poverty variables (`M_Poverty_PC`, `F_Poverty_PC`) and the overall poverty rate (`All_Poverty_PC`), leading to their removal from the final model.

### 2.4 Model Building and Diagnostics
*   **Initial Model:** An initial Ordinary Least Squares (OLS) regression model was built using `statsmodels.api.OLS` with all engineered features.
*   **Multicollinearity Check:** Variance Inflation Factors (VIFs) were calculated to diagnose multicollinearity. Features with high VIF scores (e.g., `POPESTIMATE2015`, `All_With`) were iteratively removed to improve model stability.
*   **Final Model:** A refined model was selected based on a combination of statistical significance (p-values), logical coherence of coefficients, and a final VIF check.
*   **Heteroscedasticity Test:** The Breusch-Pagan test was performed on the final model's residuals to check for homoscedasticity, a key assumption of linear regression.

## 3.0 Results and Interpretation

The final linear regression model achieved an **Adjusted R-squared of 0.737**, indicating that it successfully explains approximately 73.7% of the variance in cancer mortality rates across U.S. counties.

### 3.1 Final Model Summary
*   **Dependent Variable:** `Mortality_Rate` (per 100,000 population)
*   **Independent Variables:** `All_Without_PC`, `Incidence_Rate`, `Falling`, `Med_Income_log`, `All_Poverty_PC_squared`

### 3.2 Key Findings
The model's coefficients reveal several significant predictors:

*   **Incidence Rate (`Incidence_Rate`):** This was the strongest predictor. For each one-unit increase in the cancer incidence rate, the mortality rate is expected to increase by approximately **0.65**, holding other factors constant. This highlights the direct link between diagnosis rates and mortality.
*   **Median Income (`Med_Income_log`):** This factor was highly significant and negatively correlated. A logarithmic increase in a county's median income is associated with a decrease of approximately **8.62** in the cancer mortality rate. This suggests that higher economic status is a strong protective factor.
*   **Lack of Health Insurance (`All_Without_PC`):** For every 100-person increase in the population without health insurance (per 100,000), the mortality rate is expected to increase by about **0.02** (0.0002 * 100). This indicates that lack of access to healthcare is a significant risk factor for higher cancer mortality.
*   **Poverty (`All_Poverty_PC_squared`):** The squared term for poverty was statistically significant, suggesting a non-linear relationship where the impact of poverty on mortality accelerates as the poverty rate increases.

The Breusch-Pagan test yielded a p-value of 0.128, failing to reject the null hypothesis of homoscedasticity. This result confirms that the model's residuals have a constant variance, satisfying a critical assumption for OLS regression.

## 4.0 Technologies Used
*   **Language:** Python 3
*   **Libraries:**
    *   **Data Wrangling:** Pandas, NumPy
    *   **Statistical Modeling:** Statsmodels
    *   **Visualization:** Matplotlib, Seaborn
