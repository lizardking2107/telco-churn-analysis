# Telco Customer Churn Analysis

In this project I analyzed the customers of a telecom company and tried to find out why they leave and what the company can do about it. The data is taken from Kaggle. 

## Project Goal
The main goal of the project is to analyze the customers' base of Telco and identify what kind of factors motivate the clients to churn. Moreover we focus on what business can do about it. To conduct the research I combined several approaches: SQL, statistical testing, logistic regression and data visualization. They turned into one coherent analysis rather than stopping at one analytical metric.

## Pipeline

1. First, I checked the data quality. The `TotalCharges` column was stored as text, and 11 new customers had a blank space instead of a number, so I filled it with 0.
2. Then I loaded the data into SQLite and calculated the churn rate for different groups of customers (contract type, payment method, tenure).
3. After that I built several charts to see the class balance and the differences between the groups.
4. Then I tested the differences with Levene's test, Welch's t-test and chi-square.
5. In the end I built a logistic regression. It turned out that some variables were too correlated with each other, so I checked it with VIF and removed `MonthlyCharges`.

## Key Findings

The most important factor is the contract type. The churn rate of the customers with monthly contracts is 42.7%, while for one-year and two-year contracts it is 11.3% and 2.8%. According to the regression, a two-year contract reduces the odds of churn by about 76%.

Tenure also matters. Customers who have been with the company less than a year leave in 47.7% of cases, and for those who stay 4+ years it is only 9.5%.

Customers who pay by electronic check churn at 45.3%, which is much more than for automatic payments. It may be because electronic checks are paid manually every time, so it is easier to stop paying.

Fiber optic customers leave more often than DSL customers (odds ratio is about 1.95). I assume the reason is the price: the average monthly charge is 91.5 for fiber optic and 58.1 for DSL.

Additional services (OnlineSecurity, TechSupport, OnlineBackup) are connected with lower churn. In the first model I could not see it, because `MonthlyCharges` was too correlated with these variables (VIF up to 872). After I removed it, the effect became visible.

## Model Performance

The ROC-AUC of the model is 0.84. The recall for the churn class is 0.55, so the model finds a bit more than half of the customers who really left (with the standard threshold 0.5). If the threshold is lowered to 0.3, the recall grows to 0.78, but more customers are marked as churn by mistake.

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
