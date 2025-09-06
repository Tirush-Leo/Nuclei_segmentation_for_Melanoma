# 🧬 Nuclei Segmentation for Melanoma

This repository presents a **deep learning project for automated nuclei
segmentation in melanoma histopathology images**. It introduces an
enhanced **HoVer-Net architecture with a ConvNeXtV2 encoder**, improving
segmentation precision and classification of **tumor cells, lymphocytes,
and other nuclei**.

The work demonstrates how AI can support **pathologists** in providing
faster, more consistent, and highly accurate analysis, ultimately
improving **melanoma diagnosis and treatment planning**.

------------------------------------------------------------------------

## 📌 Project Overview

-   **Problem**: Manual histopathology analysis is slow, subjective, and
    prone to inter-observer variability.
-   **Solution**: Automated nuclei segmentation & classification
    pipeline using advanced deep learning methods.
-   **Goal**: Improve segmentation accuracy, reduce diagnostic
    ambiguity, and provide a reliable system for biomarker detection in
    melanoma.

------------------------------------------------------------------------

## 📊 Dataset

We used the **PUMA dataset** of melanoma histopathology slides:

🔗 [PUMA Dataset on Zenodo](https://zenodo.org/records/14869398)

-   HE-stained slides with **expert-annotated nuclei masks**
-   Three main categories:
    -   **Tumor nuclei**
    -   **Lymphocytes**
    -   **Other nuclei** (stromal, apoptotic, plasma cells, etc.)
-   Preprocessed into **1024×1024 patches**
-   Stain normalization + augmentation for robustness

------------------------------------------------------------------------

## 🧠 Methodology

### 1. Preprocessing

```{=html}
<p align="center">
```
`<img src="/images/pre_process.png" width="700"/>`{=html}
```{=html}
</p>
```
*Resized Image → Optimized Normalized Image → HSV Foreground Mask.*

Steps include:
- **Resizing** WSIs into patches
- **Macenko stain normalization** for consistency
- **HSV thresholding** to isolate nuclei-rich regions
- **Data augmentation**: flips, rotations, elastic distortions

------------------------------------------------------------------------

### 2. Model Architecture

```{=html}
<p align="center">
```
`<img src="images/Model_architecture.png" width="700"/>`{=html}
```{=html}
</p>
```
*Modified HoVer-Net with ConvNeXtV2 backbone.*

-   **Encoder**: ConvNeXtV2 (better feature extraction than ResNet-50)
-   **Decoder**: U-Net style with skip connections
-   **Branches**:
    1.  **Nuclear Pixel (NP) Branch** -- Binary masks for nuclei
    2.  **HoVer Branch** -- Horizontal & vertical distance maps for
        clustered nuclei separation
    3.  **Classification Branch** -- Assigns nucleus type (Tumor,
        Lymphocyte, Other)
-   **Loss functions**: Dice Loss + Cross-Entropy Loss + HoVer Loss

------------------------------------------------------------------------

### 3. Training

-   Optimizer: **Adam** (lr=1e-4 with scheduler)
-   Batch size: **8 per GPU**
-   Epochs: **50 with early stopping**
-   Mixed loss strategy to balance segmentation & classification

------------------------------------------------------------------------

### 4. Evaluation

Metrics used:
- **F1-score** per class + micro average
- **Dice coefficient**
- **IoU** (Intersection over Union)

------------------------------------------------------------------------

## 📈 Results

### Quantitative Results  

| Model                          | Tumor F1 | Lymphocyte F1 | Other F1 | Micro F1 | Avg F1 |
|--------------------------------|----------|---------------|----------|----------|--------|
| NN192                          | 0.59     | 0.11          | 0.13     | 0.46     | 0.28   |
| Hover-NeXt (PanNuke)           | 0.68     | 0.68          | 0.38     | 0.61     | 0.58   |
| Proposed (HoVer-Net + ConvNeXtV2) | **0.88** | **0.70** | **0.73** | **0.73** | **0.69** |


------------------------------------------------------------------------

### Qualitative Results

```{=html}
<p align="center">
```
`<img src="Images/Results.png" width="700"/>`{=html}
```{=html}
</p>
```
*Example segmentation results: Original WSIs → Ground Truth →
Predictions.*

-   Clear separation of overlapping nuclei\
-   Accurate detection of tumor and lymphocytes\
-   Some misclassifications remain in "Other nuclei"

------------------------------------------------------------------------

## ⚡ Limitations

-   Performance still affected by **staining variability**\
-   Classification of **"Other nuclei"** less reliable than
    tumor/lymphocyte\
-   Model has **high computational cost**, limiting real-time deployment

------------------------------------------------------------------------

## 🔮 Future Work

-   **Domain adaptation** across different labs/institutions\
-   **Self-supervised learning** to reduce annotation dependency\
-   **Model optimization** (pruning/quantization for real-time use)\
-   Integration into **clinical pathology workflows**
