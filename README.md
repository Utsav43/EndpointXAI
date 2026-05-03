# EndpointXAI: Explainable Threat Detection with MITRE ATT&CK

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/status-research-green.svg)]()

> An analyst-centric machine learning framework that fuses SHAP-based explainability with MITRE ATT&CK enrichment to detect endpoint threats with human-interpretable explanations.

---

## 🎯 Key Highlights

- ✅ **98.02% classification accuracy** across three MITRE ATT&CK tactics
- ✅ **Systematic data leakage detection** identifying and removing seven leakage-prone features
- ✅ **SHAP + ATT&CK fusion** generating analyst-ready alert explanations
- ✅ **97,617 labeled events** from the OTRF Mordor dataset
- ✅ **Fully reproducible pipeline** released as open-source artifacts

---

## 🔥 Why EndpointXAI?

Modern Security Operations Centers (SOCs) face a critical problem: machine learning detection systems generate thousands of alerts per analyst per shift, but provide no explanation for *why* something is suspicious. This leads to:

- 🚨 **Alert fatigue** from unexplained false positives
- 🤖 **Analyst distrust** of black-box AI decisions
- 🧩 **Semantic gap** between raw telemetry and actionable threat context

EndpointXAI addresses all three by combining:
1. **High-accuracy ML detection** (XGBoost on endpoint telemetry)
2. **Feature-level explainability** (SHAP TreeExplainer)
3. **Operational context** (MITRE ATT&CK tactic & technique mapping)

The result: alerts that include tactic identification, likely techniques, AI reasoning contributions, and recommended analyst actions.

---

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- 8 GB RAM minimum
- 5 GB free disk space (for dataset)

### Installation

```bash
# Clone the repository
git clone https://github.com/Utsav43/EndpointXAI.git
cd EndpointXAI

# Install dependencies
pip install -r requirements.txt
```

### Download Dataset

```bash
# Download OTRF Mordor / Security-Datasets
git clone https://github.com/OTRF/Security-Datasets.git data/Security-Datasets
```

### Run The Pipeline

Open notebooks in order:

1. `data/01_explore_data.ipynb` — Load and explore Mordor dataset
2. `data/02b_better_features.ipynb` — Feature engineering
3. `data/03_train_model.ipynb` — Train XGBoost & Random Forest
4. `data/04_explain_model.ipynb` — Generate SHAP explanations
5. `data/05_attack_mapping.ipynb` — ATT&CK enrichment

---

## 🔬 Methodology

### 1. Data Collection
We use the OTRF Mordor dataset, containing simulated adversary activity from controlled red-team engagements. After deduplication and filtering, our dataset contains 97,617 events across three ATT&CK tactics:
- **Defense Evasion** (TA0005)
- **Lateral Movement** (TA0008)
- **Persistence** (TA0003)

### 2. Feature Engineering
We extract 17 candidate features from raw telemetry, then apply systematic leakage detection to remove features that map exclusively to single classes (lab artifacts):

| Feature | Leakage Risk | Status |
|---------|-------------|--------|
| Hostname | 100% | ❌ Removed |
| Message | 100% | ❌ Removed |
| ProcessId | 99.3% | ❌ Removed |
| ThreadID | 94.9% | ❌ Removed |
| EventID | 67.7% | ✅ Kept |
| Channel | 62.5% | ✅ Kept |
| Task | 56.2% | ✅ Kept |

Final feature set: 10 leakage-free behavioral features.

### 3. Model Training
- **XGBoost** (n_estimators=200, max_depth=8, lr=0.1)
- **Random Forest** (n_estimators=100, max_depth=15) — baseline
- 80/20 stratified train-test split
- Final accuracy: **98.02%**

### 4. SHAP Explainability
Using `shap.TreeExplainer` on XGBoost, we compute feature attributions and analyze:
- Global importance across all classes
- Per-class behavioral patterns
- Single-alert waterfall explanations

### 5. ATT&CK Enrichment
We construct two mappings:
- Tactic → technique relationships from MITRE ATT&CK matrix
- Windows EventID → ATT&CK technique correlations (20 EventIDs)

---

## 📜 Citation

If you use this work in your research, please cite:

```bibtex
@article{belbase2026endpointxai,
  title={EndpointXAI: An Explainable Machine Learning Framework for
         Endpoint Threat Detection with MITRE ATT&CK Integration},
  author={Belbase, Utsav},
  year={2026},
  note={Manuscript in preparation},
  url={https://github.com/Utsav43/EndpointXAI}
}
```

---

## 📧 Contact

**Utsav Belbase**
Department of Computer Science and Engineering Technology
University of Houston-Downtown

GitHub: [@Utsav43](https://github.com/Utsav43)

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **OTRF (Open Threat Research Forge)** for the Mordor dataset
- **MITRE Corporation** for the ATT&CK framework
- **SHAP team** for the explainability library
- The broader cybersecurity research community

---

⭐ **If you find this work useful, please consider starring the repository!**