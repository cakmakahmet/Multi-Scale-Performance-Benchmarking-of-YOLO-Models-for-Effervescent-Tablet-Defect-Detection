# Multi-Scale Performance Benchmarking of YOLO Models for Effervescent Tablet Defect Detection

This repository contains the implementation, experiments, and evaluation pipeline for a comparative study on recent YOLO families for effervescent tablet defect detection. The project benchmarks **YOLO11**, **YOLO12**, and **YOLO26** across all five standard scale variants (**n, s, m, l, x**) under identical training conditions to analyze the trade-off between detection accuracy, inference speed, and computational cost.

## Overview

Effervescent tablets are highly hygroscopic pharmaceutical products, and even minor surface defects may affect product stability, dose uniformity, and overall quality. This repository focuses on automated multi-class defect detection for effervescent tablets using modern YOLO-based object detectors.

The study introduces a dedicated effervescent tablet defect dataset and provides a systematic benchmark of fifteen YOLO model variants. In addition to quantitative evaluation, the repository includes class-wise analysis, confusion matrices, precision–recall curves, and Grad-CAM-based visual interpretation.

## Main Contributions

- Introduction of a **new effervescent tablet defect dataset**
- Benchmarking of **YOLO11, YOLO12, and YOLO26**
- Evaluation of **15 model variants** across **n/s/m/l/x** scales
- Comparison using:
  - mAP@0.5
  - mAP@0.5:0.95
  - Precision
  - Recall
  - FPS
  - Parameter count
  - FLOPs
- Class-wise analysis of the best-performing model
- Grad-CAM visualizations for model interpretability

## Dataset

The dataset was constructed specifically for this study and contains:

- **251 high-resolution images**
- **12 tablets per image**
- **3,012 annotated bounding-box instances**
- **6 defect/condition classes**:
  - `intact`
  - `damaged`
  - `cracked`
  - `broken`
  - `moist`
  - `stained`

Each tablet instance is manually annotated in **YOLO format** with normalized bounding-box coordinates.

### Class Distribution

| Class   | Count |
|---------|------:|
| intact  | 617   |
| damaged | 609   |
| cracked | 445   |
| broken  | 400   |
| moist   | 353   |
| stained | 588   |

## Data Split

The dataset is divided into:

- **70% Training**
- **20% Validation**
- **10% Testing**

A stratified split is used to preserve class representation across all subsets.

## Training Setup

All models were trained under the same experimental conditions for fair comparison.

- **Framework:** Ultralytics YOLO
- **Input size:** 640 × 640
- **Epochs:** 100
- **Hardware:** NVIDIA RTX A5000 24GB
- **Augmentations:** default Ultralytics augmentation pipeline
  - horizontal / vertical flipping
  - random rotation
  - random scaling
  - random translation
  - HSV augmentation
  - mosaic augmentation

## Evaluated Models

This repository benchmarks the following YOLO families:

- **YOLO11**
- **YOLO12**
- **YOLO26**

Each family is evaluated at five scales:

- **n** (nano)
- **s** (small)
- **m** (medium)
- **l** (large)
- **x** (extra-large)

## Evaluation Metrics

The models are evaluated using the following standard object detection metrics:

- **Precision**
- **Recall**
- **mAP@0.5**
- **mAP@0.5:0.95**
- **FPS**
- **Parameters (M)**
- **FLOPs (G)**

## Quantitative Results

### Overall Model Comparison

