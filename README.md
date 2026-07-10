# UEBA-insider-threat-detection
UEBA-based Insider Threat Detection using Machine Learning on CERT dataset

A machine learning project that detects insider threats — malicious employees who misuse their legitimate access — by learning normal user behaviour and flagging anomalies. Built on the **CERT Insider Threat Test Dataset r4.2** from Carnegie Mellon University.

**Master's in Cybersecurity | Team: Swapnil Gawade & Unnati Patil**

---

## 📋 Overview

Insider threats are among the hardest security risks to detect because the individuals already hold valid credentials and bypass traditional perimeter defences. This project builds a behaviour-analytics system that:

- Learns each employee's normal behavioural baseline from real activity logs
- Detects anomalies **without labels** using unsupervised models
- Classifies employees as malicious or normal using a supervised model
- Validates predictions against confirmed ground-truth

---

## 🗂️ Dataset

**CERT Insider Threat Test Dataset r4.2** — Carnegie Mellon University, Software Engineering Institute.

- 1,000 simulated employees over ~18 months
- 3.5 million+ raw activity events → 330,268 user-day records
- 191 confirmed insider records (ground truth)

> **Note:** The dataset is not included in this repository due to its size and licensing. It can be obtained from the official source:
> https://kilthub.cmu.edu/articles/dataset/Insider_Threat_Test_Dataset/12841247

Required files: `logon.csv`, `device.csv`, `file.csv`, `email.csv`, `insiders.csv`

---

## 🔧 Features

Seven behavioural features extracted directly from the real logs, plus seven delta features measuring deviation from each user's personal baseline (14 features total):

| Feature | Source | Description |
|---------|--------|-------------|
| `login_count` | logon.csv | Daily login frequency |
| `after_hours_logins` | logon.csv | Logins before 8am / after 6pm |
| `unique_systems` | logon.csv | Distinct computers accessed |
| `files_copied_usb` | device.csv | USB copy events |
| `files_accessed` | file.csv | Files opened |
| `emails_sent` | email.csv | Emails sent |
| `emails_external_pct` | email.csv | % of emails sent externally |
| `delta_*` (×7) | derived | Deviation from personal baseline |

---

## 🤖 Models

| Model | Type | Purpose |
|-------|------|---------|
| **Isolation Forest** | Unsupervised | Anomaly detection without labels |
| **Local Outlier Factor** | Unsupervised | Density-based outlier detection |
| **Random Forest** | Supervised | Classification against ground-truth labels |

---

## 📊 Results

| Metric | Value |
|--------|-------|
| Isolation Forest flagged | 6,606 records (2.00%) |
| LOF flagged | ~2% (agreement with IF) |
| Random Forest ROC-AUC | **0.88** |
| Random Forest Precision | **0.95** |
| Random Forest Recall | 0.31 |
| False alarms | 2 (of ~99,000 test records) |
| Real insiders caught (RF) | 70 / 191 |
| Real insiders caught (IF) | 38 / 191 |

**Key findings:**
- USB file-copying is the strongest predictor of insider threat
- Two distinct threat profiles identified: chronic (sustained) and spike (single-event)
- Model validated to genuinely learn (ROC-AUC 0.87 on unseen test data), not memorise labels

---

## 🚀 How to Run

1. Open `CERT_Insider_Threat_FINAL.ipynb` in [Google Colab](https://colab.research.google.com)
2. Upload the five CERT dataset files to your Google Drive
3. Run the cells in order — the notebook mounts Drive, processes the data, and runs all models
4. Total runtime ≈ 10–15 minutes (email.csv processing is the slowest step)

### Libraries used
```
pandas · numpy · scikit-learn · imbalanced-learn · matplotlib · seaborn
```

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `CERT_Insider_Threat_FINAL.ipynb` | Complete project notebook |
| `Insider_Threat_Final_Report.docx` | Detailed project report |
| `UEBA_Presentation.pptx` | Project presentation |
| `README.md` | This file |

---

## 👥 Team Contribution

**Swapnil Gawade** — Data collection & preprocessing, feature scaling, Isolation Forest & LOF implementation, exploratory analysis and visualisations.

**Unnati Patil** — Feature engineering (delta features), Random Forest classifier, ground-truth validation and analysis, feature importance interpretation.

---

## 🔮 Future Work

- Incorporate web (http) activity logs for additional signals
- Tune the decision threshold to increase recall
- Explore sequence models (LSTM) for temporal escalation patterns
- Add graph-based analysis for lateral movement detection

---

## 📚 Reference

Glasser, J., & Lindauer, B. (2013). *Bridging the Gap: A Pragmatic Approach to Generating Insider Threat Data.* CERT Division, Software Engineering Institute, Carnegie Mellon University.
