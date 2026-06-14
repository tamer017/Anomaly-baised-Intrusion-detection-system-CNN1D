# Anomaly-Based Intrusion Detection System — 1D-CNN

> **Multi-class network traffic classifier achieving 93.2% accuracy and 93.1% F1-score on NSL-KDD using a multi-scale parallel 1D-CNN architecture.**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
[![Dataset](https://img.shields.io/badge/Dataset-NSL--KDD-green.svg)](https://www.unb.ca/csd/security/nsl.html)

---

## Overview

This project implements a high-performance **Network Intrusion Detection System (NIDS)** using a custom **Multi-Stage Feature Fusion 1D-CNN**. Unlike traditional signature-based IDS, this system learns anomaly patterns directly from raw network traffic features, making it robust against novel attack variants.

Three parallel convolutional branches with kernel sizes **2, 4, and 8** capture short, medium, and long temporal dependency patterns simultaneously. The outputs are concatenated and passed through dense layers for final classification. The entire **NSL-KDD benchmark dataset** was used to train this model.

---

## Data Preprocessing

![preprocessing](https://user-images.githubusercontent.com/83555471/189566619-215bcc28-6e00-4d38-b7b7-2673ff0a6b5a.png)

---

## Architecture

The proposed model consists of seven blocks: an input block, an output block, three CNN blocks, and two MLP (fully connected) blocks.

- **CNN blocks**: 1D convolution layer + 1D max pooling layer + Dropout (0.1 rate)
- **Input layer**: size (1, 122)
- **1D Conv layers**: sizes (1,62), (1,62), (1,124) with kernel sizes 2, 4, and 8 respectively
- **Max pooling**: pool sizes 2, 4, and 8 respectively
- **MLP blocks**: 256 and 5 neurons with ReLU and softmax activations
- **Output**: sigmoid for binary, softmax for 5-class classification

```
Input (41 NSL-KDD Features)
        │
┌───────┼────────┐
│       │        │
Conv1D  Conv1D  Conv1D
(k=2)  (k=4)   (k=8)
│       │        │
SpatialDropout1D (each branch)
│       │        │
└───────┼────────┘
        │  Concatenate
        │
  Dense + Dropout
        │
  Output (5 classes)
```

![model](https://user-images.githubusercontent.com/83555471/189566815-e7e1a10f-3de6-4066-af4a-27eaf97e0a4a.png)

**Key design decisions:**
- **SpatialDropout1D** — prevents co-adaptation of correlated traffic features along entire feature maps
- **Parallel multi-scale convolutions** — simultaneous kernel sizes capture local vs. global traffic patterns
- **Balanced class weights** — handles severe under-representation of U2R and R2L attack classes

---

## Dataset — NSL-KDD

The [NSL-KDD dataset](https://www.unb.ca/csd/security/nsl.html) improves over the original KDD Cup 1999 dataset by removing duplicate records and balancing difficult instances.

| Class | Description |
|---|---|
| `Normal` | Legitimate traffic |
| `DoS` | Denial of Service (e.g., SYN flood, Smurf) |
| `Probe` | Surveillance/scanning (e.g., Nmap, Satan) |
| `R2L` | Remote-to-Local unauthorized access |
| `U2R` | User-to-Root privilege escalation |

---

## Results

| Metric | Value |
|---|---|
| **Multi-class Accuracy** | **93.2%** |
| **F1-Score** | **93.1%** |
| Architecture | Multi-Scale Parallel 1D-CNN |
| Regularization | SpatialDropout1D |
| Training Strategy | Balanced class weights |

SpatialDropout1D outperforms standard Dropout by ~1.2% accuracy due to the high feature correlation in network traffic data.

### Binary Confusion Matrix

![binary_confusion_matrix](https://user-images.githubusercontent.com/83555471/189566865-06b18d51-e815-424a-8bb5-6987ba6ab442.png)

### Categorical Confusion Matrix

![categorical_confusion_matrix](https://user-images.githubusercontent.com/83555471/189566875-628148ac-90f6-43d6-9335-80d081784e3c.png)

---

## Installation

```bash
git clone https://github.com/tamer017/Anomaly-baised-Intrusion-detection-system-CNN1D.git
cd Anomaly-baised-Intrusion-detection-system-CNN1D
pip install tensorflow pandas scikit-learn numpy matplotlib
```

---

## Skills & Concepts

`Deep Learning` `1D-CNN` `Network Security` `Feature Engineering` `Class Imbalance` `TensorFlow/Keras` `NSL-KDD` `Anomaly Detection` `SpatialDropout` `Multi-Scale Feature Fusion`

---

## Author

**Ahmed Tamer Assy** — [GitHub](https://github.com/tamer017) | Machine Learning Researcher @ Volkswagen AG
