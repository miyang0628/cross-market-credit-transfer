# Before the Data Arrive: A Pre-Entry Transferability Framework for Alternative Credit Scoring in Emerging Economies

> **A Statistical Similarity Framework with Evidence from Korea and Indonesia**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.x](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Status: Paper Submit](https://img.shields.io/badge/Status-Paper%20Under%20Review-orange.svg)]()

---

## 📌 Overview

This repository contains the full analysis code and publicly releasable data for the paper:

> **Cross-Market Transferability of Alternative Credit Scoring Variables: A Statistical Similarity Framework with Evidence from Korea and Indonesia**
>
> *Status: Paper Submit*

### Key Contribution

We propose the **Cross-Market Variable Transferability Score (CMVTS)**, a pre-transfer quantitative framework that assesses whether alternative credit scoring variables developed in a source market can be deployed in a target market — **before any target-market data are collected**.

$$\text{CMVTS} = w_1 \cdot (1 - \overline{JSD}) + w_2 \cdot |\rho_s| + w_3 \cdot CosSim$$

| Component | Description | Weight |
|-----------|-------------|--------|
| $1 - \overline{JSD}$ | Distribution similarity (WoE bin distributions) | 0.4 |
| $\|\rho_s\|$ | Rank-order alignment (Spearman correlation over macro indicators) | 0.3 |
| $CosSim$ | Structural alignment (cosine similarity over macro statistics vector) | 0.3 |

**Korea → Indonesia result: CMVTS = 0.9225–0.9338 (HIGH) — robust across 47 sensitivity scenarios.**

---

## 🗂️ Repository Structure

```
cross-market-credit-transfer/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── indonesia_korea_statistics.csv   ← Statista 34 macro indicators (public)
│   └── aihub_raw/                       ← .gitignore (AI Hub, not public)
│
├── notebooks/
│   ├── 01_data_preprocessing_y_label.ipynb          ✅
│   ├── 02_iv_analysis_variable_selection.ipynb      ✅
│   ├── 03_korea_scorecard_model_final.ipynb         ✅
│   ├── 04_cmvts_calculation.ipynb                   ✅
│   ├── 04b_telecom_jsd_refinement.ipynb             ✅
│   ├── 04c_descriptive_statistics.ipynb             ✅
│   ├── 05_indonesia_scorecard_draft.ipynb           ✅
│   ├── 06_sensitivity_analysis.ipynb                ✅
│   └── 07_paper_figures_tables.ipynb                ✅
│
├── src/
│   ├── preprocessing.py
│   ├── cmvts.py
│   └── scorecard.py
│
└── outputs/
    ├── figures/                         ← Fig1–Fig5 (PDF + PNG, 300 dpi)
    └── tables/                          ← Table1–6 + TableA–E (CSV)
```

---

## 📊 Key Results

| Metric | Value |
|--------|-------|
| CMVTS (baseline) | **0.9338** → HIGH |
| CMVTS (refined, empirical prior) | **0.9225** → HIGH |
| Sensitivity: 47 scenarios ≥ 0.80 | **100%** |
| Korea scorecard AUC (test) | 0.9791 |
| Korea scorecard KS (test) | 0.8838 |
| Transfer variables (HIGH tier) | 9 of 64 |
| Indonesia approval rate @ 550pt | 92.2% |
| Indonesia approved bad rate | 0.024% |
| Lift | 262.8× |

### CMVTS Transfer Tiers

| Tier | Threshold | Variables | Recommendation |
|------|-----------|-----------|----------------|
| HIGH | ≥ 0.80 | 9 | Direct transfer |
| MEDIUM | 0.60–0.80 | 49 | Partial adjustment |
| LOW | < 0.60 | 6 | Local redevelopment |

---

## 🗃️ Data

### Public Data (included in this repo)
- **`data/indonesia_korea_statistics.csv`** — 34 macro-economic indicators comparing South Korea and Indonesia, compiled from Statista (e-commerce, banking, travel, consumer behavior sectors, 2020–2022).

### Restricted Data (not included)
- **AI Hub Personal CB dataset** (dataSetSn=71792, N=3,129,036): Synthetic credit bureau records. Available at [AI Hub](https://aihub.or.kr) upon application.
- **AI Hub Telecom-Card CB dataset** (N=2,640,000): 8 quarterly files, 2021Q1–2022Q4. Same access route.

> Both datasets are synthetic records generated under the 2022 FSC D-Testbed program by Obzen Inc. They are not publicly downloadable without registration.

---

## ⚙️ Setup

```bash
git clone https://github.com/[username]/cross-market-credit-transfer.git
cd cross-market-credit-transfer
pip install -r requirements.txt
```

### requirements.txt

```
pandas>=1.5.0
numpy>=1.23.0
scikit-learn>=1.1.0
scorecardpy>=0.1.9
scipy>=1.9.0
matplotlib>=3.6.0
seaborn>=0.12.0
jupyter>=1.0.0
```

---

## 🚀 Reproducing the Analysis

Run notebooks in order:

```bash
# 1. Data preprocessing & Y-label definition
jupyter notebook notebooks/01_data_preprocessing_y_label.ipynb

# 2. IV analysis & variable selection (64 final variables)
jupyter notebook notebooks/02_iv_analysis_variable_selection.ipynb

# 3. Korea scorecard model (AUC=0.979, KS=0.884, PSI=0.097)
jupyter notebook notebooks/03_korea_scorecard_model_final.ipynb

# 4. CMVTS calculation (baseline: 0.9338)
jupyter notebook notebooks/04_cmvts_calculation.ipynb

# 4b. JSD refinement with empirical telecom distributions (refined: 0.9225)
jupyter notebook notebooks/04b_telecom_jsd_refinement.ipynb

# 4c. Descriptive statistics (Table A–E)
jupyter notebook notebooks/04c_descriptive_statistics.ipynb

# 5. Indonesia scorecard draft (9 HIGH-tier variables, threshold=550)
jupyter notebook notebooks/05_indonesia_scorecard_draft.ipynb

# 6. Sensitivity analysis (47 scenarios, 100% ≥ 0.80)
jupyter notebook notebooks/06_sensitivity_analysis.ipynb

# 7. Paper figures & tables (Fig1–5, Table1–6)
jupyter notebook notebooks/07_paper_figures_tables.ipynb
```

> ⚠️ Notebooks 01–07 require the AI Hub datasets. If you do not have access, the public macro indicator data (`data/indonesia_korea_statistics.csv`) can be used to reproduce the CosSim component of CMVTS independently.

---

## 📐 CMVTS Framework

```
Source Market Data          Public Macro Statistics
(Korea CB, N=3.1M)          (Statista, 34 indicators)
        │                           │
        ▼                           ▼
 [Scorecard Development]    [CosSim Computation]
  WoE bins, IV, LR           KR vs ID vectors
        │                           │
        ▼                           │
 [C1: 1 - JSD_mean]                 │
  Bin distribution                  │
  similarity                        │
        │                           │
        ▼                           │
 [C2: |Spearman ρ|]                 │
  Rank-order alignment              │
  across macro indicators ──────────┘
        │
        ▼
 CMVTS = 0.4·C1 + 0.3·C2 + 0.3·C3
        │
        ▼
 ┌──────────────────────────┐
 │  HIGH  (≥0.80): Transfer │  ← Korea→Indonesia: 0.9225~0.9338
 │  MED   (0.60~0.80): Adj. │
 │  LOW   (<0.60):  Rebuild │
 └──────────────────────────┘
        │
        ▼
 [Indonesia Scorecard Draft]
  9 HIGH-tier variables
  Threshold: 550pt
  Approval: 92.2%, Lift: 262.8×
```

---

## ⚠️ Limitations

- Source-market scorecard estimated on **synthetic data**: AUC/KS metrics are inflated relative to real-data benchmarks (AUC 0.979 vs. 0.846). The scorecard serves as a transfer instrument, not a production model.
- Telecom-card file **primary key mismatch** prevents direct linkage with the personal CB dataset; telecom variables used for JSD refinement only.
- Indonesian proxy distributions in baseline JSD assume **uniform priors** parameterized by national penetration rates.
- Framework validated on a **single country pair** (Korea–Indonesia). Extension to other ASEAN pairs (Vietnam, Philippines) is a direction for future work.

---

## 📄 Citation

If you use this code or framework, please cite:

```bibtex

```

---

## 📬 Contact

---

## 📜 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

> The AI Hub datasets are subject to their own terms of use and are **not** covered by this license.
