# Preserving Privacy in Network Traffic Classification Using Machine Learning Techniques

**Author:** Toluwalase Emmanuel Oludipe  
**Institution:** University of Plymouth — Faculty of Science & Engineering  
**Degree:** MSc Cybersecurity  
**Supervisor:** Dr. Shaymaa Al-Juboori  
**Date:** October 2025  
**Word Count:** 22,438 (excluding front matter, references, and appendices)

---

## Overview

This dissertation investigates whether encrypted network traffic can be accurately classified for cybersecurity purposes using **supervised machine learning models trained solely on flow-level metadata** — without ever inspecting packet payloads or decrypting traffic. The research is framed around a core tension in modern cybersecurity: over 95% of web traffic is encrypted, yet more than 85% of cyberattacks are delivered through encrypted channels, making traditional inspection-based tools increasingly inadequate and legally problematic.

The work follows a **privacy-by-design, quantitative research** approach and sits at the intersection of network security, machine learning, and data protection regulation (GDPR, ISO 27001).

---

## Problem Statement

Traditional traffic analysis methods — port-based classification and Deep Packet Inspection (DPI) — are becoming obsolete:

- **Port-based classification** is trivially bypassed via port obfuscation and NAT.
- **DPI** is ineffective against modern encrypted protocols (TLS 1.3, QUIC) and raises serious GDPR compliance concerns due to content inspection.

Modern encrypted protocols like TLS 1.3 encrypt even handshake metadata (e.g., Server Name Indication via Encrypted Client Hello), and QUIC further obfuscates transport-layer information. The result: a **cybersecurity paradox** where the tools that protect user privacy also blind security defenders.

The study addresses a dual gap in the literature:
1. The lack of ML models for encrypted traffic classification that are experimentally validated **and** practically deployable.
2. The absence of a unified assessment framework combining performance optimisation, explainable AI, and regulatory compliance.

---

## Research Aim & Objectives

**Aim:** Design and evaluate a privacy-preserving framework for encrypted network traffic classification using ML, relying solely on metadata features — no payload inspection.

**Objectives (SMART):**

| # | Objective |
|---|-----------|
| 1 | Literature review of ≥30 recent sources on encrypted traffic classification methods, accuracy, and privacy gaps |
| 2 | Design a metadata-only, supervised ML classification architecture aligned with privacy-by-design |
| 3 | Collect/process benchmark datasets and train prototype classifiers |
| 4 | Evaluate classifiers using accuracy, precision, recall, F1-score, and cross-dataset generalisation |
| 5 | Conduct a privacy impact assessment including noise injection experiments and GDPR alignment analysis |

---

## Methodology

### Datasets
Two well-established public network traffic benchmark datasets were used:
- **CICIDS2017** — Canadian Institute for Cybersecurity Intrusion Detection dataset (2017)
- **CSE-CIC-IDS2018** — Extended IDS dataset with broader attack coverage (2018)

Both datasets were preprocessed under identical conditions for a fair comparison.

### Features Used (Metadata Only)
All features are **flow-level** and contain no payload content:
- Packet sizes and distributions
- Inter-arrival times and timing patterns
- Flow duration
- Forward/backward segment sizes
- Byte counts and direction indicators

### Models Evaluated
Three supervised classifiers were trained and compared:
1. **Logistic Regression** — interpretable linear baseline
2. **Random Forest** — ensemble method
3. **XGBoost** — gradient boosting

### Evaluation Protocol
- 5-fold cross-validation
- Binary and multi-class classification settings
- Identical preprocessing across both datasets
- Metrics: Accuracy, Precision, Recall, F1-score, AUC, Average Precision (AP)

### Privacy Simulation
- **Gaussian noise injection** experiments (varying σ) to quantify the privacy-utility tradeoff
- **SHAP (SHapley Additive exPlanations)** analysis for model interpretability

---

## Key Results

| Model | CICIDS2017 Accuracy | CSE-CIC-IDS2018 Accuracy |
|---|---|---|
| Logistic Regression | ~78% | Lower |
| Random Forest | ~97% | >99.9% |
| XGBoost | ~97% | >99.9% |

