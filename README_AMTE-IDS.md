<h1 align="center">AMTE-IDS: Adaptive Multi-View Transformer Ensemble<br/>for Network Intrusion Detection</h1>

<p align="center">
  <a href="https://doi.org/10.xxxx/xxxxxx">
    <img src="https://img.shields.io/badge/IEEE-Paper-blue?style=flat-square&logo=ieee" alt="IEEE Paper"/>
  </a>
  <a href="https://github.com/mehedi93hasan/AMTE-IDS/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License"/>
  </a>
  <a href="https://www.python.org/downloads/">
    <img src="https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square&logo=python&logoColor=white" alt="Python 3.9+"/>
  </a>
  <a href="https://pytorch.org/">
    <img src="https://img.shields.io/badge/PyTorch-2.0%2B-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" alt="PyTorch"/>
  </a>
  <img src="https://img.shields.io/github/stars/mehedi93hasan/AMTE-IDS?style=flat-square&color=yellow" alt="GitHub Stars"/>
</p>

<p align="center">
  <b>Md Mehedi Hasan, Rafiqul Islam, Quazi Mamun, Md Zahidul Islam, Junbin Gao</b><br/>
  Connectivity Innovation Network (CIN), Charles Sturt University, Albury, NSW, Australia
</p>

---

## Overview

<p align="center">
  <img src="https://github.com/user-attachments/assets/1f04110a-1ca3-4a83-bf24-be649ebc144b" alt="AMTE-IDS Framework Architecture" width="900"/>
</p>

<p align="center"><i>Figure 1. Complete AMTE-IDS framework architecture showing the four integrated processing stages: data preprocessing, Advanced Data Distribution Balancing (ADDB) via Multi-Modal WGAN-GP, Multi-View Feature Learning (MVFL) with three specialised transformer views, and Dynamic Ensemble Detection (DED) with adaptive router network.</i></p>

Minority-class network attack categories — such as U2R, R2L, and Infiltration — are chronically underrepresented in standard intrusion detection benchmarks, causing conventional detectors to exhibit severely degraded sensitivity on precisely the attack types that carry the highest operational risk. **AMTE-IDS** addresses this problem through a four-stage pipeline that jointly resolves class imbalance, multi-perspective feature extraction, and adaptive classification.

The framework makes three principal contributions:

1. **Advanced Data Distribution Balancing (ADDB)** — a Multi-Modal WGAN-GP generator that produces high-fidelity synthetic minority samples, eliminating the mode-collapse artifacts that afflict standard conditional GANs on network traffic data.
2. **Multi-View Feature Learning (MVFL)** — three specialised transformer encoders operating simultaneously over global statistical, temporal sequential, and protocol-specific feature spaces, capturing complementary attack signatures that any single view misses.
3. **Dynamic Ensemble Detection (DED)** — a learned router network that selects and weights classifiers per input at inference time, rather than applying a fixed combination rule.

---

## Table of Contents

