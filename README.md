# Anomaly-Based Intrusion Detection System — Multi-Stage 1D-CNN

> A high-performance Network Intrusion Detection System (NIDS) using a custom **Multi-Stage Feature Fusion 1D-CNN** trained on the NSL-KDD dataset.

[![Framework](https://img.shields.io/badge/Framework-TensorFlow%2FKeras-orange?style=flat-square)](https://www.tensorflow.org/)
[![Dataset](https://img.shields.io/badge/Dataset-NSL--KDD-blue?style=flat-square)](https://www.unb.ca/cic/datasets/nsl.html)
[![Language](https://img.shields.io/badge/Language-Python%203.x-green?style=flat-square)](https://www.python.org/)

---

## Overview

This project implements a high-performance **Intrusion Detection System (IDS)** using a custom **Multi-Stage Feature Fusion 1D-CNN**. Unlike traditional machine learning models (SVM, Random Forest) that rely on manual feature engineering, this system leverages deep learning to automatically extract hierarchical spatial patterns from the **NSL-KDD dataset**.

By reshaping 122-dimensional network traffic vectors into 1D sequences, the model utilizes multi-scale convolutional kernels to capture both local dependencies and global anomalies. The architecture uniquely fuses intermediate convolutional representations with deep MLP features, enabling the detection of complex, low-footprint attack vectors (U2R, R2L) alongside high-volume DoS attacks.

---

## Model Architecture

```
[Input: 122 Features] ──> [Reshape: (1, 122)]
                               │
         ┌───────────────────┼───────────────────┐
         │                     │                     │
   [Conv1D Block 1]      [Conv1D Block 2]      [Conv1D Block 3]
  32→62 filters (k=2)   62 filters (k=4)     124 filters (k=8)
    SpatialDropout         MaxPooling            MaxPooling
         │                     │                     │
      Flatten              Flatten              Flatten ──> [Dense: 256]
         │                     │                     │         │
         └──────────────────┼──────┼──────────────────┘
                                │
                          [Concatenate]
                                │
                          [Output Layer]
               (Binary: Sigmoid | Multi-class: Softmax)
```

---

## Technical Highlights

### Multi-Scale Feature Fusion
A non-sequential **Keras Functional API** model concatenates flattened outputs from three distinct CNN blocks with a deep MLP branch. This fusion strategy ensures the final classifier retains both low-level local feature patterns and high-level abstract representations simultaneously.

### Advanced Regularization
- **Spatial Dropout (`SpatialDropout1D`, rate=0.1):** Drops entire 1D feature maps, preventing co-adaptation of correlated network traffic features
- **Multi-Scale Receptive Fields:** Kernel sizes of 2, 4, and 8 capture anomalies at different temporal granularities
- **EarlyStopping (patience=40):** Prevents overfitting without manual epoch tuning

### Class Imbalance Mitigation
Integrated `sklearn.utils.class_weight` to compute balanced class weights, dynamically penalizing misclassifications of minority attack classes (U2R/R2L) during the training loop — the most difficult-to-detect attack categories.

### Custom Learning Rate Schedule
A `LearningRateScheduler` decays the learning rate from **0.5 → 0.001** after 108 epochs, paired with EarlyStopping to ensure convergence without overfitting on the imbalanced dataset.

---

## Dataset — NSL-KDD

| Split | Samples | Attack Ratio |
|---|---|---|
| KDDTrain+ | 125,973 | ~46% |
| KDDTest+ | 22,544 | ~46% |

**Attack categories:** DoS, Probe, Remote-to-Local (R2L), User-to-Root (U2R)

**Preprocessing pipeline:**
- `OneHotEncoder` for categorical features (protocol type, service, flag)
- `MinMaxScaler` for numerical feature normalization
- Reshape to `(n_samples, 1, 122)` for 1D convolution compatibility

---

## Results

| Mode | Metric | Value |
|---|---|---|
| Binary Classification | Accuracy | High (see notebook) |
| Multi-class Classification | F1-Score (macro) | Competitive on minority classes |

> Full classification reports and confusion matrices are available in the notebook.

---

## Skills Demonstrated

- **Deep Learning:** TensorFlow/Keras Functional API, non-sequential architectures, custom weight initialization
- **Data Engineering:** Pandas, NumPy, complex schema mapping for the NSL-KDD feature set
- **Model Optimization:** Hyperparameter tuning, learning rate scheduling, cost-sensitive learning
- **Security Domain:** Network traffic anomaly detection, attack taxonomy (DoS, Probe, U2R, R2L)
- **Diagnostics:** `classification_report`, `confusion_matrix`, training history visualization

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/tamer017/Anomaly-baised-Intrusion-detection-system-CNN1D.git
cd Anomaly-baised-Intrusion-detection-system-CNN1D

# Install dependencies
pip install tensorflow scikit-learn pandas numpy matplotlib

# Launch the notebook
jupyter notebook Anomaly_baised_Intrusion_detection_system_CNN1D.ipynb
```

> **Dataset:** Download the [NSL-KDD dataset](https://www.unb.ca/cic/datasets/nsl.html) from the Canadian Institute for Cybersecurity and place `KDDTrain+.txt` and `KDDTest+.txt` in the project root.

---

## References

- [NSL-KDD Dataset — University of New Brunswick](https://www.unb.ca/cic/datasets/nsl.html)
- [TensorFlow Keras Functional API](https://www.tensorflow.org/guide/keras/functional)
- Tavallaee et al. (2009). *A Detailed Analysis of the KDD CUP 99 Data Set*. IEEE CISDA.
