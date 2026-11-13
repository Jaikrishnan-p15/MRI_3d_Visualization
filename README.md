# Volumetric Brain MRI Tissue Segmentation and 3D Isosurface Reconstruction

[![Course](https://img.shields.io/badge/Course-EC1066%20Biomedical%20Signal%20%26%20Image%20Processing-0f766e.svg)]()
[![Institution](https://img.shields.io/badge/Institution-NIT%20Puducherry-1d4ed8.svg)]()
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776ab.svg)]()
[![Dataset](https://img.shields.io/badge/Dataset-BraTS%202020%20NIfTI-b91c1c.svg)]()

A modular, deterministic Python pipeline for **3D multimodal Brain MRI preprocessing, multi-class tissue/tumor segmentation, 15-metric statistical validation, and interactive 3D anatomical surface reconstruction**. 

Developed as part of **EC1066 – Biomedical Signal and Image Processing** (Department of Electronics and Communication Engineering, National Institute of Technology Puducherry) under the guidance of **Dr. R. Murugan**.

---

## 1. Team & Module Allocation (Roll No. 18–22)

| Track | Roll No. | Team Member | Git Branch | Assigned File(s) | Core Technical Responsibilities |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Track 1: Preprocessing & Segmentation** | `EC24B1020`<br>`EC24B1022` | **Kiran P P**<br>**Lakshmi Lahari Gajji** | `feat/prep-seg` | `src/preprocess.py`<br>`src/segment.py` | **Kiran:** NIfTI (`.nii.gz`) ingestion via `nibabel`, voxel spacing (`dx, dy, dz`), 3D Gaussian/Anisotropic denoising, `[0, 1]` normalization, SNR/PSNR logging.<br>**Lahari:** Foreground masking, Multi-Otsu thresholding & sorted K-Means ($K=4$) clustering, 3D morphological opening/closing cleanup. |
| **Track 2: Evaluation Metrics & Plots** | `EC24B1019`<br>`EC24B1021` | **Jaisurya K**<br>**Kona Chaithanya** | `feat/eval-metrics` | `src/evaluate_metrics.py` | **Jaisurya:** Computation of **15 quantitative evaluation metrics** exported to `outputs/metrics_summary.csv`.<br>**Chaithanya:** 300-DPI multi-planar (Axial, Coronal, Sagittal) ground-truth vs. predicted overlay generation in `outputs/slices/`. |
| **Track 3: 3D Visualization, Integration & Publication** | `EC24B1018` | **Jaikrishnan P** *(Lead)* | `main` & `docs/springer` | `main.py`<br>`src/viz_3d.py`<br>`report/springer_main.tex` | End-to-end orchestrator, **Marching Cubes** 3D triangulation (`skimage.measure.marching_cubes`), interactive Plotly WebGL HTML viewer, and **Springer LNCS** manuscript. |

---

## 2. System Pipeline & Architecture

```text
BraTS 2020 NIfTI (.nii / .nii.gz)
        │
        ▼
┌──────────────────────────────────────────────────────────┐
│ Track 1A: Preprocessing (src/preprocess.py)              │
│ • Voxel spacing extraction (1×1×1 mm³)                   │
│ • 3D Gaussian & Edge-Preserving Denoising + [0,1] Norm   │
└───────────────────────────┬──────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────┐
│ Track 1B: Multi-Class Segmentation (src/segment.py)      │
│ • Multi-Otsu Adaptive Thresholding & K-Means (K=4)       │
│ • 3D Morphological Opening/Closing Refinement            │
└──────────────┬───────────────────────────┬───────────────┘
               ▼                           ▼
┌────────────────────────────┐ ┌───────────────────────────┐
│ Track 2: Evaluation        │ │ Track 3: 3D Surface Mesh  │
│ (src/evaluate_metrics.py)  │ │ (src/viz_3d.py)           │
│ • 15 Quantitative Metrics  │ │ • Lewiner Marching Cubes  │
│ • 300-DPI 2D Slice Plots   │ │ • Interactive Plotly HTML │
└────────────────────────────┘ └───────────────────────────┘
