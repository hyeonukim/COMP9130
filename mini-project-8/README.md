# Mini Project 8: Flood Area Image Segmentation

**Natural Disaster Damage Assessment**  
Eric Kim & Jacky Chen | March 2026

---

## Problem Description

Natural disasters like floods cause devastating loss of life and infrastructure damage every year. When a flood occurs, emergency response teams need to know fast which areas are underwater, where roads are impassable, and where to deploy rescue resources. Traditionally, this mapping is done manually by analysts reviewing aerial imagery, which is slow and cannot scale to a large disaster zone.

Semantic segmentation using deep learning offers a faster alternative: given an aerial image, a trained model can label every pixel as flood water or dry land in seconds, producing an automated flood extent map that would take a human analyst hours to create.

This project builds a U-Net model trained on aerial and UAV imagery to perform binary flood segmentation: labeling every pixel as either **Background** (dry land, vegetation, structures) or **Flood** (water-covered areas). The model is trained and evaluated on the Kaggle Flood Area Segmentation dataset, which contains real aerial imagery captured during flood events.

**Dataset:** [Flood Area Segmentation](https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation) — 290 aerial images with binary pixel-level masks. After programmatically detecting and removing 41 corrupt image-mask pairs, 249 valid pairs remained. Images were resized to 640×640 for training, exceeding the 256×256 resolution used in class to capture the fine boundary detail required for accurate flood mapping.

| Split | Images |
|-------|--------|
| Train | 174    |
| Val   | 37     |
| Test  | 38     |

**Classes:**
- `0` — Background (58.7% of pixels)
- `1` — Flood (41.3% of pixels)

---

## File Structure

```
mini-project-8/
├── segmentation.ipynb             # Main notebook
├── README.md
├── figures/                       # Saved plot images for README
│   ├── training_curves.png
│   ├── per_class_distribution.png
│   ├── confusion_matrix.png
│   ├── predictions.png
│   └── confidence_map.png
├── data/
│   └── flood-area-segmentation.zip   # Download manually from Kaggle
├── checkpoints/                   # Saved model weights (auto-created, git-ignored)
└── .gitignore
```

---

## Setup Instructions

### Option A — Local GPU

#### 1. Clone the repository
```bash
git clone <your-repo-url>
cd mini-project-8
```

#### 2. Install dependencies
```bash
pip install tensorflow numpy matplotlib scikit-learn pillow
```

#### 3. Download the dataset
1. Go to https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation
2. Click **Download** to get the zip file
3. Place it in the `data/` folder:
```
mini-project-8/
  data/
    flood-area-segmentation.zip
  segmentation.ipynb
```

---

### Option B — Google Colab (no GPU required)

1. Go to [Google Colab](https://colab.research.google.com) and open `segmentation.ipynb` from your GitHub repo:
   - **File → Open notebook → GitHub** → paste your repo URL
2. Enable GPU: **Runtime → Change runtime type → T4 GPU**
3. Upload your `kaggle.json` API token when prompted by the first notebook cell:
   - Get it from kaggle.com → Account → Settings → API → **Create New Token**
   - The first cell handles placing it in the correct location automatically
4. Run all cells in order — the dataset will be downloaded automatically via the Kaggle API

> **Note:** Colab sessions disconnect after ~12 hours of inactivity. Make sure `ModelCheckpoint` is saving to Google Drive or download your `.keras` checkpoint files before the session ends. To save to Drive, add this before training:
> ```python
> from google.colab import drive
> drive.mount('/content/drive')
> # Then set checkpoint path to '/content/drive/MyDrive/checkpoints/unet_ce_best.keras'
> ```

---

## How to Run

Open `segmentation.ipynb` in Jupyter and run cells in order.

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

### Training History 

![Training History](/mini-project-8/figures/training_curves.png)

Both models were trained for 35 epochs. Model A (CE) converged steadily, with val_loss improving from 1.98 at epoch 1 to a best of 0.2823 at epoch 33. ReduceLROnPlateau triggered at epoch 30, reducing the learning rate from 5e-4 to 2.5e-4 and producing the best checkpoints. Model B (CE+Dice) showed severe instability in early training: val_loss spiked as high as 29.6 at epoch 3 and did not meaningfully improve until epoch 15, finishing at a best val_loss of 0.4731.

**Winner: Cross-Entropy model** — mIoU 0.7424, Mean Dice 0.8367

The CE+Dice model underperformed due to early training instability (val_loss did not improve for 14 epochs, spiking to 29.6 at epoch 3). With mild class imbalance (58.7% / 41.3%), CE was sufficient and the added Dice term introduced more noise than benefit.


### Per-Class Distribtion

![Per Class Distribution](/mini-project-8/figures/per_class_distribution.png)

The box plots show the spread of per-image IoU and Dice scores across the test set for each class. Background (blue) achieves consistently higher scores with a tighter distribution (IoU 0.7824, Dice 0.8712), reflecting the more visually consistent appearance of dry land and vegetation. Flood (orange) has a wider spread with more low-score outliers (IoU 0.7024, Dice 0.8022), indicating that some flood images are significantly harder to segment than others. The hardest cases are images where flood water is muddy or shallow, making it visually similar to wet soil or saturated vegetation.


### Confusion Matrix (CE+Dice model, test set)

|  | Predicted BG | Predicted Flood |
|--|-------------|-----------------|
| **True BG** | 7,706,179 | 1,004,942 |
| **True Flood** | 1,191,964 | 5,661,715 |

![Confusion Matrix](/mini-project-8/figures/confusion_matrix.png)

The model correctly classifies the majority of pixels in both classes. The Flood miss rate (17.4% — 1,191,964 flood pixels labelled as background) is higher than the Background miss rate (11.5% — 1,004,942 background pixels labelled as flood). This asymmetry is consistent with the lower Flood IoU (0.6659) and reflects the greater visual ambiguity of flood pixels: water appearance varies with lighting, depth, and suspended sediment, making some flood regions look nearly identical to wet background.

---


## Sample Predictions 

![Sample Predictions](/mini-project-8/figures/predictions.png)

Each row shows an input image, its ground truth mask, the model's prediction, and an error map (red = incorrect pixels). The model performs well on images with clear colour contrast between flood water and dry land. Errors concentrate at class boundaries rather than in region interiors. Large homogeneous flood or background regions are predicted correctly, while mistakes appear as thin rings at the flood-background transition. The worst predictions occur when flood water and background share similar tones (e.g., grey water under overcast skies, or muddy water near brown soil).

---

## Sample Confidence / Probability Map Visualization

![Sample Confidence](/mini-project-8/figures/confidence_map.png)

The probability maps show the softmax output for each class: brighter pixels indicate higher model confidence. P(Background) and P(Flood) are complementary: where one is bright, the other is dark. The model is highly confident (probability near 1.0) in the centres of large flood and background regions. Confidence drops at boundaries, where the probability values are closer to 0.5, reflecting genuine visual ambiguity. These low-confidence boundary zones correspond closely to the error regions visible in the error maps above.



## Team Member Contributions

| Member | Contributions |
|--------|--------------|
| **Jacky** | Data exploration, evaluation metrics (IoU/Dice/confusion matrix), visualizations (predictions, probability maps, box plots), README, report |
| **Eric** | Dataset acquisition and cleanup ,U-Net architecture, loss functions, training pipeline, data augmentation, hyperparameter tuning, model checkpointing |

---

## References

- Ronneberger et al. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. MICCAI 2015.
- Karim, F. (2022). Flood Area Segmentation Dataset. Kaggle.
- Milletari et al. (2016). V-Net. 3DV 2016. (Dice loss formulation.)