- **AUC ≈ 1.000, AP ≈ 1.000** observed in both datasets for ensemble methods, indicating near-perfect discrimination.
- Throughput: approximately **500 flows/second**, indicating near-real-time operability.

### Privacy-Utility Tradeoff
- Light Gaussian noise (σ ≤ 0.05) caused **less than 0.5% accuracy loss**, demonstrating that mild anonymisation preserves predictive power.
- Heavier noise introduces measurable degradation, confirming a quantifiable tradeoff curve.

### SHAP Interpretability
Key metadata features driving classification decisions:
- **Flow Duration**
- **Forward Segment Size**
- Packet timing patterns

SHAP analysis reduced the "black box" opacity of ensemble models, supporting audit-readiness and regulatory transparency.

---

## Privacy & Compliance Findings

The framework was assessed against:
- **GDPR** — particularly Article 25 (privacy by design and default) and Recital 78 (data minimisation, pseudonymisation)
- **ISO 27001** — information security management alignment

The metadata-only approach avoids personal identifiers and payload content, supporting compliance without sacrificing detection performance.

### Privacy-Preserving Techniques Compared

| Technique | Privacy Strength | Impact on Accuracy | Compliance |
|---|---|---|---|
| Anonymisation / IP masking | Moderate | Low–moderate loss | Partial |
| Feature suppression | Moderate | Variable | Moderate |
| Differential Privacy (DP) | High (formal) | Can be severe | Strong |
| Gaussian noise injection | Moderate | <0.5% at σ≤0.05 | Practical |

---

## Contributions

1. **First dual-dataset empirical validation** of metadata-only IDS models achieving near-perfect accuracy across two major benchmark datasets.
2. **Quantitative privacy-utility analysis** — demonstrating <0.5% accuracy loss under light anonymisation.
3. **SHAP-based explainability** integration, providing a decision-support mechanism suitable for regulatory audit trails.
4. A reusable **assessment framework** combining performance optimisation, explainability, and compliance — applicable to next-generation IDS systems.

---

## Limitations

- **Dataset realism:** CICIDS2017 and CSE-CIC-IDS2018 do not fully capture the heterogeneity, noise, and unpredictability of live production traffic.
- **Class imbalance:** Benign flows heavily outnumber malicious flows in both datasets, a known structural challenge.
- **Generalisation risk:** Cross-dataset validation mitigates but does not eliminate overfitting to known attack signatures.
- **Concept drift:** Without periodic retraining or online learning, model accuracy will degrade as traffic behaviours evolve.
- **Interpretability limits:** SHAP improves but does not fully resolve the black-box nature of large ensemble models in high-stakes deployments.
- **Scope:** No formal legal compliance audit was conducted; GDPR alignment is conceptual rather than certified.

---

## Future Work

- **Live traffic deployment** and real-world telemetry validation
- **Federated learning** to enable multi-organisation collaborative training without sharing raw data
- **Online/adaptive learning** to address concept drift in evolving attack patterns
- **Advanced XAI dashboards** beyond SHAP for richer operational interpretability
- **Resource-constrained environments** — evaluating model distillation and quantisation for edge deployment
- **Formal differential privacy integration** with domain-specific epsilon tuning

---

## Dissertation Structure

| Chapter | Content |
|---|---|
| 1 | Introduction — background, problem statement, objectives, scope |
| 2 | Literature Review — legacy methods, ML approaches, privacy regulations, datasets |
| 3 | Methodology — research design, feature engineering, model selection, privacy measures |
| 4 | Results — classifier performance metrics, feature importance, noise experiments |
| 5 | Discussion — interpretation of results, alignment with objectives |
| 6 | Conclusion — findings summary, limitations, recommendations, future directions |

---

## Relevance to Broader Goals

The research is positioned within the **UN Sustainable Development Goals**:
- **SDG 9** — Industry, Innovation and Infrastructure (responsible AI and secure digital infrastructure)
- **SDG 16** — Peace, Justice and Strong Institutions (ethical, trustworthy technology governance)

---
*Original Document attached*
