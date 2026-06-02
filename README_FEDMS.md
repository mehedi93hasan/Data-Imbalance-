<h1 align="center">FEDMS: Ensemble-Based Adversarial Defense with<br/>Dynamic Model Selection for Intrusion Detection Systems</h1>

<p align="center">
  <a href="https://doi.org/10.xxxx/xxxxxx">
    <img src="https://img.shields.io/badge/IEEE-Paper-blue?style=flat-square&logo=ieee" alt="IEEE Paper"/>
  </a>
  <a href="https://github.com/mehedi93hasan/FEDMS/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License"/>
  </a>
  <a href="https://www.python.org/downloads/">
    <img src="https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square&logo=python&logoColor=white" alt="Python 3.9+"/>
  </a>
  <a href="https://www.tensorflow.org/">
    <img src="https://img.shields.io/badge/TensorFlow-2.13%2B-FF6F00?style=flat-square&logo=tensorflow&logoColor=white" alt="TensorFlow"/>
  </a>
  <img src="https://img.shields.io/github/stars/mehedi93hasan/FEDMS?style=flat-square&color=yellow" alt="GitHub Stars"/>
</p>

<p align="center">
  <b>Md Mehedi Hasan, Rafiqul Islam, Quazi Mamun, Md Zahidul Islam, Junbin Gao</b><br/>
  Connectivity Innovation Network (CIN), Charles Sturt University, Albury, NSW, Australia
</p>

---

## Overview

<p align="center">
  <img width="2563" height="1244" alt="FEDMS System Architecture" src="https://github.com/user-attachments/assets/a504ec05-3441-491a-b317-38f52a7a39d1"/>
</p>

<p align="center"><i>Figure 1. System architecture of FEDMS showing the three-tier hierarchical structure: nine heterogeneous detection models across three complementary categories, the multi-dimensional confidence scoring mechanism, and the dynamic model selection layer that routes each input to the optimal detector based on real-time threat assessment.</i></p>

<p align="center">
  <img width="1409" height="363" alt="FEDMS Real-Time Adaptation Framework" src="https://github.com/user-attachments/assets/474acac1-ab0f-477f-af8f-4ae29d8beadc"/>
</p>

<p align="center"><i>Figure 2. Real-time adaptation framework showing continuous system evolution through concept drift detection, threshold adaptation, model reweighting, and performance validation mechanisms.</i></p>

Adversarial machine learning attacks against network intrusion detection systems (NIDS) remain an open operational threat: adversarially perturbed traffic that fools a single-model detector is trivially crafted once the model is known or inferred. **FEDMS** addresses this vulnerability through a heterogeneous ensemble of nine detection models spanning three complementary architectural families, combined with a multi-dimensional confidence scoring mechanism that performs input-adaptive model selection at inference time — without incurring the latency of ensemble voting across all nine members.

The framework makes three principal contributions:

1. **Heterogeneous nine-model ensemble** spanning deep neural networks, gradient-boosted trees, and anomaly detectors — ensuring that adversarial perturbations effective against one model family are detected by another.
2. **Multi-dimensional confidence scoring** that evaluates prediction certainty, inter-model agreement, and input similarity to training manifolds simultaneously, producing a calibrated selection signal robust to distribution shift.
3. **Real-time adaptation** via concept drift detection and online threshold adjustment, enabling the system to self-update as network traffic characteristics evolve without full retraining.

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

### Performance Under Adversarial Attack

| Metric | Value | Baseline Comparison |
|:---|:---:|:---|
| Accuracy on clean traffic | **96.8%** | Competitive with single-model SOTA |
| Accuracy under strong adversarial attacks | **75.5%** | **+16.6 pp** over state-of-the-art |
| Average inference latency | **12.4 ms** | Suitable for enterprise real-time deployment |

