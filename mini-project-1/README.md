## Problem Description and Motivation

The goal of this study is to predict median house values using scikit-learn’s California Housing dataset and to compare the performance of different models under various feature engineering approaches.

## Dataset Description (source, features, target)

- Source: [https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_california_housing.html)
- Features: 'MedInc', 'HouseAge', 'AveRooms', 'AveBedrms', 'Population', 'AveOccup', 'Latitude', 'Longitude'
- Target: 'MedHouseVal'

## Setup Instructions (how to run the code)

Python version: 3.10.4

All the codes are in 01_exploration.ipynb, and 02_modeling.ipynb. Each file can be ran with Jupyter notebook or Google Colab using the Python Version 3.10.4

## Results summary with key metrics

- Best model: Log + Ridge(alpha = 0.1, 1, 10 all tied)
    - $R^2$ = 0.619, RMSE = 0.716, MAE = 0.534
    - The log1p feature engineering helped compared to using Ridge/Linear on the original scale
- Lowest MAE model: Polynomial + Linear:
    - $R^2$ = 0.615, RMSE = 0.720, MAE = 0.469
- Baseline Linear Regression and Ridge:
    - Both around $R^2$ = 0.600 to 0.601, RMSE = 0.733 to 0.734, MAE = 0.536
- Effect of alpha:
    - The effect of alpha were minimal, seemed to be worse when alpha was 100

## Team member contributions

Solo work
