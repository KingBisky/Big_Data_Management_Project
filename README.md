# Platform Transaction Demand Classification

### IT3388 – Big Data Management Project

## 1. Project Overview

This project was developed as part of the **IT3388 Big Data Management Project**, where our team worked together to brainstorm a business problem and propose a Big Data solution for an anonymised Colombian fintech company.

After discussing possible business challenges and analytical opportunities, we proposed the **Customer Intelligence Platform for Fintech**.

The idea was to turn the company's customer and transaction data into useful insights that could support better business decisions instead of relying only on historical reports.

### What Our Team Proposed

Our proposed platform brings together four analytical areas:

| Area | Business Objective |
|---|---|
| Customer Inactivity and Retention | Identify customers who may stop using the platform. |
| Future Customer Value | Predict the transaction value customers may generate in the future. |
| Satisfaction and Experience | Analyse customer satisfaction and identify possible service-related issues. |
| Platform Transaction Demand | Predict upcoming transaction demand to support operational planning. |

Together, these areas cover customer retention, business value, customer experience and operational readiness.

### My Individual Workstream

I was responsible for **Platform Transaction Demand Classification**.

My task was to develop a machine learning solution that predicts daily platform transaction demand and classifies upcoming days into three categories:

- **Low** – Lower transaction activity.
- **Medium** – Moderate transaction activity.
- **High** – Higher transaction activity.

The goal is to help the fintech company anticipate busy periods and prepare its system capacity and operational resources.

## 2. Business Problem

Historical transaction records show what happened in the past, but they do not directly indicate how busy the platform will be on upcoming days.

Without demand predictions, the company may struggle to prepare for periods of increased transaction activity.

**My objective:** Use historical transaction and customer information to develop models that classify upcoming daily demand levels.

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| **Databricks** | Cloud platform used to store, process and analyse project data. |
| **Apache Spark / PySpark** | Processed and transformed millions of transaction records. |
| **Python** | Main programming language. |
| **Pandas and NumPy** | Data manipulation and numerical calculations. |
| **Matplotlib and Seaborn** | Data visualisation and exploratory analysis. |
| **Scikit-learn** | Developed and evaluated machine learning models. |

**Why Databricks and PySpark?**

The main dataset contains over 3.15 million transaction records. Databricks provides a cloud environment for working with large datasets, while PySpark allows me to clean, combine and aggregate the data efficiently instead of relying on manual spreadsheet processing.

## 4. Data Sources