> Results are measured under strong white-box adversarial attacks (PGD, C&W) applied to network feature vectors. The +16.6 pp adversarial accuracy gain over SOTA confirms that heterogeneous ensemble diversity provides non-recoverable robustness that single-architecture defences cannot replicate.

### Computational Profile

| Property | Value |
|:---|:---|
| Ensemble members | 9 heterogeneous models |
| Inference path | Single selected model per input (not full ensemble vote) |
| Average latency | 12.4 ms |
| Deployment target | Enterprise NIDS, edge gateway |

---

## Architecture Details

### Three-Tier Hierarchical Structure

FEDMS organises its nine detection models into three complementary tiers, each targeting a different aspect of the adversarial detection problem:

**Tier 1 — Deep Neural Detectors**  
Capture complex non-linear feature interactions and high-dimensional statistical patterns. Vulnerable to gradient-based adversarial perturbations in isolation, but contribute strong majority-class accuracy.

**Tier 2 — Gradient-Boosted Tree Detectors**  
Operate on discrete decision boundaries that are structurally resistant to gradient-based attacks. Provide a complementary hypothesis to Tier 1 on the same input.

**Tier 3 — Anomaly-Based Detectors**  
Trained on normal traffic only (one-class learning). Detect adversarially perturbed samples as out-of-distribution even when the perturbation successfully evades Tier 1 and Tier 2 classifiers.

### Multi-Dimensional Confidence Scoring

For each input *x*, the confidence scorer computes three independent signals:

| Signal | Description |
|:---|:---|
| **Prediction certainty** | Softmax entropy of the candidate model's output distribution |
| **Inter-model agreement** | Pairwise agreement rate across all nine models' hard predictions |
| **Manifold proximity** | Distance from *x* to the nearest training-set neighbours in embedding space |

These three signals are aggregated via a learned linear combination to produce the final selection score. The model with the highest score processes *x* for the final classification decision.

### Real-Time Adaptation Framework

The adaptation layer monitors four continuous signals:
- **Concept drift** — Page-Hinkley test on rolling accuracy windows
- **Threshold adaptation** — Bayesian update of per-class decision thresholds
- **Model reweighting** — exponential moving average of per-model recent performance
- **Performance validation** — held-out reference stream accuracy gating

When drift is detected, the system triggers partial retraining of the affected tier without interrupting inference on the remaining tiers.

---

## Repository Structure

```
FEDMS/
├── models/
│   ├── __init__.py
│   ├── tier1_deep.py           # Deep neural network detectors (Tier 1)
│   ├── tier2_trees.py          # Gradient-boosted tree detectors (Tier 2)
│   ├── tier3_anomaly.py        # One-class anomaly detectors (Tier 3)
│   └── ensemble.py             # Full nine-model ensemble
├── scoring/
│   ├── __init__.py
│   └── confidence.py           # Multi-dimensional confidence scorer
├── adaptation/
│   ├── __init__.py
│   └── drift.py                # Concept drift detection and threshold adaptation
├── data/
│   ├── __init__.py
│   └── preprocessing.py        # Feature engineering and normalisation
├── utils/
│   └── metrics.py              # Accuracy, adversarial robustness metrics
├── configs/
│   ├── nslkdd.yaml
│   ├── unswnb15.yaml
│   └── cicids2017.yaml
├── experiments/                # Auto-generated training logs
├── figures/                    # Architecture diagrams
├── train.py                    # Training entry-point
├── evaluate.py                 # Evaluation under clean and adversarial conditions
├── requirements.txt
├── LICENSE
└── README.md
```

---

## Installation

**Requirements:** Python ≥ 3.9 · TensorFlow 2.13 · CUDA 11.8+ *(optional, recommended)*  
32 GB RAM recommended for full nine-model ensemble training.

```bash
# 1. Clone the repository
git clone https://github.com/mehedi93hasan/FEDMS.git
cd FEDMS

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate          # Linux / macOS
# venv\Scripts\activate           # Windows

# 3. Install all dependencies
pip install -r requirements.txt
```

