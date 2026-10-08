# Credit Risk Decision Engine:  
## Probability of Default Modeling, Risk Banding, Expected Loss Estimation, Stress Testing, and Explainability

### Project Report

## Abstract

This project develops an end-to-end credit risk decision engine using the Home Credit Default Risk dataset. The objective is not only to estimate the probability that a borrower will default, but also to translate model outputs into actionable lending decisions that reflect portfolio profitability, risk appetite, and monitoring requirements. The workflow integrates borrower-level feature engineering, default prediction, probability calibration, risk band construction, lending policy optimization, expected credit loss estimation, population stability analysis, stress testing, and model explainability. The final system demonstrates how machine learning can support credit underwriting in a practical and business-oriented manner.

## 1. Introduction

Credit risk modeling is a core function in lending institutions, where the objective is to assess borrower quality and manage portfolio losses. Traditional classification models often focus only on predictive accuracy, but in a lending context, a useful model must also support decision-making. In particular, lenders need to know not only whether a borrower is risky, but also how much exposure should be approved, how losses should be estimated, and how the portfolio would behave under changing economic conditions.

This project addresses these needs by building a credit risk decision engine that combines predictive modeling with financial decision logic. The system estimates borrower-level default probability, groups applicants into interpretable risk bands, optimizes approval thresholds based on business profit, estimates expected credit loss, monitors score stability, and explains model predictions using SHAP.

## 2. Dataset

The project uses the **Home Credit Default Risk** dataset, which is well suited for retail credit modeling because it contains applicant-level data and multiple auxiliary tables describing borrower history. The following data sources were used:

- main application table
- bureau and bureau balance tables
- previous application history
- installment payment records
- POS cash balance records
- credit card balance records

These tables were merged to produce a borrower-level analytical dataset. The final feature space includes affordability indicators, prior credit behavior, delinquency patterns, repayment consistency, and external score information.

## 3. Feature Engineering

Feature engineering was a critical part of the project. The merged dataset was transformed into interpretable credit-risk variables that reflect underwriting logic. Key features included:

- credit-to-income ratio
- annuity-to-income ratio
- credit term
- payment burden
- income per household member
- employment duration
- external score aggregates
- bureau delinquency indicators
- prior loan quality metrics
- installment repayment behavior
- POS and credit card behavior summaries

These features were designed to capture capacity, stability, and historical repayment behavior, which are central to credit risk assessment.

## 4. Modeling Methodology

Two models were used in the project.

### 4.1 Logistic Regression
Logistic Regression was used as the baseline interpretable model. It provides a transparent benchmark and allows direct coefficient interpretation.

### 4.2 XGBoost
XGBoost was used as the main predictive model because of its strong performance on tabular data and ability to capture nonlinear relationships and interactions among borrower attributes.

The models were evaluated using standard discrimination and calibration metrics, including:

- ROC-AUC
- PR-AUC
- Gini coefficient
- KS statistic
- Brier score
- precision
- recall
- F1-score

Probability calibration was applied to improve the reliability of predicted default probabilities.

## 5. Model Performance

The final XGBoost model showed strong risk-ranking capability on held-out test data, with:

- **AUC:** 0.7858
- **Gini:** 0.5715
- **KS:** 0.4309

These values indicate that the model is able to separate high-risk and low-risk borrowers effectively.

The Logistic Regression baseline performed substantially worse, which confirmed that the problem contains meaningful nonlinear structure that is better captured by gradient boosting.

## 6. Risk Band Construction

To make the model more interpretable for business use, borrower PDs were converted into five risk bands:

- Very Low Risk
- Low Risk
- Moderate Risk
- High Risk
- Very High Risk

These bands were defined using PD thresholds and then validated by observed default rates. The results showed a monotonic increase in default probability across the bands, confirming that the banding structure was aligned with actual risk.

This step allowed the model output to be translated into a clear underwriting language that could support approval, review, and rejection decisions.

## 7. Lending Policy Optimization

Rather than using a fixed classification cutoff such as 0.5, the project optimized the lending policy using a business-profit framework. The policy considered:

