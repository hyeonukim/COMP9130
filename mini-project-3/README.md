# Mini Project 3: Credit Card Customer Segmentation

## Problem Description and Motivation

Credit card companies manage large and diverse customer bases with varying spending behaviors, credit usage patterns, and risk profiles. Understanding these differences is essential for targeted marketing, customer retention, and risk management.

The goal of this project is to apply unsupervised machine learning techniques to segment credit card customers based on their financial behavior. By combining clustering, dimensionality reduction, and anomaly detection, the project aims to identify meaningful customer segments and uncover unusual or high-impact customers such as VIPs or potential risk cases.

## Dataset Description

**Source**
Credit Card Dataset (Kaggle):
https://www.kaggle.com/datasets/arjunbhasin2013/ccdata

**Objective**
Predict whether a patient has heart disease.

**Features**

* Total features: 17 (after preprocessing)
* Key features include:

  * BALANCE
  * PURCHASES
  * ONEOFF_PURCHASES
  * INSTALLMENTS_PURCHASES
  * CASH_ADVANCE
  * PURCHASES_FREQUENCY
  * CASH_ADVANCE_FREQUENCY
  * CREDIT_LIMIT

**Target Variable**

* None (unsupervised learning)

**Dataset Size**

* 8,950 customers

**Preprocessing Steps**

* Handled missing values
* Standardized features using StandardScaler
* Selected relevant spending and usage variables for modeling

## Project Structure
```
mini-project-2/
├── README.md
├── requirements.txt
├── data/
│   └── CC GENERAL.csv
├── notebooks/
│   ├── analysis.ipynb
|── .gitignore
└── src/
```

## Setup Instructions

1. Clone the repository:

   ```bash
   git clone https://github.com/hyeonukim/COMP9130
   cd mini-project-3
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Run the notebook:

   * `analysis.ipynb`

## Methods Used 

**Clustering**

* K-means clustering
* Evaluated K values from 2 to 10
* Selected final K using:
    * Elbow Method
    * Silhouette Scores
    * Business interpretability
Dimensionality Reduction

Principal Component Analysis (PCA)

Used to assess explained variance and linear structure

t-SNE

Used to visualize non-linear relationships and cluster separation

**Key Takeaways**:


## Team Member Contributions

* **Eric**
  * Exploratory Data Analysis (EDA)
  * Data preprocessing
  * Feature engineering
  * Hyperparameter tuning
  * Modeling

* **Jacky**
  * Model evaluation
  * Model analysis
  * Model visualizations
  * `requirements.txt`
  * `README.md`
