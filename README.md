# Online Transaction Fraud Detection Using Behavioral and Device Intelligence

## 📌 Overview

Online payment fraud is a major challenge for financial institutions and e-commerce platforms. Fraudulent transactions are relatively rare compared to legitimate transactions, making fraud detection a highly imbalanced classification problem.

This project develops a **Machine Learning-based fraud detection system** using transaction, identity, device, card, email, and behavioral information.

The project compares multiple Machine Learning models and identifies the most suitable model for detecting fraudulent transactions based on fraud-specific evaluation metrics.

---

## 🎯 Objectives

- Analyze transaction and identity data to identify fraud-related patterns.
- Perform Exploratory Data Analysis (EDA) on transaction, device, email, card, and behavioral features.
- Handle missing values and high-cardinality categorical variables.
- Engineer meaningful behavioral and transaction-based features.
- Use a time-based train-validation split to simulate real-world fraud detection.
- Compare multiple Machine Learning algorithms.
- Evaluate models using fraud-focused metrics.
- Generate a fraud probability and risk level for individual transactions.

---

## 📂 Dataset

The project uses the **IEEE-CIS Fraud Detection dataset**.

The dataset consists of four files:

- `train_transaction.csv`
- `train_identity.csv`
- `test_transaction.csv`
- `test_identity.csv`

The transaction and identity datasets are merged using:

`TransactionID`

A **left join** is used so that all transaction records are retained while identity/device information is added where available.

---

## 🔍 Exploratory Data Analysis

The project performs EDA to understand the characteristics of fraudulent and legitimate transactions.

Key areas analyzed include:

- Fraud vs. legitimate transaction distribution
- Transaction amount
- Product category
- Transaction time
- Device information
- Email domains
- Card-related features
- M1–M9 behavioral/matching features
- Missing-value patterns
- Categorical feature cardinality
- Numerical features associated with fraud

The analysis also helps identify the challenges caused by the highly imbalanced target variable.

---

## ⚙️ Feature Engineering

Several features are created to provide additional information to the Machine Learning models.

### Time-Based Features

- `TransactionHour`
- `TransactionDay`

These features help capture temporal transaction behavior.

### Transaction Amount

A log-transformed transaction amount is created to reduce the impact of highly skewed transaction values.

- `TransactionAmt_log`

### Missingness Features

Missing information itself can contain useful fraud signals.

Examples include:

- Device information missing indicator
- Email domain missing indicator
- Identity missing count
- Other missing-value indicators

### Email-Based Features

Email-related information is used to create additional behavioral signals, including email-domain relationships.

### Frequency Features

Frequency-based features are used to capture how frequently certain entities or attributes appear in the dataset, helping identify unusual transaction behavior.

---

## 🕒 Train-Validation Strategy

Instead of randomly splitting the data, the project uses a **time-based split**.

Transactions are sorted using:

`TransactionDT`

Earlier transactions are used for training, while later transactions are used for validation.

This approach better represents a real-world fraud detection scenario where a model is trained on historical transactions and used to predict future transactions.

---

## 🧹 Data Preprocessing

The preprocessing pipeline handles numerical and categorical features separately.

### Numerical Features

- Missing values are replaced using **median imputation**.

### Categorical Features

- Missing values are replaced using **most-frequent imputation**.
- Categorical variables are converted using **One-Hot Encoding**.
- `handle_unknown='ignore'` is used to handle unseen categories during validation/prediction.

The preprocessing is implemented using Scikit-learn's pipeline and `ColumnTransformer`.

---

## 🤖 Machine Learning Models

Three Machine Learning algorithms are compared:

### 1. Logistic Regression

Used as the baseline classification model.

Class balancing is applied because fraudulent transactions are much less frequent than legitimate transactions.

### 2. Random Forest

An ensemble-based model capable of capturing nonlinear relationships between transaction features.

### 3. XGBoost

A gradient boosting algorithm used as the primary model because of its strong performance on structured/tabular data.

---

## 📊 Model Evaluation

Because fraud detection is an imbalanced classification problem, accuracy alone is not sufficient.

The models are evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC
- PR-AUC

### Model Comparison

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Logistic Regression | 0.804 | 0.105 | 0.624 | 0.179 | 0.779 | 0.123 |
| Random Forest | **0.926** | **0.251** | 0.580 | **0.351** | 0.871 | 0.448 |
| XGBoost | 0.895 | 0.210 | **0.748** | 0.328 | **0.910** | **0.510** |

### Final Model

**XGBoost** is selected as the final model for the fraud detection system.

Although Random Forest achieves higher accuracy, XGBoost performs better on the metrics that are particularly important for fraud detection:

- Highest Recall: **74.8%**
- Highest ROC-AUC: **0.910**
- Highest PR-AUC: **0.510**

The higher recall means that XGBoost is able to identify a larger proportion of fraudulent transactions.

---

## 🚨 Fraud Probability & Risk Prediction

The final XGBoost model does not only provide a binary fraud prediction.

It also generates a **fraud probability** using:

`predict_proba()`

The probability can be converted into a risk level for easier interpretation.

Example:

```text
Fraud Probability: 87.42%
Prediction: Fraud
Risk Level: Very High Risk