- approval threshold
- review band
- default loss
- operating cost
- review cost
- expected revenue from performing loans

A risk-constrained optimization approach was used to prevent the model from approving nearly all borrowers. The final validation policy produced a realistic triage outcome:

- **Approval rate:** 74.75%
- **Review rate:** 3.10%
- **Reject rate:** 22.15%
- **Bad rate among approved:** 4.01%

This demonstrates that the model can be used not only to rank borrowers but also to support practical credit decisions.

## 8. Expected Credit Loss Estimation

A simplified IFRS 9-style expected credit loss engine was developed using the relationship:

\[
ECL = PD \times LGD \times EAD
\]

Risk bands were mapped to simplified stages for provisioning purposes:

- Very Low Risk and Low Risk → Stage 1
- Moderate Risk → Stage 2
- High Risk and Very High Risk → Stage 3

The ECL analysis showed increasing loss levels by stage and under higher LGD scenarios. The total base ECL was:

- **Stage 1:** 346,199,539
- **Stage 2:** 840,667,430
- **Stage 3:** 1,323,047,261

Total ECL under LGD scenarios was:

- **Low LGD:** 1,952,155,512
- **Base LGD:** 2,509,914,230
- **Stress LGD:** 3,346,552,306

These results show that the portfolio loss estimate responds appropriately to risk and severity assumptions.

## 9. Stress Testing

The project included macroeconomic stress testing under three scenarios:

- Normal
- Mild Recession
- Severe Recession

For each scenario, PD was increased, LGD was increased, and lending margin was reduced. The resulting expected portfolio profit was:

- **Normal:** 1,799,033,920
- **Mild Recession:** 591,061,852
- **Severe Recession:** -929,426,648

This result is important because it shows that the portfolio remains profitable under normal conditions but becomes loss-making in a severe downturn. The stress test therefore demonstrates how macroeconomic deterioration can materially affect lending outcomes.

## 10. Population Stability Index

Population Stability Index was used to compare the development score distribution with the monitored distribution. The PSI value was:

- **PSI = 5.5261**

This indicates a very large distribution shift. In practical terms, the monitored sample was scoring much lower than the development sample. This suggests that the model would require recalibration, review, or possible redevelopment before deployment in a real production environment.

The PSI analysis strengthens the project by showing that model performance alone is not sufficient; score stability must also be monitored.

## 11. Explainability

SHAP was used to provide both global and local explanations of the XGBoost model. The global feature importance results identified the following key drivers:

- external score aggregates
- installment delinquency
- prior refusal rate
- credit term
- repayment history
- employment duration
- bureau and previous application behavior

For a selected high-risk borrower, SHAP showed that the model assigned a PD of 43.9%, placing the borrower in the Very High Risk band. The local explanation revealed which features increased the risk score and which features reduced it. Logistic Regression coefficients were also reported as a linear interpretability benchmark.

This explainability layer ensures that the project is not merely predictive, but also interpretable and suitable for credit decision support.

## 12. Conclusion

This project demonstrates a full credit risk modeling pipeline that combines machine learning, business optimization, loss estimation, monitoring, and explainability. The final system goes beyond standard classification by converting predictions into lending actions, expected loss, and stress-sensitive portfolio outcomes.

The project shows that:

- borrower default risk can be estimated effectively using XGBoost;
- risk bands provide a practical underwriting framework;
- profitability depends heavily on the lending policy;
- stress conditions can turn a profitable portfolio into a loss-making one;
- PSI can reveal significant score instability;
- SHAP provides borrower-level and model-level interpretability.

Overall, the project presents a practical and financially meaningful approach to credit risk analytics.

## 13. Future Improvements

The project could be extended in several ways:

- include a fully productionized borrower scoring interface
- add fair lending / bias diagnostics
- improve calibration under distribution shift
- use real time-based out-of-time validation
- refine the ECL engine with richer recovery assumptions
- incorporate macroeconomic variables directly into stress modeling
- add automated monitoring for model drift and threshold refresh

---