| Dataset | Source | Purpose |
|---|---|---|
| COFINFAD Customer and Transaction Data | [Mendeley Data](https://data.mendeley.com/datasets/mhb4zn3258/1) | Customer profiles and historical transaction records. |
| Colombia Public Holidays 2023 | [Colombia Calendario](https://colombiacalendario.com/en/2023) | Identify holidays and calendar patterns. |
| Historical TRM Exchange Rates | [Datos Abiertos Colombia](https://www.datos.gov.co/Econom-a-y-Finanzas/Tasa-de-Cambio-Representativa-del-Mercado-Historic/mcec-87by) | Provide historical exchange-rate information. |

The main COFINFAD dataset contains approximately **48,723 customer profiles and 3,159,157 transaction records**, covering 2023.

Raw datasets are not included in this repository.

## 5. My Implementation

### 5.1 Data Preparation
- Loaded datasets into Databricks.
- Used PySpark to handle missing values, remove duplicates and standardise data types.
- Validated transaction records and customer identifiers.

### 5.2 Data Integration
- Joined customer and transaction records using `customer_id`.
- Integrated public holiday and exchange-rate information.
- Aggregated individual transactions into daily records to represent overall platform activity.

### 5.3 Exploratory Data Analysis (EDA)
- Examined daily transaction trends.
- Compared activity across weekdays and public holidays.
- Analysed transaction types and active-customer activity.
- Identified historical patterns that could help explain changes in demand.

### 5.4 Feature Engineering

Created historical features to help models identify demand patterns, including:

- Previous-day transaction counts and rolling averages.
- Transaction-type proportions.
- Active-customer activity and application engagement.
- Product adoption and customer composition.
- Calendar and historical exchange-rate information.

Used shifted historical windows to ensure predictions relied on past information and reduce data leakage.

### 5.5 Machine Learning

Developed and compared four classification models:

- Multinomial Logistic Regression
- Random Forest
- Gradient Boosting
- AdaBoost

Used a chronological **70:30 training and testing split**, training on earlier dates and testing on later dates to better reflect real-world future predictions.

## 6. Model Evaluation

Models were evaluated using Accuracy, Macro Precision, Macro Recall, Macro F1 and High-demand Recall.

| Model | Accuracy | Macro Precision | Macro Recall | Macro F1 | High-demand Recall |
|---|---:|---:|---:|---:|---:|
| Random Forest | 45.00% | 43.84% | 42.83% | 43.02% | 53.19% |
| Gradient Boosting | 40.00% | 39.36% | 38.66% | 38.61% | 44.68% |
| AdaBoost | 43.00% | 38.27% | 38.29% | 37.54% | 59.57% |
| Multinomial Logistic Regression | 34.00% | 32.92% | 33.24% | 32.45% | 38.30% |

### Key Findings

- **Random Forest** achieved the best overall performance, with the highest Accuracy and Macro F1.
- **AdaBoost** achieved the highest High-demand Recall at 59.57%, making it the strongest model for identifying actual high-demand days.
- **Medium-demand classification** remained challenging across the models.

Overall, Random Forest provided the strongest balanced performance, while AdaBoost was better at detecting high-demand periods. Further improvements are needed before using these predictions for operational decisions.

## 7. Dashboard Preparation

Prepared four analytical tables in Databricks Unity Catalog for dashboard visualisation:

- `dashboard_demand_calendar`
- `dashboard_transaction_mix`
- `dashboard_active_customers`
- `dashboard_historical_signals`

These tables support the visualisation of daily demand levels, transaction composition, active customers and historical demand patterns.

**Note:** Viewing the live Databricks dashboard requires a Databricks account and access to the workspace. For readers who do not have access, screenshots of the completed dashboard are provided in my **Individual Final Project Report**.

## 8. Big Data Characteristics

| Characteristic | Application in My Project |
|---|---|
| **Volume** | Processed over 3.15 million transaction records. |
| **Velocity** | Aggregated transaction activity daily to support upcoming demand predictions. |
| **Variety** | Combined customer, transaction, holiday and exchange-rate data. |
| **Veracity** | Performed data cleaning, duplicate removal and validation checks. |

## 9. Limitations and Future Improvements

- Approximately one year of historical data limits long-term trend analysis.
- Daily aggregation results in fewer modelling observations than the original transaction dataset.
- Medium-demand classification needs improvement.
- Further model tuning and testing on future data could improve reliability.
- Additional runtime and resource benchmarking is needed to evaluate scalability.

## 10. Repository Contents

| File | Description |
|---|---|
| `Project Proposal.docx` | Group proposal documenting the initial brainstorming outcome, business problem, project objectives and proposed Customer Intelligence Platform. |
| `244423Q_IT3388_KangBin_BDMPJ_FINAL.ipynb` | Main implementation notebook covering data preparation, feature engineering, model development and evaluation. |
| `244423Q_KangBin_IT3388_EDA.ipynb` | Notebook containing exploratory data analysis and visualisations. |
| `KangBin_244423Q_IT3388_Final_Report.docx` | Individual final report documenting my methodology, implementation, results and recommendations. |
| `IT3388_KangBin_EDA_Report.docx` | EDA report describing the datasets and initial findings. |
| `IT3388 2026S1 Project Guide_V1.pdf` | Official IT3388 project requirements and guidelines. |

## 11. Conclusion

This project began with a team effort to identify a real business problem and propose a unified Customer Intelligence Platform for a fintech company.

My individual contribution focused on developing the transaction-demand classification workstream. Using Databricks, PySpark and machine learning, I built a workflow covering data preparation, exploratory analysis, feature engineering, model evaluation and dashboard preparation.

Random Forest achieved the best overall balanced performance, while AdaBoost was more effective at identifying high-demand days.

The work demonstrates how historical fintech transaction data can be transformed into predictive insights to help anticipate platform activity and support operational planning.
