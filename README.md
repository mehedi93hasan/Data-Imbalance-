# AMTE-IDS: Adaptive Multi-View Transformer Ensemble for Intrusion Detection

## Overview
AMTE-IDS is a comprehensive framework that addresses critical challenges in network intrusion detection through innovative integration of advanced data balancing, multi-perspective feature learning, and dynamic ensemble classification.

![Fig 2 v1](https://github.com/user-attachments/assets/1f04110a-1ca3-4a83-bf24-be649ebc144b)
Figure : Complete AMTE-IDS Framework Architecture

🏗️ Architecture Components
Our framework consists of four integrated modules:

📊 Data Preprocessing Module: Context-aware cleaning, feature engineering, and normalization
⚖️ Advanced Data Distribution Balancing (ADDB): Multi-Modal WGAN-GP for high-quality synthetic minority samples
👁️ Multi-View Feature Learning (MVFL): Three specialized transformer views (Global, Temporal, Protocol-specific)
🎯 Dynamic Ensemble Detection (DED): Adaptive classifier combination with router network

## 🔧 Quick Start
### 1. Install Dependencies
pip install torch scikit-learn pandas numpy seaborn matplotlib

### 2. Data Preprocessing
from preprocessing import preprocess_csv_data
X_train, y_train, X_test, y_test, preprocessor = preprocess_csv_data("your_dataset.csv")

### 3. Algorithm 1: Data Balancing
from algorithm_1 import balance_dataset_from_csv

X_balanced, y_balanced, X_test, y_test, scaler, encoder = balance_dataset_from_csv(
    "your_dataset.csv", epochs=500
)

### 4. Algorithm 2: Multi-View Feature Learning
from algorithm_2 import train_mvfl_module, extract_mvfl_features

mvfl_model, classifier = train_mvfl_module("balanced_dataset.csv", epochs=100)

mvfl_features = extract_mvfl_features(mvfl_model, "balanced_dataset.csv")

### 5. Algorithm 3: Dynamic Ensemble Detection
from algorithm_3 import train_dynamic_ensemble

ensemble_model, results = train_dynamic_ensemble("mvfl_features.csv")

print(f"Final Accuracy: {results['accuracy']:.4f}")
print(f"F1-Score: {results['f1_weighted']:.4f}")

## 📈 Key Results

152.1% F1-score improvement for U2R attacks (NSL-KDD)
80.9% improvement for Worms attacks (UNSW-NB15)
39.7% improvement for Infiltration attacks (CIC-IDS2017)
Maintains high detection rates for common attacks


## 🔬 Citation
bibtex@article{hasan2025amte,
  title={Adaptive Multi-View Transformer Ensemble for Intrusion Detection: Addressing Data Imbalance and Enhancing Attack Classification},
  author={Hasan, Md Mehedi and Islam, Rafiqul and Mamun, Quazi and Islam, Md Zahidul and Gao, Junbin},
  journal={Research Article},
  year={2025}
}

