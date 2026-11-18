# Platform Transaction Demand Classification

### IT3388 – Big Data Management Project

## Project Overview

This project is part of the **Customer Intelligence Platform for Fintech**, a cloud-based Big Data project proposed for an anonymised Colombian fintech company.

The overall platform aims to transform customer profiles and transaction records into predictive insights that support customer retention, future customer-value estimation, satisfaction analysis and operational planning.

My assigned workstream focuses on **Platform Transaction Demand Classification**. My responsibility was to develop a predictive solution that classifies upcoming platform transaction demand into three categories: **Low, Medium and High**.

The purpose is to help the fintech company anticipate potential high-demand periods and prepare its infrastructure capacity, staffing and operational resources accordingly.

### Business Problem

Historical transaction records describe past platform activity but do not provide a forward-looking prediction of upcoming transaction demand. Without demand predictions, the fintech company may face difficulties planning infrastructure capacity, staffing and operational resources during peak transaction periods.

### Business Question

Can upcoming platform transaction demand be classified as Low, Medium or High using historical transaction activity, transaction-type composition, calendar patterns and aggregated customer-level characteristics?

### Objective

Develop and evaluate machine learning classification models that use historical transaction and customer information to identify potential high-demand periods and support operational planning.

## Overall Project Context

The proposed Customer Intelligence Platform consists of four complementary analytical workstreams:

| Workstream | Business Purpose |
|---|---|
| Customer Inactivity and Retention | Identify customers at risk of becoming inactive. |
| Future Customer Value | Estimate customers' future transaction value. |
| Satisfaction and Experience | Analyse customer satisfaction and service-related factors. |
| Platform Transaction Demand | Predict upcoming transaction-demand levels for operational planning. |

My workstream focuses on daily platform activity rather than individual customer predictions. It complements the other workstreams by providing operational insights into when transaction demand may increase.

## Technologies Used

- **Databricks** and **Apache Spark / PySpark**
- **Python**
- **Pandas and NumPy** – Data manipulation
- **Matplotlib and Seaborn** – Data visualisation
- **Scikit-learn** – Machine learning

## Data Sources

The following datasets were used for my transaction demand workstream. Raw datasets are not hosted in this repository.

