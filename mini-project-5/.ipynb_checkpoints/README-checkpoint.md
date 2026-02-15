# Mini Project 5: CNN Image Classifier

## Problem Description and Motivation

Image classification is a fundamental task in computer vision with applications ranging from medical diagnosis to autonomous vehicles. For this project, we build a custom Convolutional Neural Network (CNN) to classify natural scene images into six categories: buildings, forest, glacier, mountain, sea, and street. This task demonstrates the effectiveness of deep learning approaches for extracting spatial hierarchies and visual patterns from image data.

The challenge lies in building a model that can generalize well to unseen images rather than simply memorizing the training data. Through this project, we explore the trade-offs between model accuracy and overfitting, and investigate how augmentation and regularization techniques impact model performance and generalization capability.

## Dataset Description

**Source**  
Intel Image Classification Dataset (Kaggle):  
https://www.kaggle.com/datasets/puneet6060/intel-image-classification

**Objective**  
Classify natural scene images into one of six categories.

**Classes**
- Buildings
- Forest
- Glacier
- Mountain
- Sea
- Street

**Dataset Size**
- Training: 11,228 images (80% of seg_train)
- Validation: 2,806 images (20% of seg_train)
- Test: 3,000 images

**Image Properties**
- Image size: 150×150 pixels (RGB)
- Pixel range: [0, 255] (normalized to [0, 1] in models)
- Class distribution: Well-balanced (1.13:1 imbalance ratio)

## Project Structure
```
mini-project-5/
├── README.md
├── requirements.txt
├── data/
│   ├── seg_train/
│   ├── seg_test/
│   └── seg_pred/
├── notebooks/
│   └── cnn.ipynb
└── .gitignore
```

## Setup Instructions

1. Clone the repository:
```bash
   git clone [your-repo-url]
   cd mini-project-5
```

2. Install dependencies:
```bash
   pip install -r requirements.txt
```

3. Download the dataset from Kaggle and place it in the `data/` directory

4. Run the notebook:
   - Open `notebooks/cnn.ipynb` in Jupyter Lab

## Methods Used

**Baseline CNN (No Augmentation)**
- 3 convolutional blocks (32 → 64 → 128 filters)
- No data augmentation
- No regularization (dropout or batch normalization)
- Trained for 10 epochs

**Improved CNN (With Augmentation + Regularization)**
- Same architecture as baseline
- Data augmentation:
  - RandomFlip (horizontal)
  - RandomRotation (10%)
  - RandomZoom (5%)
- Regularization techniques:
  - BatchNormalization after each conv layer
  - Dropout (0.1, 0.15, 0.2)
- Trained for 10 epochs

**Evaluation Metrics**
- Accuracy (training, validation, test)
- Precision, Recall, F1-Score (per class)
- Train-validation gap (overfitting measure)

## Results Summary

**Baseline Model Performance**
- Training Accuracy: 98.4%
- Validation Accuracy: 77.8%
- Test Accuracy: 77.2%
- Weighted F1-Score: 0.77
- Train-Val Gap: 20.7% (severe overfitting)

**Improved Model Performance**
- Training Accuracy: 69.9%
- Validation Accuracy: 64.3%
- Test Accuracy: 65.5%
- Weighted F1-Score: 0.65
- Train-Val Gap: 5.5% (good generalization)

**Key Findings**

While the improved model achieved lower overall accuracy (65.5%) compared to the baseline (77.2%), it demonstrated significantly better generalization. The baseline model's 20.7% train-validation gap indicates it was memorizing training patterns rather than learning robust features. In contrast, the improved model's 5.5% gap shows that regularization techniques (augmentation, BatchNorm, dropout) successfully prevented overfitting.

This demonstrates an important machine learning principle: high validation accuracy alone does not guarantee a good model if it comes at the cost of severe overfitting. The improved model's lower but more honest accuracy suggests it would perform more reliably on real-world data.

**Per-Class Performance Insights**

The baseline model performs particularly well on forest (F1: 0.92) and street (F1: 0.80) classes, but struggles with sea (F1: 0.76) due to visual similarity with other natural scenes. The improved model shows a different pattern, with strong forest classification (F1: 0.83) but weaker performance on glacier (F1: 0.50) and mountain (F1: 0.57), suggesting the regularization may have been too restrictive for these visually similar categories.

**Visualization Results**
- Confusion matrices show class-specific performance differences between models
- Feature visualizations reveal learned patterns in the first convolutional layer
- Misclassification analysis identifies challenging image categories (glacier vs mountain, sea vs sky)

## Team Member Contributions

* **Eric**
  * 

* **Henry**
  * 