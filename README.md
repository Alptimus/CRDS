# Credit Risk Decision Engine

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](#)
[![XGBoost](https://img.shields.io/badge/XGBoost-Model-orange)](#)
[![SHAP](https://img.shields.io/badge/SHAP-Explainability-green)](#)
[![scikit--learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-red)](#)

An end-to-end credit risk project built on the **Home Credit Default Risk** dataset.  
The project goes beyond default prediction and focuses on practical lending decisions through **risk ranking, business-loss optimization, expected loss estimation, stress testing, and explainability**.

## Project Objectives

- Predict **Probability of Default (PD)**
- Construct interpretable **risk bands**
- Optimize a **lending policy** using portfolio profit
- Estimate **Expected Credit Loss (ECL)** using `PD × LGD × EAD`
- Test portfolio resilience under stressed macroeconomic scenarios
- Monitor score stability using **Population Stability Index (PSI)**
- Explain model outputs using **SHAP**

## Dataset

This project uses the **[Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/)** dataset with applicant-level features built from:

- application data
- bureau history
- previous applications
- installments
- POS cash balance
- credit card balance

## Modeling Approach

### Baseline Model
- Logistic Regression for interpretability and benchmarking

### Main Model
- XGBoost for stronger predictive performance on tabular credit data

## Key Results

- **Test AUC:** 0.7858
- **Test Gini:** 0.5715
- **Test KS:** 0.4309

## Risk Bands

Borrowers are grouped into the following bands:

- Very Low Risk
- Low Risk
- Moderate Risk
- High Risk
- Very High Risk

These bands support underwriting actions such as:

- Approve
- Review
- Reject

## Business Policy Optimization

Rather than using a fixed probability cutoff such as 0.5, the project learns a lending threshold based on:

- portfolio profit
- approval rate
- bad rate among approved loans
- risk constraints

This makes the model useful for real lending policy decisions.

## Expected Credit Loss

The project estimates portfolio loss using a simplified credit-loss framework:

\[
ECL = PD \times LGD \times EAD
\]

This is used to evaluate portfolio exposure under different loss-severity assumptions.

## Stress Testing

The portfolio is evaluated under three macroeconomic scenarios:

- Normal
- Mild Recession
- Severe Recession

For each scenario, the model adjusts:

- PD upward
- LGD upward
- lending margin downward

This shows how portfolio profitability changes under stress.

## Monitoring and Drift Detection

The project uses **PSI** to compare the development score distribution with the monitored population.  
This helps identify whether the model’s score distribution has shifted significantly over time.

## Explainability

The project uses **SHAP** to explain the XGBoost model at both global and local levels.

It provides:

- global feature importance
- borrower-level explanations
- direction of risk contribution

Logistic Regression coefficients are also reported as a benchmark for linear interpretability.

## Repository Structure

```text
CRDS/
│
├── credit-risk-decision-system.ipynb
├── README.md
├── requirements.txt
├── artifacts/
├── plots/
├── REPORT.md
├── SUMMARY.md
└── LICENSE

```

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/Alptimus/CRDS.git
cd credit-risk-decision-engine
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Open the notebook
```bash
jupyter credit-risk-decision-system.ipynb
```

### 4. Run the notebook
Execute the notebook cells in order to reproduce:

- feature engineering
- model training
- calibration
- risk bands
- ECL
- PSI
- stress testing
- SHAP explainability

## Main Outputs

The notebook produces:

- model metrics
- risk band summaries
- ECL tables and plots
- PSI analysis
- stress test outputs
- SHAP explanations
- coefficient-based interpretation for Logistic Regression

## Tech Stack

- Python
- Pandas
- NumPy
- scikit-learn
- XGBoost
- SHAP
- Matplotlib

## Summary

This project demonstrates a complete credit risk workflow that combines:

- predictive modeling
- business decision-making
- loss estimation
- monitoring
- explainability

It is designed to be suitable for roles in credit risk, lending analytics, financial data science, and regulated decision systems.

---

[MIT License](LICENSE)