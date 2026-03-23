# Identifying At-Risk Customers with Logistic Regression

**Khin Chan Thar**

---

## 📌 Overview

This project applies a Generalised Linear Model (Logistic Regression) to predict bank customer churn based on whether a customer will leave the bank by using demographic and behavioural data from 10,000 retail banking customers. The full pipeline covers exploratory data analysis, feature engineering, stepwise model selection, diagnostic validation, odds ratio interpretation, and performance evaluation on a held-out test set.

---

## 🗂️ Dataset

| Property | Detail |
|---|---|
| Source | Sharmaroshan (2019) — Churn Modelling Dataset via GitHub |
| Records | 10,000 unique customer records |
| Original Features | 14 variables |
| Features Used | 10 predictors (after feature selection) |
| Target | `Exited` (Binary: 1 = Churned, 0 = Retained) |
| Class Split | ~79.6% Retained / ~20.4% Churned |

**Predictor Variables:**

| Variable | Type | Description |
|---|---|---|
| CreditScore | Continuous | Customer credit score |
| Geography | Categorical | France, Germany, Spain |
| Gender | Categorical | Male / Female |
| Age | Continuous | Customer age |
| Tenure | Continuous | Years with the bank |
| Balance | Continuous | Account balance |
| NumOfProducts | Continuous | Number of bank products held |
| HasCrCard | Binary | Has credit card (Yes/No) |
| IsActiveMember | Binary | Active membership status |
| EstimatedSalary | Continuous | Estimated annual salary |
| Age_Group *(engineered)* | Categorical | Young Adult / Middle Aged / Senior |

---

## 📝 Methodology

### 🛠️ Data Preparation
- Dropped irrelevant identifiers (`RowNumber`, `CustomerId`, `Surname`)
- Converted categorical variables to factors (`Geography`, `Gender`, `HasCrCard`, `IsActiveMember`, `Exited`)
- **Feature Engineering:** Created `Age_Group` (Young Adult ≤30, Middle Aged ≤50, Senior 50+)
- **Train/Test Split:** 70% training (7,001 records) / 30% test (2,999 records), stratified by churn status

### 🕵 Exploratory Data Analysis
- Churn distribution pie chart (20.4% churn rate confirmed)
- Churn rate by Geography — Germany shows notably higher churn
- Churn rate by Age Group — Senior customers churn at higher rates
- Age distribution histogram by churn status
- Balance distribution boxplot by churn status
- Correlation heatmap of numeric variables

### 📊 Model Fitting
- **Initial Model:** Full logistic regression GLM with all 11 predictors (binomial family)
- **Model Selection:** Stepwise AIC (`stepAIC`, bidirectional) to remove non-significant predictors
- **Optimal Model Predictors:** CreditScore, Geography, Gender, Age, Balance, NumOfProducts, IsActiveMember, Age_Group

### 🩺 Model Diagnostics
- **VIF (Variance Inflation Factor):** All predictors below threshold — no significant multicollinearity detected
- **Cook's Distance:** Influential observations identified at threshold 4/N = 0.00057; flagged points visualised
- **QQ Plot of Deviance Residuals:** Assessed normality of model residuals

### 🔍 Model Evaluation
- Confusion matrix at 0.5 probability threshold
- ROC curve and AUC on held-out test set
- Odds Ratios with 95% confidence intervals for substantive interpretation

---

## 📣 Results

### 📈 Model Performance

| Metric | Value |
|---|---|
| Accuracy | 80.83% |
| Sensitivity (Non-churners) | 96.40% |
| **Specificity (Churners)** | **19.97%** |
| AUC | 0.777 |

### 📊 Confusion Matrix

|  | Predicted: Retained | Predicted: Churned |
|---|---|---|
| **Actual: Retained** | 2,302 | 86 |
| **Actual: Churned** | 489 | 122 |

### 🔎 Key Finding: Class Imbalance Problem
While the model achieves 80.83% overall accuracy, its **Specificity of only 19.97%** means it misses approximately **80% of true churners**. This is a direct consequence of the 79.6/20.4 class imbalance — the MLE method in standard GLMs biases predictions toward the majority class.

### 🎯 Odds Ratio Highlights

| Predictor | Odds Ratio | Interpretation |
|---|---|---|
| **IsActiveMember** | **0.341** | Active members are 66% less likely to churn — strongest protective factor |
| **GeographyGermany** | **2.131** | German customers are 2x more likely to churn vs France |
| **Age** | **1.069** | Each additional year of age increases churn odds by ~7% |
| **GenderMale** | **0.594** | Male customers are ~41% less likely to churn than females |
| **Balance** | **1.000002** | Higher balances marginally increase churn risk |
| NumOfProducts | 0.875 | More products slightly reduce churn probability |

---

## 📝 Conclusion & Recommendations

The logistic regression model establishes a statistically significant relationship between customer profile variables and churn, with a moderate AUC of 0.777. However, the low Specificity (19.97%) makes it unsuitable for direct deployment in a proactive retention system — the majority of high-risk customers would not be flagged.

**Recommended next steps:**
1. Apply **SMOTE** (Synthetic Minority Over-sampling Technique) to address class imbalance and improve churner detection
2. Explore **cost-sensitive learning** to penalise false negatives more heavily
3. Lower the classification threshold below 0.5 to trade some specificity for sensitivity
4. Benchmark against ensemble methods (Random Forest, XGBoost) for comparison

---

## Tech Stack

- **Language:** R
- **Key Packages:** `ggplot2`, `MASS`, `caret`, `dplyr`, `car`, `pROC`, `ggcorrplot`

---

## Project Structure

```
├── 14684190_Khin_Chan_Thar.docx   # Full written report
├── MA2405_.docx                    # R code output and model results
└── README.md                       # This file
```

---

## Data Source

Sharmaroshan. (2019). *Churn Modelling Dataset*. GitHub. https://github.com/sharmaroshan/Churn-Modelling-Dataset

---

## Author

**Khin Chan Thar**  
Customer Churn Prediction — GLM Analysis Project
