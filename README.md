
# Platform Transaction Demand Classification
### IT3388 – Big Data Management Project

## Project Overview

This project focuses on analysing and predicting daily transaction demand for an anonymised Colombian fintech company using Big Data technologies and machine learning.

**My Assigned Workstream: Platform Transaction Demand**

My responsibility was to develop a predictive solution that classifies the next day's platform transaction demand into three categories: **Low, Medium and High**.

**Business Problem:**

Historical transaction records describe past platform activity but do not provide a forward-looking prediction of upcoming transaction demand. Without demand predictions, the fintech company may face difficulties planning infrastructure capacity, staffing and operational resources during peak transaction periods.

**Business Question:**

Can the next day's platform transaction demand be classified as Low, Medium or High using historical transaction activity, transaction-type composition, calendar patterns and aggregated customer-level characteristics?

**Objective:**

Develop and evaluate machine learning classification models that use historical transaction and customer information to identify potential high-demand periods and support operational planning.

## Technologies Used

- **Databricks** and **Apache Spark / PySpark**
- **Python**
- **Pandas and NumPy** – Data manipulation
- **Matplotlib and Seaborn** – Data visualisation
- **Scikit-learn** – Machine learning

## Data Sources

The following datasets were used for my transaction demand workstream. Raw datasets are not hosted in this repository.

| Dataset | Source | Purpose |
| :--- | :--- | :--- |
| **COFINFAD Customer and Transaction Data** | [Mendeley Data](https://data.mendeley.com/datasets/mhb4zn3258/1) | Customer information and historical transaction activity |
| **Colombia Public Holidays 2023** | [Colombia Calendario](https://colombiacalendario.com/en/2023) | Identifying public holidays and calendar patterns |
| **Historical TRM Exchange Rates** | [Datos Abiertos Colombia](https://www.datos.gov.co/Econom-a-y-Finanzas/Tasa-de-Cambio-Representativa-del-Mercado-Historic/mcec-87by) | Incorporating historical exchange-rate information |

## My Implementation

### 1. Data Loading and Cleaning

- Loaded customer, transaction, Colombian public holiday and historical TRM datasets into Databricks.
- Used PySpark to perform data processing and quality checks.
- Handled missing values, removed duplicates and standardised data types.
- Validated transaction records and customer identifiers.

### 2. Data Integration and Transformation

- Joined customer and transaction datasets using `customer_id`.
- Integrated public holiday and historical exchange-rate information based on transaction dates.
- Aggregated transaction records into a daily analytical dataset for demand classification.

### 3. Exploratory Data Analysis (EDA)

- Analysed daily transaction demand and transaction trends.
- Investigated differences in transaction activity across weekdays and public holidays.
- Examined transaction-type composition and active-customer activity.
- Explored historical patterns that could help explain changes in transaction demand.

### 4. Feature Engineering

Created historical features using information available before the prediction date to prevent data leakage.

Features included:

- Previous-day transaction counts and historical rolling averages.
- Transaction-type proportions, including transfers, withdrawals, payments and deposits.
- Active-customer activity and application engagement.
- Product adoption and customer composition.
- Historical TRM and calendar information.

Implemented shifted historical windows so that future demand predictions were based on past information rather than data from the prediction date.

### 5. Machine Learning

Developed and compared four classification models to predict daily transaction demand:

- Multinomial Logistic Regression
- Random Forest
- Gradient Boosting
- AdaBoost

Used a chronological 70:30 training and testing split, where earlier dates were used for training and later dates were reserved for testing.

### 6. Model Evaluation

Evaluated model performance using:

- Accuracy
- Macro Precision
- Macro Recall
- Macro F1
- High-demand Recall

The models were compared based on their ability to classify Low, Medium and High transaction-demand days.

| Model | Accuracy | Macro F1 |
| :--- | :--- | :--- |
| **Random Forest** | 45.00% | 43.02% |
| **Gradient Boosting** | — | 38.61% |
| **AdaBoost** | — | — |
| **Logistic Regression** | 34.00% | 32.45% |

**Key Findings:**

- Random Forest achieved the best overall balanced performance, with a Macro F1 score of 43.02%.
- AdaBoost achieved the highest High-demand Recall of 59.57%.
- Medium-demand classification remained challenging.
- Historical transaction and customer features provided some predictive value, although further improvements are needed.

### 7. Dashboard Preparation

Prepared four Unity Catalog tables in Databricks to support dashboard visualisation of transaction demand patterns:

- `dashboard_demand_calendar`
- `dashboard_transaction_mix`
- `dashboard_active_customers`
- `dashboard_historical_signals`

These tables support the visualisation of daily demand, transaction composition, active customers and historical signals.

## Big Data Characteristics

My implementation demonstrates several Big Data characteristics:

- **Volume:** Processed a dataset containing over 3.15 million transaction records.
- **Velocity:** Aggregated transaction activity by day to support next-day demand prediction.
- **Variety:** Combined customer, transaction, public holiday and exchange-rate datasets.
- **Veracity:** Performed data cleaning, duplicate checks, missing-value handling and data validation.

## Repository Contents

| File | Description |
| :--- | :--- |
| `244423Q_IT3388_KangBin_BDMPJ_FINAL.ipynb` | Main implementation notebook covering data preparation, feature engineering and machine learning. |
| `244423Q_KangBin_IT3388_EDA.ipynb` | Exploratory Data Analysis notebook. |
| `KangBin_244423Q_IT3388_Final_Report.docx` | Individual final project report. |
| `IT3388_KangBin_EDA_Report.docx` | Exploratory Data Analysis report. |
| `Project Proposal.docx` | Initial group project proposal detailing all 4 workstreams.|
| `IT3388 2026S1 Project Guide_V1.pdf` | Official project guide. |

## Conclusion

Through this workstream, I developed a Big Data processing and machine learning pipeline for classifying daily fintech transaction demand.

The implementation involved processing large datasets using PySpark, conducting exploratory data analysis, engineering historical features, training and evaluating multiple classification models, and preparing analytical tables for dashboard visualisation.

The results showed that Random Forest achieved the strongest overall balanced performance, while AdaBoost was better at identifying High-demand days. Although the models demonstrated some ability to identify historical demand patterns, further improvements are needed to achieve more reliable predictions for operational planning.
