# 💸 UPI Fraud Detection Using Machine Learning & GenAI

> An end-to-end fraud detection system for UPI transactions that combines behavioral feature engineering, Machine Learning, class-imbalance handling, hyperparameter tuning, Generative AI explanations, and an interactive Gradio screening application.

---

## 📌 Project Overview

UPI fraud detection requires more than simply looking at transaction amounts. Suspicious behavior can appear through transaction velocity, first-time receivers, unusual transaction hours, device changes, location changes, and transaction amounts that differ significantly from a user's normal behavior.

This project develops an end-to-end **UPI transaction fraud detection pipeline** that converts transaction-level data into behavioral risk signals and uses Machine Learning to estimate fraud probability.

The project also includes a **Generative AI explanation layer using Groq** that converts model signals into plain-English explanations and an interactive **Gradio fraud screening application** for transaction-level risk assessment.

---

## 🎯 Objectives

- Build an end-to-end UPI fraud detection pipeline
- Clean and validate transaction data
- Perform exploratory data analysis
- Engineer behavioral fraud detection features
- Handle class imbalance using class weighting and SMOTE
- Compare multiple Machine Learning models
- Optimize the Random Forest model using hyperparameter tuning
- Generate fraud probability scores
- Explain flagged transactions using Generative AI
- Deploy an interactive fraud screening interface using Gradio

---

## 📂 Dataset Information

| Attribute | Details |
|-----------|---------|
| **Domain** | UPI / FinTech |
| **Dataset Type** | Synthetic UPI Transactions |
| **Transactions** | 10,000 |
| **Target Variable** | `Is_Fraud` |
| **Problem Type** | Binary Classification |
| **Fraud Patterns** | 5 Programmatically Injected Patterns |

The dataset is synthetically generated because real UPI transaction logs with verified fraud labels are not publicly available due to privacy and security considerations.

The project simulates realistic fraud behavior through five injected patterns:

- Velocity bursts
- Odd-hour high-value transfers
- Device/location mismatches
- Mule-account fan-in
- Test-then-drain scams

---

## 🔄 Project Workflow

```text
UPI Transaction Data
        │
        ▼
Data Cleaning & Validation
        │
        ▼
Exploratory Data Analysis
        │
        ▼
Behavioral Feature Engineering
        │
        ▼
Feature Selection
        │
        ▼
Chronological Train/Test Split
        │
        ▼
Scaling + SMOTE / Class Weighting
        │
        ▼
Machine Learning Models
        │
        ▼
Model Evaluation
        │
        ▼
Random Forest Hyperparameter Tuning
        │
        ▼
Fraud Probability
        │
        ▼
Groq GenAI Explanation
        │
        ▼
Gradio Fraud Screening Demo
```

---

## 🧹 Data Cleaning

The transaction data was checked and cleaned before analysis and modeling.

The preprocessing workflow includes:

- Duplicate transaction detection
- Duplicate row removal
- Missing value analysis
- Critical identifier validation
- Invalid transaction amount detection
- Removal of non-positive transaction amounts
- Self-transfer detection
- UPI ID normalization
- Text field standardization
- Transaction amount outlier analysis

Outliers were analyzed rather than automatically removed because unusually large transactions can contain important fraud signals.

---

## 📊 Exploratory Data Analysis

EDA was performed to understand the fraud distribution and identify behavioral patterns.

The analysis includes:

- Fraud vs. legitimate transaction distribution
- Transaction amount distribution
- Fraud sub-type analysis
- Fraud rate by hour
- Fraud rate by transaction type
- Fraud rate by transaction status
- Outlier analysis

Because fraud is an imbalanced classification problem, **F1-score, precision, and recall** are emphasized instead of relying only on accuracy.

---

## ⚙️ Behavioral Feature Engineering

The project focuses on behavioral signals rather than using raw identifiers as predictive features.

### Key Features

#### ⏱️ Transaction Velocity

Measures how many transactions a sender performs within the previous hour.

```text
Sender_Velocity_1hr
```

High transaction velocity can indicate suspicious activity or automated transaction bursts.

---

#### 👤 New Receiver Detection

Identifies whether the sender is making a transaction to a receiver for the first time.

```text
Is_New_Receiver
```

---

#### 📱 Device Change

Checks whether the transaction originates from a device different from the sender's previous device.

```text
Device_Changed
```

---

#### 📍 Location Change

Identifies changes from the sender's previous transaction location.

```text
Location_Changed
```

---

#### 💰 Transaction Amount Z-Score

Measures how unusual the current transaction amount is compared with the sender's previous transaction behavior.

```text
Amount_Zscore
```

---

#### 🌙 Night-Time Transaction

Identifies transactions occurring during late-night hours.

```text
Is_Night
```

---

## 🔎 Feature Selection

Feature selection was performed using multiple approaches:

- Correlation Analysis
- Mutual Information
- Random Forest Feature Importance

The initial candidate features were evaluated for relevance and redundancy.

Two low-signal features were removed:

```text
Day_of_Week
Status_Failed
```

