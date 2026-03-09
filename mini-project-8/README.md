# Mini Project 8: Flood Area Image Segmentation

**COMP 9130 Applied AI — Natural Disaster Damage Assessment**  
Jacky Chen & Eric Kim | March 2026

---

## Problem Description

This project applies semantic segmentation to aerial and UAV imagery of flooded areas. A U-Net model labels every pixel as either **Background** or **Flood**, enabling automated flood extent mapping for emergency response teams.

**Dataset:** [Flood Area Segmentation](https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation) — 290 aerial images with binary masks (flood / no-flood). After removing 41 corrupt pairs, 249 valid image-mask pairs were used.

| Split | Images |
|-------|--------|
| Train | 174    |
| Val   | 37     |
| Test  | 38     |

**Classes:**
- `0` — Background (58.7% of pixels)
- `1` — Flood (41.3% of pixels)

---

## Setup Instructions

### 1. Clone the repository
```bash
git clone <your-repo-url>
cd mini-project-8
```

### 2. Install dependencies
```bash
pip install tensorflow numpy matplotlib scikit-learn pillow
```

### 3. Download the dataset

1. Go to https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation
2. Click **Download** to get the zip file
3. Place it in the `data/` folder:
```
mini-project-8/
  data/
    flood-area-segmentation.zip
  segmentation_local.ipynb
```

---

## How to Run

Open `segmentation_local.ipynb` in Jupyter and run cells in order.

| Section | Description |
|---------|-------------|
| 1–2 | Imports and configuration |
| 3 | Unzip dataset (no Kaggle API needed) |
| 4 | Remove corrupt files |
| 5 | Train/val/test split (70/15/15) |
| 6 | Preprocessing and DataLoaders |
| 7 | Explore data + class distribution |
| 8 | U-Net model definition |
| 9 | Train Model A (CE loss, 640x640) |
| 10 | Train Model B (CE+Dice loss, 640x640) |
| 11 | Training history plots |
| 12 | Test set evaluation — per-class IoU and Dice |
| 13 | Prediction visualizations + error maps |
| 14 | Confusion matrix |

**If you get OOM errors:** Reduce `BATCH_SIZE` from `8` to `4` in the configuration cell.

---

## Results

### Model Comparison

| Model | Resolution | BG IoU | Flood IoU | mIoU | Mean Dice |
|-------|-----------|--------|-----------|------|-----------|
| Full U-Net (CE) | 640x640 | 0.7824 | 0.7024 | **0.7424** | **0.8367** |
| Full U-Net (CE+Dice) | 640x640 | 0.7675 | 0.6659 | 0.7167 | 0.8144 |

**Winner: Cross-Entropy model** — mIoU 0.7424, Mean Dice 0.8367

The CE+Dice model underperformed due to early training instability (val_loss did not improve for 14 epochs, spiking to 29.6 at epoch 3). With mild class imbalance (58.7% / 41.3%), CE was sufficient.

### Confusion Matrix (CE+Dice model, test set)

|  | Predicted BG | Predicted Flood |
|--|-------------|-----------------|
| **True BG** | 7,706,179 | 1,004,942 |
| **True Flood** | 1,191,964 | 5,661,715 |

Errors concentrate at flood-background boundaries and in visually ambiguous zones (muddy water, shallow water over vegetation). See notebook Section 13 for full prediction visualizations with error maps.

---

## Team Member Contributions

| Member | Contributions |
|--------|--------------|
| **Jacky Chen** | U-Net architecture, loss functions, training pipeline, evaluation metrics (IoU/Dice/confusion matrix), data augmentation, report |
| **Eric** | Dataset acquisition and cleanup, data exploration, visualization code (predictions, probability maps, box plots), README |

---

## File Structure

```
mini-project-8/
├── segmentation_local.ipynb   # Main notebook
├── README.md
├── data/
│   └── flood-area-segmentation.zip   # Download manually from Kaggle
├── checkpoints/               # Saved model weights (auto-created, git-ignored)
└── .gitignore
```

---

## References

- Ronneberger et al. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. MICCAI 2015.
- Karim, F. (2022). Flood Area Segmentation Dataset. Kaggle.
- Milletari et al. (2016). V-Net. 3DV 2016. (Dice loss formulation.)
