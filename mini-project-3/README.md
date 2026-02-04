# Mini Project 3: Credit Card Customer Segmentation

## Problem Description and Motivation

Credit card companies serve a large and diverse customer base with wide variation in spending behavior, credit utilization, and financial risk. A key business challenge is understanding these differences in order to support targeted marketing strategies, improve customer retention, and enhance risk management. Without clear segmentation, customers are often treated as a homogeneous group, which can lead to inefficient decision-making and missed opportunities to identify high-value or high-risk customers.

Unsupervised learning is appropriate for this problem because there is no predefined label that categorizes customers into specific behavior-based groups. Instead, the goal is to discover hidden patterns and natural groupings within the data. Techniques such as clustering and dimensionality reduction allow the identification of meaningful customer segments based on financial behavior, while anomaly detection helps uncover unusual customers, such as potential VIPs or customers exhibiting risky spending patterns. This exploratory approach enables data-driven insights without requiring prior assumptions about customer categories.

## Dataset Description

**Source**
Credit Card Dataset (Kaggle):
https://www.kaggle.com/datasets/arjunbhasin2013/ccdata

**Objective**
Predict whether a patient has heart disease.

**Features**

* Total features: 17 (after preprocessing)
* Key features include:

    * BALANCE: Average balance remaining in the customer’s account.

    * PURCHASES: Total amount of purchases made.

    * ONEOFF_PURCHASES: Total value of one-time purchases.

    * INSTALLMENTS_PURCHASES: Total value of purchases made in installments.

    * CASH_ADVANCE: Total amount of cash advances taken.

    * PURCHASES_FREQUENCY: Frequency of purchase transactions.

    * CASH_ADVANCE_FREQUENCY: Frequency of cash advance transactions.

    * CREDIT_LIMIT: Credit limit assigned to the customer.

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

**Dimensionality Reduction**

* Principal Component Analysis (PCA)

    * Used to assess explained variance and linear structure

* t-SNE

    * Used to visualize non-linear relationships and cluster separation

* Anomaly Detection

    * Isolation Forest

    * Tested multiple contamination values (3%, 5%, 10%)

    * Final contamination selected based on interpretability and business realism

## Results Summary

**Optimal Number of Clusters**

We selected K = 3 based on both quantitative metrics and business interpretability. Although the elbow plot shows a gradual bend around K ≈ 5, the silhouette score peaks at K = 3 and declines for larger values, indicating weaker cluster separation. From a business perspective, three clusters provide clear, actionable customer segments, while higher K values add complexity without meaningful additional insight.

**Cluster Descriptions**
- Cluster 0 – High Usage Customers
Customers in this cluster exhibit high balances, large purchase volumes, frequent cash advances, and higher credit limits. This group contains the most behaviorally extreme users and includes both high-value customers and higher-risk profiles.

- Cluster 1 – Low Activity / Stable Customers
This cluster consists of customers with low balances, infrequent purchases, minimal cash advances, and lower credit limits. Their behavior is consistent and predictable, indicating low risk and limited engagement.

- Cluster 2 – Active Transactors
Customers in this segment show moderate balances and purchases with relatively frequent card usage, but without the extreme spending or cash advance behavior seen in Cluster 0. This group represents engaged, regular card users

**Anomalies: Number and Types**

Using Isolation Forest with a 5% contamination rate, approximately 448 anomalies were detected.

These anomalies fall into three main types:

- VIP customers with exceptionally high purchases, balances, and credit limits

- Cash-advance-heavy users, indicating potential financial risk

- Irregular or one-time high spenders with unusual but not necessarily risky patterns

Anomalies are concentrated primarily in Cluster 0, with a smaller number in Cluster 2 and almost none in Cluster 1.

**Key Business Insights**

This analysis shows that customer behavior is best understood through a layered approach. Clustering provides broad, interpretable customer segments, while anomaly detection identifies high-impact individuals within those segments who require specialized attention. Notably, Cluster 0 contains both the most valuable customers and the most potentially risky users, highlighting the need for differentiated strategies within the same segment.

From a business perspective, these findings support targeted marketing for VIP customers, risk monitoring for cash-advance-heavy users, and cost-efficient management of stable, low-activity customers. Integrating clustering, dimensionality reduction, and anomaly detection enables more precise and informed decision-making than relying on any single technique alone.

## Team Member Contributions

* **Eric**
  * Clustering Analysis
  * Dimensionality Reduction
  * Model Visualizations
  * Technical Report

* **Jacky**
  * Anomaly Dectection
  * Integrated Analysis
  * Model Visualizations
  * `README.md`
