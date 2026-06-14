# Anomaly-Based Intrusion Detection System — 1D-CNN

> **Multi-class network traffic classifier achieving >98% accuracy on NSL-KDD using a multi-scale parallel 1D-CNN architecture.**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
[![Dataset](https://img.shields.io/badge/Dataset-NSL--KDD-green.svg)](https://www.unb.ca/csd/security/nsl.html)

---

## Overview

This project implements a high-performance **Network Intrusion Detection System (NIDS)** using a custom **Multi-Stage Feature Fusion 1D-CNN**. Unlike traditional signature-based IDS, this system learns anomaly patterns directly from raw network traffic features, making it robust against novel attack variants.

Three parallel convolutional branches with kernel sizes **2, 4, and 8** capture short, medium, and long temporal dependency patterns simultaneously. The outputs are concatenated and passed through dense layers for final classification.

---

## Architecture

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

**Key design decisions:**
- **SpatialDropout1D** instead of standard Dropout — prevents co-adaptation of correlated traffic features along entire feature maps rather than individual neurons
- **Parallel multi-scale convolutions** — simultaneous kernel sizes capture local vs. global traffic patterns
- **Balanced class weights** — handles severe under-representation of U2R and R2L attack classes
- **Custom LearningRateScheduler** — anneals from 0.5 → 0.001 after epoch 108

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

41 engineered features include: duration, protocol type, service, flag, byte counts, land, wrong fragments, urgent packets, and 19 traffic/host-based rate features.

---

## Results

| Metric | Value |
|---|---|
| **Multi-class Accuracy** | **>98%** |
| Architecture | Multi-Scale Parallel 1D-CNN |
| Regularization | SpatialDropout1D |
| Training Strategy | Balanced class weights + LR annealing |

SpatialDropout1D outperforms standard Dropout by ~1.2% accuracy due to the high feature correlation in network traffic data.

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
