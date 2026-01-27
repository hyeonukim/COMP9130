# Mini Project 2: Heart Disease Prediction

## Problem Description and Motivation

Heart disease remains one of the leading causes of death worldwide. Early detection and risk assessment can significantly improve patient outcomes by enabling timely intervention. The goal of this project is to apply machine learning techniques to predict whether a patient has heart disease based on clinical and demographic features.

## Dataset Description

**Source**
Heart Disease Dataset (Kaggle):
[https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)

**Objective**
Predict whether a patient has heart disease.

**Features**

* Total features: 14
* Examples include:

  * age
  * sex
  * chest pain type
  * resting blood pressure
  * cholesterol
  * fasting blood sugar
  * maximum heart rate achieved
  * exercise-induced angina
  * thal

**Special Feature Encoding**

* `thal`:

  * 0 = normal
  * 1 = fixed defect
  * 2 = reversible defect

**Target Variable**

* Binary classification:

  * 0 = No heart disease
  * 1 = Heart disease

**Dataset Size**

* 303 samples

**Class Balance**

* Approximately balanced:

  * ~54% with heart disease
  * ~46% without heart disease


## Project Structure
```
mini-project-2/
├── README.md
├── requirements.txt
├── data/
│   └── heart.csv
├── notebooks/
│   ├── 01_exploration.ipynb
│   └── 02_modeling.ipynb
└── src/
```

## Setup Instructions

1. Clone the repository:

   ```bash
   git clone https://github.com/hyeonukim/COMP9130
   cd mini-project-2
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Run the notebooks in order:

   * `01_exploration.ipynb`
   * `02_modeling.ipynb`

## Evaluation Metrics

The models are evaluated using multiple classification metrics to provide a comprehensive assessment:

* **Accuracy**: Overall correctness of the model
* **Precision**: Proportion of predicted positives that are correct
* **Recall**: Proportion of actual positives correctly identified
* **F1 Score**: Harmonic mean of precision and recall
* **ROC-AUC**: Ability of the model to distinguish between classes

Confusion matrices and ROC curves are also used for visualization and deeper analysis.

## Results Summary

| Model               | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---------------------|----------|-----------|--------|----------|---------|
| Logistic Regression | 0.885246 | 0.882353 | 0.909091 | 0.895522 | 0.917749 |
| Decision Tree       | 0.721311 | 0.766667 | 0.696970 | 0.730159 | 0.723485 |
| KNN                 | 0.852459 | 0.833333 | 0.909091 | 0.869565 | 0.869589 |

**Key Takeaways**:

* Logistic Regression achieved the best overall performance, with the highest ROC-AUC (0.918) and a strong balance of accuracy, precision, and recall, making it the most reliable model in this comparison.

* KNN demonstrated high recall (0.909), indicating strong ability to correctly identify patients with heart disease, though with slightly lower precision than Logistic Regression.

* Decision Tree showed noticeably lower performance across all metrics, suggesting weaker generalization and higher sensitivity to the specific train–test split compared to the other models.

* Both Logistic Regression and KNN models can be used for heart disease prediction as they both have high recall (0.909)

## Team Member Contributions

* **Eric**
  * Exploratory Data Analysis (EDA)
  * Data preprocessing
  * Feature engineering
  * Hyperparameter tuning
  * Modeling

* **Timothy**
  * Model evaluation
  * Model analysis
  * Model visualizations
  * `requirements.txt`
  * `README.md`
