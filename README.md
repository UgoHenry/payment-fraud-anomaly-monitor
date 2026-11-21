# Payment Fraud & Anomaly Detection Monitor (Static Rule vs. Isolation Forest)

## 1. Business Objective
Detect fraudulent payment transactions in real time without overwhelming Risk Operations teams with false-positive alerts. This project benchmarks a traditional compliance threshold rule (`Amount > EUR 1,500`) against an unsupervised **Isolation Forest** anomaly detection model across **284,807** cardholder transactions (`492` actual fraud cases; `0.173%` fraud incidence; `EUR 60,127.97` total fraud exposure).

## 2. Repository Contents
* **`payment_fraud_dual_path.ipynb`**: End-to-end Google Colab notebook contrasting Path 1 (static SQL/compliance threshold rule) against Path 2 (unsupervised `IsolationForest` trained across all anonymized PCA behavioral features `V1–V28` and transaction `Amount`).
* **`Flagged_Suspicious_Transactions.csv`**: Prioritized investigation queue of the top **500** most anomalous transactions ranked by lowest `Anomaly_Score` for Risk Operations review.
* **`fraud_detection_comparison.png`**: Side-by-side benchmark chart illustrating the massive jump in both Fraud Recall and Alert Precision achieved by unsupervised ML.

## 3. Dual-Track Comparison: Static Threshold Rule vs. Machine Learning
| Approach | Methodology | Alerts Triggered | Fraud Caught (Recall) | Alert Precision | Fraud Volume Caught |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Path 1: Static SQL Threshold Rule** | Hardcoded rule (`Amount > EUR 1,500`) | **1,334** | **0.6%** | **0.2%** | **EUR 5,440.48** |
| **Path 2: Unsupervised Isolation Forest** | Unsupervised ensemble (`150` trees, `contamination=0.002`) across `V1–V28` + `Amount` | **570** | **31.7%** | **27.4%** | **EUR 15,105.46** |

## 4. Key Insights & Risk Operations Recommendations
* **50x Improvement in Fraud Detection with 57% Fewer Alerts:** Fraudsters routinely test stolen cards with small-to-medium amounts to evade static `EUR 1,500` thresholds. Isolation Forest detects multi-dimensional behavioral anomalies regardless of ticket size—lifting recall from `0.6%` to `31.7%` and precision from `0.2%` to `27.4%` while cutting analyst alert fatigue by more than half (`570` vs. `1,334` alerts).
* **Risk Queue Triage:** Deploy `Anomaly_Score` thresholds in payment gateways to auto-step-up authentication (3DS) on the top `0.2%` tail while routing `Flagged_Suspicious_Transactions.csv` to fraud analysts.

## 5. Tech Stack & Dataset
* **Tools:** Python (`pandas`, `numpy`, `scikit-learn`, `matplotlib`), Google Colab
* **Dataset:** [Credit Card Fraud Detection (`creditcard.csv` — Kaggle / ULB Machine Learning Group)](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
