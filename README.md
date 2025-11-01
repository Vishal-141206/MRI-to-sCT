# MRI-to-Synthetic-CT-Brain-Scan-Translation

**Capstone Project | IIT Mandi Minor in Data Science & Machine Learning**<br>
**Author:** Vishal Kumar <br>

---

## 1. Project Overview

In clinical radiotherapy, planning requires both MRI (for soft-tissue detail) and CT (for radiation dose calculation). This dual-scan process is costly, time-consuming, and exposes patients to unnecessary radiation.

This project aims to solve this challenge by developing and optimizing a deep learning framework to generate high-fidelity synthetic CT (sCT) images directly from T1-weighted MRI scans. The ultimate goal is to enable a safer, faster, and more cost-effective "MRI-only" clinical workflow.

This repository contains the code for all experiments, data preprocessing, and the final state-of-the-art model.

## 2. Key Results & Performance

The primary contribution of this project is a novel **two-phase fine-tuning strategy** that surpassed the fully-trained 81-epoch baseline model in fewer total epochs. This optimized model achieved state-of-the-art results for this task.

| Model Description | Total Epochs | G:D Strategy | Loss Strategy | **PSNR (↑)** | **SSIM (↑)** | **MAE (↓)** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Our Model (E6)** | **78** | **1:1 → 2:1** | **Charb. → Charb.** | **35.74** | **0.962** | **11.49** |
| Baseline (Final) (E1) | 81 | 1:1 | L1 | 34.75 | 0.954 | 13.22 |
| Baseline (Best Val) | 54 | 1:1 | L1 | 34.38 | 0.951 | 13.60 |

---

## 3. Methodology

This project systematically optimized the Pix2Pix framework by evaluating advanced training techniques.

### Tech Stack
* **Python 3.9+**
* **PyTorch:** For building and training the GAN models.
* **NumPy:** For data preprocessing and calculations.
* **Nibabel:** For loading 3D NIfTI files from the dataset.
* **Matplotlib:** For plotting and image visualization.

### Core Architecture
* **Generator:** A U-Net with depthwise separable convolutions for computational efficiency.
* **Discriminator:** A standard `PatchGAN` discriminator, which evaluates realism on patches of the image to encourage high-frequency detail.

### Winning Strategy: Two-Phase Fine-Tuning (E6)
Our best model (E6) was the result of a curriculum-based training strategy:
1.  **Phase 1 (Foundation):** Trained for 40 epochs with a stable **1:1 G:D ratio** and **Charbonnier loss (ε=1e-6)**. This built a strong, general foundation.
2.  **Phase 2 (Fine-Tuning):** Loaded the Phase 1 weights and fine-tuned for 38 epochs with an aggressive **2:1 G:D ratio** and a smoother **Charbonnier loss (ε=1e-4)**. This pushed the model to a new performance peak.

---

## 4. Dataset & Preprocessing

* **Dataset:** The project used the **SynthRad dataset**, which contains 180 3D paired and aligned brain scans (MRI, CT, and segmentation masks).
* **Preprocessing:**
    1.  **Slicing:** 3D volumes were sliced into 2D images.
    2.  **Masking:** Brain masks were applied to remove the skull and background.
    3.  **Filtering:** Slices with less than 15% non-zero pixels were discarded.
    4.  **Normalization:**
        * **MRI:** Min-max scaled to `[0, 1]` on a per-slice basis.
        * **CT:** Windowed (`-1000` to `1000` HU) and then scaled to `[0, 1]`.
