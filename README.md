# Telco Customer Churn Analysis

End-to-end churn analysis on the [Telco Customer Churn dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) (Kaggle) — from raw data to a business-ready retention strategy.

## Project Goal
The main goal of the project is to analyze the customers' base of Telco and identify what kind of factors motivate the clients to churn. Moreover we focus on what business can do about it. To conduct the research I combined several approaches: SQL, statistical testing, logistic regression and data visualization. They turned into one coherent analysis rather than stopping at one analytical metric.

## Pipeline

1. **EDA** — data validation, hidden missing values (`TotalCharges` stored as whitespace strings for new customers), category consistency checks.
2. **SQL** — churn rate by segment (contract type, payment method, tenure bracket) via SQLite, including window functions for risk ranking and share-of-base sizing.
3. **Visualization** — class balance, churn rate by segment, correlation structure among numeric features.
4. **Statistical testing** — Levene's test + Welch's t-test (tenure, MonthlyCharges by churn), chi-square (Contract, PaymentMethod vs churn).
5. **Modeling** — logistic regression (`statsmodels`), including a VIF-based multicollinearity diagnosis and fix, evaluated via ROC-AUC, precision/recall, and confusion matrix (not just accuracy, given ~27% class imbalance).

## Key Findings

- **Contract type is the main churn driver.** The churn rate of the customers with monthly subscriptions is at 42.7%, vs. 11.3% (one-year) and 2.8% (two-year). A two-year contract reduces churn odds by ~76% relative to month-to-month (according to the logistic regression).
- **Tenure also defines the churn rates.** Churn is 47.7% for customers under 12 months, dropping to 9.5% for 4+ year customers.
- **Electronic check payers churn at 45.3%**, roughly 3x the rate of automatic payment methods. It may be explained by the fact that electronic checks are paid manually, while the automatic payment methods are not controlled by the clients.
- **Fiber optic customers churn more than DSL**, even after controlling for other factors (odds ratio is about 1.95). I assume that the main reason for that is the cost (the average monthly charge for fiber optic clients is 91.5 vs 58.1 (DSL)).
- **Additional services reduce churn.** Customers with OnlineSecurity, TechSupport or OnlineBackup have significantly lower churn odds. In the first model this effect was not visible, because `MonthlyCharges` was strongly correlated with the service and internet features (VIF up to 872). After I removed `MonthlyCharges`, the VIF dropped below 11 and the effect became clear.

## Model Performance

- ROC-AUC: 0.84
- Recall (churn class): 0.55 — the model catches just over half of actual churners at the default 0.5 threshold, which matters for how a retention campaign built on these predictions would be sized.

## Business Recommendations

- The company should persuade month-to-month customers toward longer contracts (the single largest lever). Potential practical solution: the company may offer a discount for signing 1+ year contract.
- Prioritize retention efforts in the first 12 months of the customer lifecycle. Potential practical solution: the clients should be offered special tariffs (more beneficial ratio of the utility to the price). 
- Investigate the Fiber optic churn gap — pricing competitiveness vs. service quality. Probably the fiber optic is not proportional to the amount of money paid. 
- Encourage a shift away from electronic check toward automatic payment methods. Potential practical solution: encourage the customers by offering them a small one-time bonus for enabling autopay (for example ApplePay) and make it a default option even when the customer renews or signs up.
- Position security/support add-ons as retention tools, not just as additional revenue from sales. Potential practical solution: offer to at-risk clients a free trial period of those services.

## Tech Stack

Python, pandas, SQLite, scipy, statsmodels, scikit-learn, matplotlib/seaborn

## Dataset

[Telco Customer Churn — Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn), downloaded via `kagglehub`.
