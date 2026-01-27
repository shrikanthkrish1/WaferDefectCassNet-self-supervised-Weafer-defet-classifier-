# WaferDefectNet  
### PVM-Guided Self-Supervised Wafer Defect Discovery and Classification

This repository implements an end-to-end wafer inspection pipeline combining Pyramid Voting (PVM), deep feature learning, clustering, anomaly detection, and classification.

The system automatically discovers defect patterns from wafer maps and detects previously unseen anomalies with minimal supervision.

---

## Overview

Pipeline:

Wafer Image  
→ Pyramid Voting Module (PVM)  
→ Core Mask + Confidence Map  
→ CNN Encoder  
→ 128-D Embedding  
→ DBSCAN Clustering  
→ Defect Discovery + Anomaly Detection  
→ Classifier  
→ Final Prediction

---

## Key Features

- Robust defect extraction using Pyramid Voting
- CNN-based embedding learning
- Density-based clustering for automatic defect discovery
- Explicit anomaly detection
- Lightweight classifier for deployment
- Designed for semiconductor wafer inspection

---

## Dataset

WM811K Silicon Wafer Map Dataset.

Each wafer is processed at full resolution before resizing for the encoder.

---

## Pyramid Voting Module (PVM)

PVM extracts reliable defect pixels using geometric consistency.

Steps:

1. Generate multiple transformed views (rotations, flips, scaling)
2. Run DBSCAN on each view
3. Inverse-map masks to original coordinates
4. Perform pixel-wise voting
5. Produce:
   - Core defect mask
   - Confidence map

Only pixels stable across transformations survive.

This removes noise while preserving true defect structures.

---

## Wafer Filtering

Wafers with no detected defect pixels are removed before training.

This avoids learning from empty or meaningless samples.

---

## Encoder Network

Each wafer becomes a 3-channel input:

1. Original wafer
2. PVM core mask
3. PVM confidence map

A CNN encoder converts this into a 128-dimensional embedding.

---

## Defect Discovery

Embeddings are standardized and clustered using DBSCAN:

- Dense regions → defect families
- Sparse samples → anomalies

This enables unsupervised discovery of defect types.

---

## Classifier

A lightweight neural classifier is trained on embeddings.

Classes:

- Discovered defect clusters
- One anomaly class

---

## Inference

For a new wafer:

1. Apply PVM
2. Encode wafer
3. Standardize embedding
4. Classify

Output:

- Defect class OR anomaly
- Confidence score

---

## Evaluation

Metrics:

- Accuracy
- Macro F1
- Weighted F1
- Confusion Matrix

Typical results:

- Accuracy ≈ 94%
- Weighted F1 ≈ 0.97

---

## Repository Structure