| Model    | mAP@0.5 | mAP@0.5:0.95 | Precision | Recall | FPS   | Params (M) | FLOPs (G) |
|----------|--------:|-------------:|----------:|-------:|------:|-----------:|----------:|
| YOLO11N  | 95.6 | 90.6 | 94.0 | 91.4 | 345.9 | 2.5 | 6.4 |
| YOLO11S  | 96.0 | 91.2 | 92.0 | 93.6 | 224.2 | 9.4 | 21.6 |
| YOLO11M  | 96.7 | 91.6 | 94.8 | 93.1 | 185.2 | 20.0 | 68.2 |
| YOLO11L  | **96.8** | 91.7 | 91.5 | 93.3 | 142.9 | 25.3 | 87.3 |
| YOLO11X  | 96.6 | 91.4 | 94.3 | 92.6 | 88.5 | 56.8 | 195.5 |
| YOLO12N  | 95.6 | 90.6 | 92.9 | 91.0 | 303.0 | 2.5 | 6.5 |
| YOLO12S  | 96.7 | 90.8 | 90.0 | 92.3 | 243.9 | 9.2 | 21.5 |
| YOLO12M  | 96.3 | 91.2 | 92.6 | 93.1 | 169.5 | 20.1 | 67.8 |
| YOLO12L  | 96.7 | **91.8** | 93.9 | 93.2 | 69.4 | 26.3 | 89.4 |
| YOLO12X  | 96.3 | 91.4 | 90.1 | 93.1 | 37.0 | 59.1 | 199.9 |
| YOLO26N  | 88.0 | 83.0 | 85.7 | 74.8 | 298.1 | 2.5 | 5.8 |
| YOLO26S  | 94.6 | 89.0 | 88.9 | 91.1 | 254.5 | 9.9 | 22.5 |
| YOLO26M  | 95.6 | 90.3 | 93.4 | 88.9 | 66.7 | 21.7 | 74.8 |
| YOLO26L  | 96.4 | 90.8 | 92.4 | 91.4 | 63.3 | 26.1 | 93.2 |
| YOLO26X  | 96.5 | 91.5 | 93.7 | 92.6 | 52.0 | 58.8 | 208.6 |

## Key Findings

- **YOLO11L** achieves the best overall detection performance with **96.8% mAP@0.5** and **91.7% mAP@0.5:0.95**.
- **YOLO11N** provides the most attractive real-time trade-off with **345.9 FPS**, **95.6% mAP@0.5**, and only **2.5M parameters**.
- **YOLO12** variants achieve competitive accuracy, but their larger scales show a substantial drop in inference speed.
- **YOLO26** performs weakly at the nano scale but becomes competitive at larger scales.

## Best-Performing Model: YOLO11L

### Class-wise Performance

| Class   | Instances | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 |
|---------|----------:|----------:|-------:|--------:|-------------:|
| broken  | 45  | 99.0 | 95.6 | 98.8 | 91.2 |
| cracked | 72  | 86.3 | 96.4 | 97.1 | 92.5 |
| damaged | 85  | 87.3 | 87.1 | 94.0 | 88.5 |
| intact  | 113 | 88.8 | 91.4 | 96.6 | 92.4 |
| moist   | 58  | 92.8 | 89.1 | 95.1 | 91.0 |
| stained | 71  | 95.0 | 100.0 | 99.4 | 94.4 |

### Interpretation

Class-wise results show that:

- **stained** is the easiest class, with the highest class-level performance
- **broken** is also detected very reliably due to clear geometric discontinuities
- **damaged** is the most challenging class because of visual overlap with cracked and broken samples

## Grad-CAM Analysis

Grad-CAM visualizations indicate that the best-performing model focuses on physically meaningful regions such as:

- cracks
- fissures
- moisture patches
- discolorations

This supports the interpretability of the detector and its practical relevance for pharmaceutical inspection systems.

## Authors
- Mustafa Yurdakul
- Ahmet Melih Çakmak

This repository is maintained as part of a collaborative academic study.

## Repository Structure

```text
.
├── data/
│   ├── images/
│   ├── labels/
│   └── data.yaml
├── models/
├── runs/
├── results/
│   ├── confusion_matrices/
│   ├── pr_curves/
│   ├── gradcam/
│   └── sample_predictions/
├── scripts/
│   ├── train.py
│   ├── validate.py
│   ├── test.py
│   └── visualize.py
├── notebooks/
├── README.md
└── requirements.txt