| Dataset | Source | Purpose |
|---|---|---|
| **COFINFAD Customer and Transaction Data** | [Mendeley Data](https://data.mendeley.com/datasets/mhb4zn3258/1) | Customer information and historical transaction activity. |
| **Colombia Public Holidays 2023** | [Colombia Calendario](https://colombiacalendario.com/en/2023) | Identifying public holidays and calendar patterns. |
| **Historical TRM Exchange Rates** | [Datos Abiertos Colombia](https://www.datos.gov.co/Econom-a-y-Finanzas/Tasa-de-Cambio-Representativa-del-Mercado-Historic/mcec-87by) | Incorporating historical exchange-rate information. |

The main COFINFAD dataset contains approximately **48,723 customer profiles and 3,159,157 transaction records**, covering activity during 2023.

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

Created historical features using information available before the prediction date to reduce the risk of data leakage.

Features included:

- Previous-day transaction counts and historical rolling averages.
- Transaction-type proportions, including transfers, withdrawals, payments and deposits.
- Active-customer activity and application engagement.
- Product adoption and customer composition.
- Historical TRM and calendar information.

Implemented shifted historical windows so that demand predictions were based on past information rather than data from the prediction date.

### 5. Machine Learning

Developed and compared four classification models to predict daily transaction demand:

- Multinomial Logistic Regression
- Random Forest
- Gradient Boosting
- AdaBoost

Used a chronological 70:30 training and testing split, where earlier dates were used for training and later dates were reserved for testing.

This approach reflects the time-based nature of the prediction task and avoids randomly mixing earlier and later dates between training and testing.

### 6. Model Evaluation

Evaluated model performance using:

- Accuracy
- Macro Precision
- Macro Recall
- Macro F1
- High-demand Recall

The models were compared based on their ability to classify Low, Medium and High transaction-demand days.

| **Model** | **Accuracy** | **Macro Precision** | **Macro Recall** | **Macro F1** | **High-demand Recall** |
|---|---:|---:|---:|---:|---:|
| **Random Forest** | 45.00% | 43.84% | 42.83% | 43.02% | 53.19% |
| **Gradient Boosting** | 40.00% | 39.36% | 38.66% | 38.61% | 44.68% |
| **AdaBoost** | 43.00% | 38.27% | 38.29% | 37.54% | 59.57% |
| **Multinomial Logistic Regression** | 34.00% | 32.92% | 33.24% | 32.45% | 38.30% |

**Key Findings:**

- Random Forest achieved the best overall performance, with the highest Accuracy (45.00%) and Macro F1 (43.02%).
- AdaBoost achieved the highest High-demand Recall (59.57%), making it the strongest model for identifying high-demand days.
- Gradient Boosting achieved a Macro F1 of 38.61%, outperforming AdaBoost and Logistic Regression on this metric.
- Multinomial Logistic Regression recorded the lowest overall performance.
- Medium-demand classification remained challenging across the models.

These results suggest that historical activity can help identify potential demand patterns, but the models still have room for improvement before being used for operational decisions.

Overall, Random Forest provided the strongest balanced classification performance, while AdaBoost was more effective at detecting High-demand days.

### 7. Dashboard Preparation

Prepared four Unity Catalog tables in Databricks to support dashboard visualisation of transaction demand patterns:

- `dashboard_demand_calendar`
- `dashboard_transaction_mix`
- `dashboard_active_customers`
- `dashboard_historical_signals`

These tables support the visualisation of:

- Daily transaction-demand levels.
- Transaction-type composition.
- Active-customer activity.
- Historical demand signals.

The intended operational use is to help users monitor demand patterns and identify potential high-demand periods.

## Big Data Architecture

The overall project proposal follows a cloud-based Big Data processing approach:

1. Upload raw customer and transaction datasets to cloud storage.
2. Use Databricks and Apache Spark to clean, integrate and process large datasets.
3. Create analytical datasets and historical features for machine learning.
4. Train and evaluate models using chronological validation.
5. Store analytical outputs in query-ready tables.
6. Connect the results to dashboards for business interpretation and decision-making.

My workstream contributes to this architecture through PySpark-based data processing, daily demand aggregation, machine learning classification and dashboard-ready analytical tables.

## Big Data Characteristics

My implementation demonstrates several Big Data characteristics:

- **Volume:** Processed a dataset containing over 3.15 million transaction records.
- **Velocity:** Aggregated transaction activity by day to support upcoming demand prediction.
- **Variety:** Combined customer, transaction, public holiday and exchange-rate datasets.
- **Veracity:** Performed data cleaning, duplicate checks, missing-value handling and data validation.

## Limitations and Future Improvements

- The dataset covers approximately one year of historical activity, limiting the analysis of long-term seasonal patterns.
- Daily demand classification provides a relatively small number of observations compared with transaction-level modelling.
- Medium-demand classification remains challenging and could benefit from further feature engineering and model tuning.
- Additional validation on future transaction periods would help determine whether the models generalise beyond the available dataset.
- Further runtime and resource benchmarking would be needed to evaluate processing scalability under larger data volumes.

## Repository Contents

| File | Description |
|---|---|
| `244423Q_IT3388_KangBin_BDMPJ_FINAL.ipynb` | Main implementation notebook covering data preparation, feature engineering and machine learning. |
| `244423Q_KangBin_IT3388_EDA.ipynb` | Exploratory Data Analysis notebook. |
| `KangBin_244423Q_IT3388_Final_Report.docx` | Individual final project report. |
| `IT3388_KangBin_EDA_Report.docx` | Exploratory Data Analysis report. |
| `Project Proposal.docx` | Initial group project proposal describing the Customer Intelligence Platform and its four workstreams. |
| `IT3388 2026S1 Project Guide_V1.pdf` | Official project guide. |

## Conclusion

Through this workstream, I developed a Big Data processing and machine learning pipeline for classifying daily fintech transaction demand.

The implementation involved processing large datasets using PySpark, conducting exploratory data analysis, engineering historical features, training and evaluating multiple classification models, and preparing analytical tables for dashboard visualisation.

The results showed that Random Forest achieved the strongest overall balanced performance, while AdaBoost was better at identifying High-demand days.

Overall, this workstream contributes to the proposed Customer Intelligence Platform by providing predictive operational insights that can help the fintech company prepare for potential changes in transaction demand.