- [Key Results](#key-results)
- [Architecture Details](#architecture-details)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Dataset Preparation](#dataset-preparation)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Reproducing Paper Results](#reproducing-paper-results)
- [Citation](#citation)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Key Results

### F1-Score Improvements for Minority Attack Classes

| Dataset | Attack Class | Baseline F1 | AMTE-IDS F1 | Improvement |
|:---|:---|:---:|:---:|:---:|
| NSL-KDD | U2R | — | — | **+152.1%** |
| UNSW-NB15 | Worms | — | — | **+80.9%** |
| CIC-IDS2017 | Infiltration | — | — | **+39.7%** |

> Improvements are measured against the strongest reported prior baseline on each dataset. AMTE-IDS simultaneously maintains high detection rates for common majority-class attacks (DoS, Probe, Normal).

### Cross-Dataset Performance Summary

| Dataset | Accuracy | F1 (Weighted) | Key Minority Gain |
|:---|:---:|:---:|:---:|
| NSL-KDD | — | — | +152.1% (U2R) |
| UNSW-NB15 | — | — | +80.9% (Worms) |
| CIC-IDS2017 | — | — | +39.7% (Infiltration) |

---

## Architecture Details

AMTE-IDS processes each input through four tightly coupled stages.

### Stage 1 — Data Preprocessing

Context-aware cleaning, feature engineering (flow statistics, protocol indicators, temporal deltas), and per-feature normalisation. Missing values and infinite flow-rate entries arising from zero-duration connections are handled explicitly before any learning stage.

### Stage 2 — Advanced Data Distribution Balancing (ADDB)

A Multi-Modal WGAN-GP generates synthetic minority samples conditioned on attack category. The Wasserstein critic with gradient penalty (λ=10) prevents mode collapse and enforces Lipschitz continuity, producing synthetic traffic with realistic protocol-level feature distributions. The generator architecture uses residual blocks with spectral normalisation; training alternates five critic updates per generator step.

### Stage 3 — Multi-View Feature Learning (MVFL)

Three parallel transformer encoders extract complementary representations from the balanced dataset:

| View | Input Space | Captures |
|:---|:---|:---|
| **Global View** | Full feature vector | Statistical co-occurrence patterns across all 80+ features |
| **Temporal View** | Time-ordered flow sequences | Sequential dependencies and burst dynamics |
| **Protocol View** | Protocol-specific feature subsets | Layer-specific attack signatures (TCP/UDP/ICMP) |

View representations are concatenated and passed through a cross-view attention layer that learns inter-view dependencies before producing the final embedding.

### Stage 4 — Dynamic Ensemble Detection (DED)

A lightweight router network (2-layer MLP) takes the MVFL embedding as input and outputs per-classifier softmax weights at inference time. The weighted combination of classifiers (Random Forest, XGBoost, MLP) is therefore input-adaptive rather than globally fixed — routing complex or ambiguous samples toward the classifier with the strongest demonstrated specialisation on similar training examples.

---

## Repository Structure

```
AMTE-IDS/
├── preprocessing/
│   ├── __init__.py
│   └── preprocess.py           # Context-aware cleaning, normalisation, encoding
├── algorithm_1/
│   ├── __init__.py
│   └── addb.py                 # Multi-Modal WGAN-GP data balancing (Algorithm 1)
├── algorithm_2/
│   ├── __init__.py
│   └── mvfl.py                 # Multi-View Feature Learning — three transformer views (Algorithm 2)
├── algorithm_3/
│   ├── __init__.py
│   └── ded.py                  # Dynamic Ensemble Detection with router network (Algorithm 3)
├── models/
│   └── ensemble.py             # Full AMTE-IDS pipeline
├── configs/
│   ├── nslkdd.yaml             # NSL-KDD hyperparameters
│   ├── unswnb15.yaml           # UNSW-NB15 hyperparameters
│   └── cicids2017.yaml         # CIC-IDS2017 hyperparameters
├── utils/
│   └── metrics.py              # Per-class F1, macro/weighted averages, confusion matrix
├── experiments/                # Auto-generated logs and checkpoints
├── figures/                    # Architecture diagrams
├── train.py                    # End-to-end training entry-point
├── evaluate.py                 # Evaluation and per-class reporting
├── requirements.txt
├── LICENSE
└── README.md
```

---

## Installation

**Requirements:** Python ≥ 3.9 · PyTorch ≥ 2.0 · CUDA 11.8+ *(optional)*

```bash
# 1. Clone the repository
git clone https://github.com/mehedi93hasan/AMTE-IDS.git
cd AMTE-IDS

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate          # Linux / macOS
# venv\Scripts\activate           # Windows

# 3. Install all dependencies
pip install -r requirements.txt
```

**Core dependencies:**

```
torch>=2.0.0
scikit-learn>=1.3.0
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
tqdm>=4.65.0
```

---

## Dataset Preparation

AMTE-IDS is evaluated on three publicly available network intrusion detection benchmarks.

### NSL-KDD

Download from the [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/nsl.html). Place `KDDTrain+.txt` and `KDDTest+.txt` in `data/nslkdd/`.

### UNSW-NB15

Download from the [UNSW Research Data Repository](https://research.unsw.edu.au/projects/unsw-nb15-dataset). Place the CSV partition files in `data/unswnb15/`.

### CIC-IDS2017

Download from the [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/ids-2017.html). Place the CICFlowMeter-generated CSVs in `data/cicids2017/`.

> **Note on CIC-IDS2017 column names:** CICFlowMeter CSVs use non-standard column names including double-space variants (e.g., `" Flow Duration"`). The preprocessing module handles these automatically.

---

## Quick Start

```python
import torch
from preprocessing.preprocess import preprocess_csv_data
from algorithm_1.addb import balance_dataset_from_csv
from algorithm_2.mvfl import train_mvfl_module, extract_mvfl_features
from algorithm_3.ded import train_dynamic_ensemble

# Stage 1: Preprocessing
X_train, y_train, X_test, y_test, preprocessor = preprocess_csv_data(
    "data/nslkdd/KDDTrain+.txt"
)

# Stage 2: WGAN-GP data balancing
X_balanced, y_balanced, X_test, y_test, scaler, encoder = balance_dataset_from_csv(
    "data/nslkdd/KDDTrain+.txt",
    epochs=500,
)

# Stage 3: Multi-View Feature Learning
mvfl_model, classifier = train_mvfl_module(
    "data/balanced_dataset.csv",
    epochs=100,
)
mvfl_features = extract_mvfl_features(mvfl_model, "data/balanced_dataset.csv")

# Stage 4: Dynamic Ensemble Detection
ensemble_model, results = train_dynamic_ensemble("data/mvfl_features.csv")

print(f"Accuracy : {results['accuracy']:.4f}")
print(f"F1-Score : {results['f1_weighted']:.4f}")
```

---

## Usage

### End-to-end training

```bash
# NSL-KDD
python train.py --dataset nslkdd --data_root data/nslkdd --epochs 500 --seed 42

# UNSW-NB15
python train.py --dataset unswnb15 --data_root data/unswnb15 --epochs 500 --seed 42

# CIC-IDS2017
python train.py --dataset cicids2017 --data_root data/cicids2017 --epochs 500 --seed 42
```

### Evaluation with per-class reporting

```bash
python evaluate.py \
    --dataset    nslkdd \
    --data_root  data/nslkdd \
    --checkpoint checkpoints/best_nslkdd.pt \
    --output     results/nslkdd_test.json
```

---

## Reproducing Paper Results

To reproduce the minority-class F1 gains reported in the paper, run five seeds and report mean ± std:

```bash
for SEED in 42 43 44 45 46; do
    python train.py --dataset nslkdd --data_root data/nslkdd \
                    --seed $SEED --save_dir checkpoints/seed_$SEED
    python evaluate.py --dataset nslkdd --data_root data/nslkdd \
                       --checkpoint checkpoints/seed_$SEED/best_nslkdd.pt \
                       --output results/nslkdd_seed_$SEED.json
done
```

Repeat for `--dataset unswnb15` and `--dataset cicids2017`.

---

## Citation

If AMTE-IDS contributes to your research, please cite:

```bibtex
@article{hasan2025amteid,
  title   = {Adaptive Multi-View Transformer Ensemble for Intrusion Detection:
             Addressing Data Imbalance and Enhancing Attack Classification},
  author  = {Hasan, Md Mehedi and Islam, Rafiqul and Mamun, Quazi
             and Islam, Md Zahidul and Gao, Junbin},
  journal = {[Journal Name]},
  year    = {2025},
  doi     = {10.xxxx/xxxxxx},
  note    = {Code: \url{https://github.com/mehedi93hasan/AMTE-IDS}}
}
```

---

## License

This project is released under the [MIT License](LICENSE).

---

## Acknowledgements

This work was conducted at the **Connectivity Innovation Network (CIN)**, Charles Sturt University, Albury, NSW, Australia.

---

<p align="center">
  For questions, please open a <a href="https://github.com/mehedi93hasan/AMTE-IDS/issues">GitHub Issue</a>.
</p>