The final feature set focuses on behavioral signals such as:

- Transaction amount
- Sender account age
- Transaction hour
- Night-time indicator
- New receiver
- Sender transaction velocity
- Device changes
- Location changes
- Amount z-score
- Transaction type

---

## ⚖️ Handling Class Imbalance

Fraud datasets are naturally imbalanced because legitimate transactions greatly outnumber fraudulent transactions.

Two approaches were used:

### Class Weighting

Models were trained using class-balanced weights so that fraudulent transactions receive greater importance during training.

### SMOTE

**Synthetic Minority Over-sampling Technique (SMOTE)** was applied only to the training data to increase representation of the minority fraud class.

The test set remains untouched to provide an unbiased evaluation.

---

## 🤖 Machine Learning Models

Multiple classification approaches were evaluated.

### Logistic Regression

Provides a strong and interpretable baseline for binary fraud classification.

### Random Forest

Captures nonlinear relationships between behavioral fraud indicators and provides feature importance information.

### XGBoost

A gradient boosting approach capable of modeling complex relationships between transaction features.

---

## 📊 Model Evaluation

Because the dataset is imbalanced, **F1-score is used as the primary model selection metric**.

Additional metrics include:

- Precision
- Recall
- F1-Score
- ROC-AUC
- PR-AUC
- Confusion Matrix
- Classification Report

The project evaluates six model configurations combining:

- Class weighting
- SMOTE
- Logistic Regression
- Random Forest
- XGBoost

---

## 🔧 Hyperparameter Tuning

The Random Forest model was further optimized using:

```text
RandomizedSearchCV
```

The search evaluates parameters including:

- Number of estimators
- Maximum tree depth
- Minimum samples split
- Minimum samples leaf
- Maximum features

The optimization target is **F1-score**, reflecting the importance of balancing fraud detection recall with false-positive control.

The tuned model replaces the baseline only when it improves F1-score on the held-out test set.

---

## 🧠 Generative AI Fraud Explainer

A Generative AI layer was added to make fraud predictions easier to understand.

The project uses:

```text
Groq
GPT-OSS 120B
```

The model receives transaction-level risk signals and generates a short plain-English explanation of why the transaction may be suspicious.

Example signals include:

- High transaction velocity
- First-time receiver
- Device change
- Location change
- Night-time transaction
- Unusually large transaction amount

### Offline Fallback

If the Groq API is unavailable, the system uses a rule-based explanation engine instead.

This allows the fraud screening workflow to remain functional without an API connection.

---

## 🚀 Gradio Fraud Screening Demo

The project includes an interactive Gradio interface where users can enter transaction characteristics and receive:

- Fraud probability
- Risk category
- Plain-English explanation

### Risk Categories

```text
🟢 LOW RISK
🟠 MEDIUM RISK
🔴 HIGH RISK
```

The demo considers inputs such as:

- Transaction amount
- Sender account age
- Hour of transaction
- First-time receiver
- Sender transaction velocity
- Device change
- Location change
- Merchant payment indicator

---

## 🛠️ Technologies Used

### Programming

- Python

### Data Analysis

- Pandas
- NumPy

### Visualization

- Matplotlib
- Seaborn

### Machine Learning

- Scikit-learn
- Logistic Regression
- Random Forest
- XGBoost

### Imbalanced Learning

- SMOTE
- Class Weighting

### Generative AI

- Groq API
- GPT-OSS 120B

### Deployment

- Gradio

### Development

- Jupyter Notebook

---

## 🚀 Key Features

- End-to-End Fraud Detection Pipeline
- Synthetic UPI Transaction Dataset
- Behavioral Fraud Feature Engineering
- Leakage-Aware Feature Creation
- Exploratory Data Analysis
- Mutual Information Analysis
- Random Forest Feature Importance
- Class Weighting
- SMOTE
- Logistic Regression
- Random Forest
- XGBoost
- Randomized Hyperparameter Search
- F1-Based Model Selection
- Fraud Probability Scoring
- GenAI Fraud Explanation
- Rule-Based Offline Fallback
- Interactive Gradio Screening Tool

---

## 💼 Business Applications

This project can support:

- UPI transaction monitoring
- Digital payment fraud screening
- FinTech risk analytics
- Transaction risk scoring
- Fraud investigation support
- Analyst decision support
- Customer transaction monitoring

---

## 🌟 Future Scope

Potential extensions include:

- Graph-based transaction network analysis
- Mule-account detection using transaction networks
- Real-time fraud scoring pipelines
- Streaming transaction monitoring
- Analyst feedback loops
- Continuous model retraining
- Model monitoring and drift detection
- Production API deployment
- Integration with payment risk systems

---

## ⚠️ Important Note

This project uses **synthetically generated UPI transaction data** with programmatically injected fraud patterns. It is intended for educational and portfolio purposes and should not be treated as a production fraud detection system.

Model performance on synthetic data does not represent performance on real-world UPI transactions.

---

## ⭐ If you found this project useful, consider giving it a Star!
