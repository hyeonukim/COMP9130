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
- Trained for 25 epochs

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
- Training Accuracy: 80.9%
- Validation Accuracy: 73.4%
- Test Accuracy: 73.3%
- Weighted F1-Score: 0.74
- Train-Val Gap: 7.4% (moderate overfitting)

**Key Findings**

The improved model achieved 73.3% test accuracy compared to the baseline's 77.2%, a difference of only 4%. However, the improved model's 7.4% train-validation gap demonstrates significantly better generalization compared to the baseline's severe 20.7% gap. This shows that regularization techniques (augmentation, BatchNorm, dropout) successfully reduced overfitting while maintaining competitive accuracy.

This demonstrates an important machine learning principle: high validation accuracy alone does not guarantee a good model if it comes at the cost of severe overfitting. The improved model required 25 epochs to converge (compared to baseline's 10 epochs), but achieved more reliable performance that would generalize better to real-world data.

**Per-Class Performance Insights**

Based on the confusion matrix analysis:
- The baseline model performs particularly well on forest (86.1% per-class accuracy) and sea (87.3%), but struggles with glacier (66.0%) and mountain (64.2%)
- The improved model shows stronger performance on buildings (90.8%) and forest (93.9%), but weaker performance on glacier (52.6%) and street (64.1%)
- Both models struggle most with visually similar classes (glacier vs mountain), indicating this is an inherently difficult distinction in the dataset
- The improved model's misclassification rate (26.7%) is only slightly higher than baseline's (22.8%), despite having much better generalization

**Visualization Results**
- Confusion matrices show class-specific performance differences between models
- Feature visualizations reveal learned patterns in the first convolutional layer
- Misclassification analysis identifies that both models struggle most with glacier (34-35% misclassification rate) and mountain (35-36% misclassification rate)
- Training curves show the improved model required more epochs due to the added complexity of augmentation and regularization

## Team Member Contributions

* **Eric**
  * [To be filled in]

* **Henry**
  * [To be filled in]