**Core dependencies:**

```
tensorflow==2.13.0
scikit-learn==1.3.0
xgboost>=1.7.0
pyod==1.1.0
numpy==1.24.3
pandas==2.0.3
matplotlib==3.7.2
tqdm>=4.65.0
joblib>=1.3.0
```

---

## Dataset Preparation

FEDMS is evaluated on three publicly available network intrusion detection benchmarks.

### NSL-KDD

Download from the [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/nsl.html). Place `KDDTrain+.txt` and `KDDTest+.txt` in `data/nslkdd/`.

### UNSW-NB15

Download from the [UNSW Research Data Repository](https://research.unsw.edu.au/projects/unsw-nb15-dataset). Place the CSV partition files in `data/unswnb15/`.

### CIC-IDS2017

Download from the [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/ids-2017.html). Place the CICFlowMeter-generated CSVs in `data/cicids2017/`.

---

## Quick Start

```python
from models.ensemble import FEDMSEnsemble
from scoring.confidence import ConfidenceScorer
import numpy as np

# Load a pre-trained ensemble
ensemble = FEDMSEnsemble.load("checkpoints/best_nslkdd.pt")
scorer   = ConfidenceScorer.load("checkpoints/scorer_nslkdd.pt")

# Single-sample inference
x = np.random.randn(1, 41)           # [1, num_features]
selected_model = scorer.select(ensemble, x)
prediction     = selected_model.predict(x)

print(f"Prediction       : {prediction}")
print(f"Selected model   : {selected_model.name}")
print(f"Confidence score : {scorer.last_score:.4f}")
```

---

## Usage

### End-to-end training

```bash
# NSL-KDD
python train.py --dataset nslkdd --data_root data/nslkdd \
                --epochs 100 --seed 42

# UNSW-NB15
python train.py --dataset unswnb15 --data_root data/unswnb15 \
                --epochs 100 --seed 42

# CIC-IDS2017
python train.py --dataset cicids2017 --data_root data/cicids2017 \
                --epochs 100 --seed 42
```

### Evaluation under adversarial attack

```bash
python evaluate.py \
    --dataset    nslkdd \
    --data_root  data/nslkdd \
    --checkpoint checkpoints/best_nslkdd.pt \
    --attack     pgd \
    --output     results/nslkdd_adversarial.json
```

Supported `--attack` options: `none` (clean), `pgd`, `cw`, `fgsm`.

---

## Reproducing Paper Results

To reproduce the adversarial robustness figures reported in the paper, run five seeds and report mean ± std:

```bash
for SEED in 42 43 44 45 46; do
    python train.py --dataset nslkdd --data_root data/nslkdd \
                    --seed $SEED --save_dir checkpoints/seed_$SEED
    python evaluate.py --dataset nslkdd --data_root data/nslkdd \
                       --checkpoint checkpoints/seed_$SEED/best_nslkdd.pt \
                       --attack pgd \
                       --output results/nslkdd_pgd_seed_$SEED.json
done
```

Repeat for `--dataset unswnb15`, `--dataset cicids2017`, and `--attack cw`.

---

## Citation

If FEDMS contributes to your research, please cite:

```bibtex
@article{HASAN2026101970,
title = {Ensemble-based adversarial defense with dynamic model selection for intrusion detection systems},
journal = {Internet of Things},
volume = {38},
pages = {101970},
year = {2026},
issn = {2542-6605},
doi = {https://doi.org/10.1016/j.iot.2026.101970},
url = {https://www.sciencedirect.com/science/article/pii/S2542660526001009},
author = {Md Mehedi Hasan and Rafiqul Islam and Quazi Mamun and Md Zahidul Islam and Junbin Gao},
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
  For questions, please open a <a href="https://github.com/mehedi93hasan/FEDMS/issues">GitHub Issue</a>.
</p>
