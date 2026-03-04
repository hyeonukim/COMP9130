# Mini Project 7: Blood Cell Detection with YOLOv26

## Problem Description and Motivation

Object detection in medical imaging is a high-impact application of computer vision with direct relevance to clinical workflows. For this project, we train a YOLOv26 model to detect and classify three types of blood cells — red blood cells (RBC), white blood cells (WBC), and platelets — in blood smear microscopy images. This task mirrors the automated complete blood count (CBC) analysis problem faced by pathology labs that process thousands of samples daily.

Unlike image classification, object detection requires both identifying what cell type is present and localizing where each instance appears in the image. This makes it directly applicable to CBC automation, where individual cell counts are clinically meaningful. We evaluate the model's performance and assess its viability for clinical deployment as a decision-support tool.

## Dataset Description

**Source**  
BCCD (Blood Cell Count and Detection) Dataset — Roboflow Universe:  
https://universe.roboflow.com/roboflow-100/bccd-fy4dl

**Objective**  
Detect and classify blood cells in microscopy images into three categories.

**Classes**
- RBC (Red Blood Cells)
- WBC (White Blood Cells)
- Platelets

**Dataset Size**
- Train: 377 images (6,192 total annotations)
- Validation: 110 images
- Test: 53 images

**Class Distribution (Training Set)**
- RBC: 5,590 annotations
- WBC: 424 annotations
- Platelets: 178 annotations
- Note: Severe class imbalance (31:2.4:1 ratio)

## Project Structure
```
mini-project-7/
├── README.md
├── requirements.txt
├── yolo26n.pt
├── BCCD_YOLO.ipynb
├── Blood-Cell-Cella+BCCD-1/
│   ├── train/
│   │   ├── images/
│   │   └── labels/
│   ├── valid/
│   │   ├── images/
│   │   └── labels/
│   ├── test/
│   │   ├── images/
│   │   └── labels/
│   ├── data.yaml
│   ├── README.dataset.txt
│   └── README.roboflow.txt
└── runs/
    └── detect/
        ├── runs/BCCD/         (training output + model weights)
        ├── val/               (validation run outputs)
        ├── val2/
        └── ...
```

## Setup Instructions

1. Clone the repository:
```bash
git clone https://github.com/hyeonukim/COMP9130/tree/main/mini-project-7
cd mini-project-7
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Download the dataset from Roboflow Universe and place it in the `Blood-Cell-Cella+BCCD-1/` directory, or use the Roboflow API in the notebook to download it automatically. The `data.yaml` file should already be configured with the correct paths.

4. Run the notebook:
   - Open `BCCD_YOLO.ipynb` in Jupyter Lab or Google Colab
   - A GPU runtime is strongly recommended (Tesla T4 or equivalent)

## Methods Used

**Model**
- YOLOv26n (nano variant) from Ultralytics
- Pre-trained on COCO weights (`yolo26n.pt`)
- 122 layers, 2,375,421 parameters, 5.2 GFLOPs

**Training Configuration**
- Epochs: 30
- Image size: 640×640
- Batch size: 16
- Initial learning rate: 0.001
- Early stopping patience: 10 epochs
- Augmentation: Ultralytics default randaugment pipeline (random flip, mosaic, random erase)

**Evaluation Metrics**
- mAP@50 and mAP@50-95 (overall and per-class)
- Precision and Recall
- Normalized confusion matrix

## Results Summary

**Overall Validation Performance (Best Checkpoint)**
- mAP@50: 0.8765
- mAP@50-95: 0.7543
- Precision: 0.8147
- Recall: 0.8200

**Per-Class Performance**

| Class | mAP@50 | mAP@50-95 | Precision | Recall |
|-------|--------|-----------|-----------|--------|
| RBC | 0.931 | 0.827 | 0.829 | 0.876 |
| WBC | 0.925 | 0.856 | 0.882 | 0.893 |
| Platelets | 0.773 | 0.581 | 0.731 | 0.689 |

**Key Findings**

RBC and WBC detection achieved strong performance with mAP@50 above 0.92. Platelets were the most challenging class (mAP@50: 0.773), primarily due to severe class imbalance (only 178 training examples) and their small physical size in the images. The confusion matrix shows 28% of true Platelets were missed entirely (predicted as Background), compared to 21% for RBC and 14% for WBC.

Training was stable across 30 epochs with no signs of overfitting — train and validation losses tracked closely throughout. Both mAP metrics plateaued around epoch 15, indicating the model converged efficiently using transfer learning from COCO pre-trained weights.

The model is not ready for standalone clinical deployment but is viable as a decision-support tool under human oversight. Key improvements needed are a larger Platelet training set, class-weighted loss, and validation on an independent dataset.

## Team Member Contributions

* **Eric**
  * Dataset download and setup
  * Data exploratory analysis and annotation visualization
  * Model training and hyperparameter configuration
  * Training curve visualization
  * Discussion and report

* **Henry**
  * Dataset statistics and class distribution analysis
  * Validation evaluation and per-class metric analysis
  * Confusion matrix visualization
  * Prediction visualization (validation set)
  * README.md
  * requirements.txt
