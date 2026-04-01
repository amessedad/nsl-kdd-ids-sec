# NSL-KDD Intrusion Detection System
### Anomaly Detection using Decision Trees and Neural Networks

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-latest-orange)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Overview

This project builds and evaluates machine learning models for network intrusion detection using the **NSL-KDD dataset** — the standard benchmark for anomaly-based IDS research, and an improved version of the widely-used KDD Cup 99 dataset.

The notebook implements a full ML pipeline:
- Exploratory data analysis and feature engineering
- Label encoding (fit on train, transform on test — correct approach)
- Min-max feature scaling
- Decision Tree classifier with GridSearchCV hyperparameter tuning
- Multi-layer perceptron (MLP) neural network classifier
- Cross-validation learning curve analysis

**Primary research question:** How accurately can ML models distinguish normal network traffic from anomalous (attack) behaviour, and which features are most informative for that classification?

---

## Dataset: NSL-KDD

The NSL-KDD dataset is maintained by the **Canadian Institute for Cybersecurity** at the University of New Brunswick. It addresses known problems in the original KDD Cup 99 dataset including redundant records and unbalanced class distributions.

| Split | Records | Features |
|-------|---------|----------|
| Training set | 125,972 | 41 + labels |
| Test set | 22,543 | 41 + labels |

**Attack categories:** DoS, Probe, R2L, U2R, plus Normal traffic

**Download:** https://www.unb.ca/cic/datasets/nsl.html

> Place `KDDTrain+.txt` and `KDDTest+.txt` in the project root directory before running the notebook.

---

## Project Structure

```
nsl_kdd_ids_analysis.ipynb   # Main analysis notebook
KDDTrain+.txt                # Training data (download separately)
KDDTest+.txt                 # Test data (download separately)
README.md                    # This file
```

---

## Installation & Requirements

**Python version:** 3.8+

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy
```

---

## Notebook Structure

### 1. Introduction
Overview of IDS, anomaly detection vs. signature-based detection, and the NSL-KDD dataset.

### 2. Data Preparation & Exploratory Analysis
- Load and merge training and test sets
- Null value and duplicate row checks
- Attack type distribution analysis (train vs. test)
- Normal vs. anomaly class balance (pie charts)
- Service, flag, and protocol feature analysis
- Pearson correlation between train/test feature distributions
- Information gain calculation for categorical features
- Label encoding: **fit on train only, transform on test** (prevents data leakage)
- Min-max feature scaling

### 3. Decision Tree Classifier
- Trained with entropy criterion (information gain splitting)
- Tree visualisation to depth 3
- Feature importance analysis
- GridSearchCV hyperparameter tuning (5-fold cross-validation)
- Cross-validation learning curve (depth 1–20)

### 4. Neural Network Classifier (MLPClassifier)
- Multi-layer perceptron with ReLU activation, Adam optimiser
- Trained on NSL-KDD features after full preprocessing pipeline
- GridSearchCV demonstration on Iris dataset (separate variables — does not affect NSL-KDD results)

### 5. Results Summary

| Model | Test Accuracy |
|-------|--------------|
| Decision Tree (default, unconstrained) | 79.3% |
| Decision Tree (depth=15) | 80.9% |
| Decision Tree (depth=20, GridSearchCV tuned) | 81.7% |
| MLP Neural Network | 80.5% |

---

## Key Findings

- **Flag** and **protocol_type** are the most informative features for anomaly classification (information gain: 0.52 and 0.06 respectively)
- The test set has a higher proportion of anomalies (56.9%) vs. training (46.5%), creating a class distribution shift that affects generalisation
- Increasing decision tree depth from unconstrained to depth 20 improved accuracy from 79% to 82% — further depth showed diminishing returns on this dataset
- The MLP achieved comparable accuracy to the tuned decision tree (~80%), but with significantly higher training accuracy (99.7%), suggesting some overfitting
- Several attack types present in the test set are absent from the training set — a key generalisation challenge for real-world IDS deployment

---

## Limitations & Future Work

- NSL-KDD originates from 1999 network traffic and does not reflect modern attack vectors (application-layer attacks, encrypted traffic, AI-assisted intrusion)
- Label encoding imposes an ordinal relationship on categorical features (service, flag, protocol) that does not exist — one-hot encoding would be more appropriate for some model types
- Future extensions:
  - Random Forest and gradient boosting comparison
  - SMOTE oversampling to address class imbalance
  - Feature selection to reduce dimensionality
  - Evaluation on UNSW-NB15 or CIC-IDS-2017 for modern attack coverage
  - GridSearchCV applied directly to MLP on NSL-KDD (currently left as future work due to compute constraints)

---

## Context

This analysis was completed as part of the **CS502M Security Analytics with AI** module at the University of Aberdeen (MSc Cybersecurity, 2023–2024).

It forms part of a broader research interest in AI-assisted security tooling, which extended into independent work on LLM-based automated penetration testing:

> **Related project:** [AutoExploitGPT](https://github.com/amessedad/autoexploitGPT) — automated exploit generation pipeline using GPT-4o, RAG over CVE data, and agentic execution loops. Paper submitted to IEEE Open Journal of the Computer Society (Revise & Resubmit).

---

## License

MIT License — see [LICENSE](LICENSE) for details.
