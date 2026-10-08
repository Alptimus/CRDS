# Credit Risk Decision Engine  
### PD Modeling, Risk Bands, Expected Loss, Stress Testing, and Explainability

This project builds an end-to-end **credit risk decision engine** on the Home Credit Default Risk dataset.  
The goal is not just to predict default, but to convert borrower risk into a **lending policy** that balances profit, loss, and portfolio quality.

The notebook-based pipeline covers:

- **Probability of Default (PD)** prediction
- **Risk banding** for borrowers
- **Business-profit threshold optimization**
- **Expected Credit Loss (ECL)** estimation
- **Population Stability Index (PSI)** monitoring
- **Stress testing** under macroeconomic scenarios
- **SHAP-based explainability** for model interpretation

---

## Business Objective

A lender does not only need a model that ranks borrowers by risk.  
It also needs to answer:

- Should this loan be approved, reviewed, or rejected?
- What threshold gives the best portfolio profit?
- How much loss should be expected under normal and stressed conditions?
- Is the model stable enough for deployment?
- Why is a specific borrower considered risky?

This project was designed to answer those questions.

---

## Dataset

The project uses the **Home Credit Default Risk** dataset.  
The dataset includes a main application table and several auxiliary tables such as:

- bureau history
- previous applications
- installments
- POS cash balance
- credit card balance

These tables were merged and transformed into applicant-level features.

---

## Feature Engineering

The notebook creates credit-risk features such as:

- credit-to-income ratio
- annuity-to-income ratio
- credit term
- payment burden
- employment length
- external score aggregates
- bureau delinquency indicators
- prior loan quality indicators
- installment behavior measures
- POS and credit card behavior measures

These features were used to build a richer borrower-level representation.

---

## Modeling Approach

### Baseline model
A **Logistic Regression** model was trained as the interpretable benchmark.

### Main challenger model
An **XGBoost** model was trained as the main predictive model.

### Evaluation metrics
The model was evaluated using:

- ROC-AUC
- PR-AUC
- Gini
- KS statistic
- Brier score
- precision
- recall
- F1-score

### Calibration
The model probabilities were calibrated so the outputs could be used as meaningful PD estimates for business logic.

---

## Key Results

### Model discrimination on held-out test data
The final XGBoost model achieved:

- **AUC:** 0.7858  
- **Gini:** 0.5715  
- **KS:** 0.4309  

This indicates strong ranking power for default risk.

### Risk bands
Borrower PDs were converted into five risk bands:

- Very Low Risk
- Low Risk
- Moderate Risk
- High Risk
- Very High Risk

These bands showed a monotonic increase in default rate, which confirms that the banding logic is meaningful for underwriting.

### Explainability
SHAP results showed that the most important drivers included:

- `EXT_SOURCE_MEAN`
- installment delinquency behavior
- prior refusal rate
- credit term
- credit-to-income related variables
- employment stability variables
- bureau and prior application behavior

This made the model explainable at both the global and individual borrower level.

---

## Business-Profitable Lending Policy

Instead of using a fixed threshold like 0.5, the notebook learns a **business-constrained lending policy**.

The policy uses:

- PD cutoff
- review band
- approval / reject logic
- expected revenue
- expected default loss
- operating cost
- review cost

The final validation policy produced a realistic triage outcome:

- **Approval rate:** 74.75%
- **Review rate:** 3.10%
- **Reject rate:** 22.15%
- **Bad rate among approved:** 4.01%

This is much closer to a real lending policy than a plain classification cutoff.

---

## Expected Credit Loss (ECL)

A simplified **IFRS 9-style ECL engine** was built using:

\[
ECL = PD \times LGD \times EAD
\]

The risk bands were mapped into stages for provisioning logic:

- Very Low / Low Risk → Stage 1
- Moderate Risk → Stage 2
- High / Very High Risk → Stage 3

### Base ECL by stage
- Stage 1: 346,199,539
- Stage 2: 840,667,430
- Stage 3: 1,323,047,261

### Total ECL under LGD scenarios
- **Low LGD:** 1,952,155,512
- **Base LGD:** 2,509,914,230
- **Stress LGD:** 3,346,552,306

ECL increased monotonically with risk stage and under harsher LGD assumptions, which is the expected behavior.

---

## Stress Testing

The notebook simulates three macroeconomic scenarios:

- Normal
- Mild Recession
- Severe Recession

Under stress, the model increases PD and LGD while reducing lending margin.

### Stress test results
- **Normal profit:** 1,799,033,920
- **Mild recession profit:** 591,061,852
- **Severe recession profit:** -929,426,648

### Interpretation
The portfolio remains profitable under normal conditions, compresses sharply under mild recession, and becomes loss-making under severe recession.  
This shows the model is sensitive to macro deterioration, which is exactly what a stress test should reveal.

---

## Population Stability Index (PSI)

PSI was used to compare the development score distribution with the monitored population.

### Result
- **PSI score:** 5.5261

This is a very large value and indicates a **major distribution shift**.  
In practical terms, the test population is scoring much lower than the development population, so the model would require recalibration or review before production deployment.

---

## Explainability Summary

The SHAP analysis showed:

### Global drivers
The strongest global drivers were:
- `EXT_SOURCE_MEAN`
- installment delinquency features
- previous refusal rate
- credit term
- POS repayment behavior
- employment duration
- bureau history

### Local explanation
For one high-risk borrower:
- **Predicted PD:** 43.9%
- **Risk band:** Very High Risk
- **Policy cutoff:** 0.542

Main risk-increasing factors included:
- `EXT_SOURCE_MEAN`
- `EXT_SOURCE_PROD`
- `CREDIT_TERM`
- `prev_refusal_rate`

Main risk-reducing factors included:
- `pos_cnt_instalment_mean`
- `FLAG_OWN_CAR`
- bureau credit history variables
- previous loan quality indicators

The Logistic Regression coefficients broadly supported the same credit-risk story.

---

## Final Story of the Project

This notebook demonstrates a complete credit risk workflow:

1. build borrower-level features from multiple loan-history tables  
2. train a default-risk model  
3. calibrate predicted probabilities  
4. convert PD into risk bands  
5. optimize a lending policy for profit and risk  
6. estimate ECL under provisioning scenarios  
7. test the portfolio under macro stress  
8. monitor score stability with PSI  
9. explain individual decisions with SHAP  

The result is not just a model, but a **credit decision engine**.

---