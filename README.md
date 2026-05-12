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

<h1><b>Overview</b></h1>
FEDMS is an innovative adversarial defense framework designed specifically for network intrusion detection systems (NIDS). 
Our approach integrates nine heterogeneous detection models across three complementary categories with a sophisticated 
multi-dimensional confidence scoring mechanism that enables dynamic model selection based on real-time input characteristics and threat assessment.

<h1><b>Key Achievements</b></h1>

-96.8% accuracy on clean network traffic data

-75.5% average accuracy under strong adversarial attacks (16.6% improvement over state-of-the-art)

-12.4ms average processing latency suitable for enterprise deployment

-Real-time adaptation capabilities for evolving threat landscapes

<h1><b>System Architecture</b></h1> 
Main Framework Architecture
<div align="center">
  <img width="2563" height="1244" alt="fig 1_1 v8" src="https://github.com/user-attachments/assets/a504ec05-3441-491a-b317-38f52a7a39d1" />
  <br>
  <em>Figure 1: System architecture of our proposed adversarial defense framework showing the three-tier hierarchical structure integrating heterogeneous ensemble components, multi-dimensional confidence scoring, and dynamic selection mechanisms.</em>
</div>


<h1><b>Real-time Adaptation Framework</b></h1>  
<div align="center">
  <img width="1409" height="363" alt="fig 1_2 v3" src="https://github.com/user-attachments/assets/474acac1-ab0f-477f-af8f-4ae29d8beadc" />
  <br>
  <em>Figure 2: Real-time adaptation framework showing continuous system evolution through concept drift detection, threshold adaptation, model reweighting, and performance validation mechanisms.</em>
</div>

<h1><b>Installation</b></h1>  

<h2><b>Prerequisites </b></h2>  
 
Python 3.9.16+

CUDA 11.8+ (for GPU acceleration)

32GB RAM recommended for optimal performance

<h2><b>Quick Installation</b></h2> 

git clone https://github.com/mehedi93hasan/FEDMS.git

cd FEDMS

<h2><b>Quick Dependencies</b></h2> 
<br>

pip install tensorflow==2.13.0

pip install scikit-learn==1.3.0

pip install xgboost

pip install pyod==1.1.0

pip install numpy==1.24.3

pip install pandas==2.0.3

pip install matplotlib==3.7.2

pip install tqdm

pip install joblib

</br>

<h1><b>Citation</b></h1>  
bibtex@article{hasan2025amte, title={Ensemble-Based Adversarial Defense with Dynamic Model Selection for Intrusion Detection Systems}, author={Hasan, Md Mehedi and Islam, Rafiqul and Mamun, Quazi and Islam, Md Zahidul and Gao, Junbin}, journal={Research Article}, year={2025} }

