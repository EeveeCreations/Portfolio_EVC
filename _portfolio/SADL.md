---
title: "StarNet: Image Segmentation in Astronomy"
authors: "Eefje Karremans, Chloé Tap, Barbaros Isik, Naël Belhaj Haddou"
date: May 2025
layout: project
image: /img/1.jpg
category: "Machine Learning / Astronomy"
---

## Abstract

The **StarNet** project explores how transformer-based segmentation models can be applied to complex astronomical imagery.  
Using the **Segment Anything Model (SAM)** as a foundation, we developed a preprocessing and fine-tuning pipeline for Hubble deep field images, adapting techniques from microscopy and medical imaging to detect and segment celestial sources.  
This work demonstrates that advanced computer vision models can accelerate astronomical data analysis, reducing manual labelling effort and improving consistency across massive sky surveys.

---

## 1. Introduction

Astronomy is facing a data explosion. Modern observatories like **Hubble** and **JWST** generate terabytes of images containing millions of faint galaxies and stars.  
Traditional manual labelling and deblending are not scalable for upcoming large-scale projects such as the **LSST**, which will produce over **500 PB of data**.

Deep learning has transformed computer vision tasks in other fields — particularly **medical imaging**, **microscopy**, and **remote sensing**. Models such as **U-Net**, **CNNs**, and **transformers** have shown strong performance for segmentation and classification tasks.

However, astronomical images are uniquely challenging:
- High dynamic range (bright and faint sources in one frame)
- Overlapping and blended sources
- Sparse labels and non-standard data formats (FITS)

To address this, we evaluate **Meta’s Segment Anything Model (SAM)** — a transformer-based system capable of **zero-shot segmentation** with minimal additional fine-tuning.  
SAM has shown remarkable flexibility in dense and complex scenes such as cellular imaging. Our project tests whether similar adaptability applies to **astronomical images**.

> **Goal:** Fine-tune SAM on processed Hubble images, evaluate segmentation performance, and explore how preprocessing and augmentation affect accuracy.

---

## 2. Methods

### 2.1 Data

- **Source:** MAST (Mikulski Archive for Space Telescopes) — GOODS-S and GOODS-N CANDELS fields  
- **Instrument:** Hubble’s **ACS** (30 mas/pixel) and **WFC** (60 mas/pixel) cameras  
- **Data format:** FITS files converted to RGB composites

![Figure 1: SAM model overview](images/starnet_sam_overview.png)

---

### 2.2 Image Preprocessing

Astronomical images must be converted from **FITS** format into conventional image tensors while retaining essential photometric information.

**Pipeline overview:**
1. **Sky background removal** with a rolling median filter  
2. **Percentile clipping** to remove outliers (inner 95% intensity range)
3. **Median filtering** (kernel = 10) to reduce speckled noise  
4. **Channel normalisation** to [0, 1] range:
   \[
   p_{norm} = \frac{p - \min(p)}{\max(p) - \min(p)}
   \]
5. **RGB stacking**:
   - Red → F160W (IR)
   - Green → F850LP (Optical/IR)
   - Blue → F606W (Optical)

Resulting images are tiled into ~682×682 composites, balancing resolution and compute efficiency.

> **Libraries:** `Astropy`, `NumPy`, `SciPy.ndimage`, and `Matplotlib` — lightweight and astronomy-compatible.

![Figure 2: Preprocessed GOODS-S tile](images/starnet_preprocessed_tile.png)

---

### 2.3 Data Augmentation

To compensate for the small fine-tuning dataset (≈15 images), we applied diverse augmentations:

- Random rotations (±30°)
- Horizontal/vertical flips
- Scaling (±10%) and translation
- Gamma and brightness shifts
- Coarse dropout and random cropping

These transformations increase sample diversity and help the model generalize to different image orientations and contrasts.

![Figure 3: Augmented samples](images/starnet_augmentation_examples.png)

---

### 2.4 Segmentation Approaches

We compared three segmentation strategies:

1. **Direct inference:** pre-trained SAM applied to full images.  
2. **Tiled inference:** images split into overlapping chunks, processed individually, and reassembled.  
3. **Fine-tuned SAM:** retrained on Hubble images using corrected segmentation masks.

Fine-tuning was performed on an **NVIDIA RTX 4060 GPU** (3 epochs, ViT-B backbone).  
Although data was limited, previous studies (Archit et al., 2023) show strong gains with even 1–5 training samples.

---

## 3. Experiments

### 3.1 Fine-Tuning

We fine-tuned the SAM model using manually corrected masks derived from initial predictions.  
The dataset was split into:
- 10 training images  
- 2 validation images  
- 3 test images  

Future versions will include augmented data and extended epochs for larger models (e.g., ViT-H).

---

### 3.2 Evaluation Metrics

Segmentation performance was assessed via **Intersection over Union (IoU)**:

\[
IoU = \frac{|Mask_{true} \cap Mask_{pred}|}{|Mask_{true} \cup Mask_{pred}|}
\]

We also examined qualitative differences in mask boundaries and faint object detection.

---

## 4. Results

### 4.1 Quantitative Metrics

| Dataset | Mean IoU |
|----------|-----------|
| Training | 0.20 |
| Validation | 0.20 |
| Test | 0.18 |

### 4.2 Qualitative Comparison

- **Direct Inference:** detects few objects with moderate precision  
- **Tiled Inference:** detects many objects but introduces noise and false positives  
- **Fine-tuned SAM:** balances coverage and accuracy, detecting more objects while reducing artefacts

![Figure 4: Segmentation comparison](images/starnet_segmentation_comparison.png)

---

## 5. Discussion

Fine-tuning improved segmentation quality despite the limited dataset.  
SAM’s transformer-based architecture adapted well to astronomical image structure, but results are still constrained by:
- Small sample size  
- Limited FITS-to-RGB fidelity  
- Imperfect mask corrections  

Nonetheless, the results suggest that **transfer learning from microscopy to astronomy** is feasible and valuable.

---

## 6. Conclusion & Future Work

StarNet demonstrates that **SAM can segment astronomical images** with minimal retraining.  
To further improve:

- Test multiple SAM variants (ViT-L, ViT-H)
- Automate prompt generation for faster inference
- Experiment with direct **FITS-based encoders (e.g., VAEs)**
- Add a **classification layer** for galaxy morphology
- Expand dataset with JWST and LSST imagery

Improving annotation quality and cross-survey diversity will help the model generalize, paving the way toward **AI-assisted sky mapping**.

---

### Figures
![Placeholder 1](images/starnet_placeholder1.png)
![Placeholder 2](images/starnet_placeholder2.png)
![Placeholder 3](images/starnet_placeholder3.png)

---

*Last updated: October 2025*
