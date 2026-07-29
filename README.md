# Lightweight Explainable Intrusion Detection System for Low-Resource Environments

![Python](https://img.shields.io/badge/Python-3.10-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey)

A lightweight and explainable supervised machine learning-based Intrusion Detection System (IDS) designed for deployment in low-resource computing environments with 4 GB RAM and dual-core CPU configurations.

---

## Table of Contents

- [Overview](#overview)
- [Research Objectives](#research-objectives)
- [System Architecture](#system-architecture)
- [Features](#features)
- [Repository Structure](#repository-structure)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Installation](#installation)
- [Dataset](#dataset)
- [Training the Models](#training-the-models)
- [Running the IDS](#running-the-ids)
- [Synthetic Attack Scenarios](#synthetic-attack-scenarios)
- [Results Summary](#results-summary)
- [Citation](#citation)
- [License](#license)
- [Contact](#contact)
- [Acknowledgements](#acknowledgements)
- [Disclaimer](#disclaimer)

---

## Overview

This repository contains the source code, configuration files, and synthetic PCAP generation scripts for a lightweight and explainable Intrusion Detection System (IDS) developed for deployment in resource-constrained computing environments.

The project addresses the challenge of deploying effective intrusion detection in schools, small and medium-sized enterprises (SMEs), and public institutions operating on ageing hardware with limited computational resources.

Unlike conventional IDS solutions that primarily prioritise predictive accuracy, this implementation jointly optimises software architecture, machine learning performance, and explainability to provide a practical and deployment-ready solution for low-resource environments.

### Key Contributions

- Demonstrates that packet-processing architecture optimisation using **dpkt** streaming contributes more to deployment efficiency than machine learning algorithm selection.
- Implements an optimised 11-feature flow-based representation validated through feature ablation analysis.
- Compares four supervised machine learning algorithms under identical low-resource hardware conditions.
- Provides a lightweight explainability framework that translates model outputs into plain-language security alerts.
- Includes synthetic PCAP generation scripts for evaluating regionally relevant cyberattack scenarios.

---

## Research Objectives

1. Develop a lightweight IDS capable of operating on hardware with 4 GB RAM and a dual-core CPU.
2. Evaluate the performance of Random Forest, Decision Tree, Logistic Regression, and LightGBM under constrained computing conditions.
3. Compare two packet-processing architectures: the Scapy-based baseline (Version 1.0) and the optimised dpkt-based streaming implementation (Version 2.0).
4. Improve model interpretability through plain-language security alerts for non-expert administrators.
5. Validate the IDS using synthetic attack scenarios representing regionally relevant cyber threats.

---

## System Architecture

### Version 1.0 – Baseline (Scapy-Based)

- Object-oriented packet parsing.
- Constructs complete Python objects for each packet and protocol layer.
- Comprehensive packet inspection capabilities.
- Higher memory allocation and garbage collection overhead.
- Suitable for detailed packet analysis but computationally intensive.

### Version 2.0 – Optimised (dpkt-Based Streaming)

- Byte-oriented packet parsing.
- Interprets packet headers directly from raw bytes.
- Extracts only the fields required for feature generation.
- Streaming architecture processes packets without loading entire PCAP files into memory.
- Achieved **11.7× faster processing** with **48% lower peak memory consumption**.

### System Components

1. **Packet Capture Module** – Reads PCAP files.
2. **Flow Aggregation Module** – Groups packets into bidirectional network flows.
3. **Feature Extraction Module** – Extracts the selected 11 flow-based features.
4. **Classification Module** – Performs machine learning inference.
5. **Explainability Module** – Generates plain-language security alerts.
6. **Graphical User Interface (GUI) Module** – Displays alerts and system information.

---

## Features

### Lightweight Implementation

- Designed to operate on systems with **4 GB RAM** and a **dual-core CPU**.
- Optimised for deployment in resource-constrained computing environments.
- Supports efficient offline PCAP-based network traffic analysis.

### Explainable Alerts

- Generates plain-language security alerts.
- Feature importance-based explanations.
- Approximately **0.02 ms** explanation overhead compared to approximately **2,800 ms** for SHAP.
- Achieved a **100% comprehension rate** during the explainability usability evaluation.

### Machine Learning Models Overview

| Model | F1-Score | Peak Memory | Inference Latency |
|:---|:---:|:---:|:---:|
| **Random Forest** | 0.87 | ~280 MB | 110 ms / 1000 flows |
| **Decision Tree** | 0.83 | ~65 MB | 40 ms / 1000 flows |
| **Logistic Regression** | 0.72 | ~42 MB | 20 ms / 1000 flows |
| **LightGBM** | 0.76 | ~110 MB | 80 ms / 1000 flows |

### Selected Features (11-Feature Set)

| Rank | Feature | Description |
|:---:|:---|:---|
| 1 | `spkts` | Number of packets transmitted from source to destination |
| 2 | `state` | Connection state and associated protocol state |
| 3 | `rate` | Average packet transmission rate (packets per second) |
| 4 | `dpkts` | Number of packets transmitted from destination to source |
| 5 | `proto` | Network transport protocol (e.g., TCP, UDP, ICMP) |
| 6 | `dur` | Total duration of the network flow |
| 7 | `sbytes` | Total bytes transferred from source to destination |
| 8 | `dbytes` | Total bytes transferred from destination to source |
| 9 | `service` | Destination network service (e.g., HTTP, FTP, DNS) |
| 10 | `sttl` | Source-to-destination Time-To-Live (TTL) value |
| 11 | `dttl` | Destination-to-source Time-To-Live (TTL) value |

---

## Repository Structure

```text
lightweight-explainable-ids/
│
├── README.md                  # Project documentation
├── LICENSE                    # MIT License file
├── requirements.txt           # Python dependencies
├── .gitignore                 # Git ignore rules
│
├── config/
│   └── model_params.yaml      # Model hyperparameters
│
├── data/
│   └── README.md              # Dataset download instructions
│
├── src/
│   ├── __init__.py
│   ├── packet_parser/
│   │   ├── __init__.py
│   │   ├── scapy_parser.py
│   │   └── dpkt_parser.py
│   ├── feature_extraction/
│   │   ├── __init__.py
│   │   └── extract_features.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── train_model.py
│   │   └── predict.py
│   ├── explainability/
│   │   ├── __init__.py
│   │   └── explain.py
│   └── utils/
│       ├── __init__.py
│       └── helpers.py
│
├── scripts/
│   ├── generate_bruteforce.py
│   ├── generate_smishing.py
│   ├── generate_c2_beacon.py
│   └── generate_baseline.py
│
├── tests/
│   ├── __init__.py
│   └── test_models.py
│
└── results/
    └── .gitkeep
```

---

## Hardware Requirements

### Minimum Configuration

| Component | Specification |
|-----------|---------------|
| Processor | Dual-core x86_64 CPU @ 2.00 GHz |
| Memory | 4 GB DDR3 / DDR4 |
| Storage | 256 GB SSD |
| Operating System | Ubuntu 22.04 LTS (64-bit) |

### Recommended Configuration

| Component | Specification |
|-----------|---------------|
| Processor | Quad-core x86_64 CPU @ 2.50 GHz |
| Memory | 8 GB DDR4 |
| Storage | 512 GB SSD |
| Operating System | Ubuntu 22.04 LTS (64-bit) |

---

## Software Requirements

### Operating Systems

- Ubuntu 22.04 LTS or newer
- Windows 10/11
- macOS (experimental)

### Python Runtime

- Python 3.10 or newer

### Required Python Libraries

| Library | Version | Purpose |
|---------|---------|---------|
| scikit-learn | ≥1.3.0 | Model implementation and evaluation |
| pandas | ≥2.0.3 | Data preprocessing and manipulation |
| numpy | ≥1.24.3 | Numerical computation |
| lightgbm | ≥4.0.0 | Gradient boosting classifier |
| scapy | ≥2.5.0 | Baseline packet parsing (Version 1.0) |
| dpkt | ≥1.9.8 | High-performance packet parsing (Version 2.0) |
| pyyaml | ≥6.0 | Configuration parsing |

---

## Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/<your-username>/lightweight-explainable-ids.git
cd lightweight-explainable-ids
```

### Step 2: Create a Virtual Environment

**Linux/macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows:**
```cmd
python -m venv venv
venv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Download the Dataset

Download the **UNSW-NB15** dataset from one of the official sources below:

- https://research.unsw.edu.au/projects/unsw-nb15-dataset
- https://researchdata.edu.au/the-unsw-nb15-dataset/1957529

Extract the downloaded files into the `data/` directory.

### Step 5: Verify the Installation

```bash
python -c "import sklearn, pandas, numpy, lightgbm, scapy, dpkt; print('Installation successful!')"
```

---

## Dataset

### UNSW-NB15 Dataset

This project uses the **UNSW-NB15** dataset developed by the Australian Centre for Cyber Security (ACCS), University of New South Wales.

### Official Dataset Sources

- https://research.unsw.edu.au/projects/unsw-nb15-dataset
- https://researchdata.edu.au/the-unsw-nb15-dataset/1957529

### Dataset Summary

| Attribute | Value |
|-----------|-------|
| Total Records | 2,540,044 |
| Training Records | 1,790,050 |
| Testing Records | 750,000 |
| Features | 49 (including class label) |
| Attack Categories | 9 |
| Normal Traffic | Approximately 60% |
| Attack Traffic | Approximately 40% |

### Attack Categories

- Fuzzers
- Analysis
- Backdoors
- DoS
- Exploits
- Generic
- Reconnaissance
- Shellcode
- Worms

### Dataset Directory Setup

After downloading and extracting, your `data/` directory should be structured as follows:

```text
data/
├── UNSW_NB15_training-set.csv
├── UNSW_NB15_testing-set.csv
└── NUSW-NB15_features.csv
```

---

## Training the Models

### Training All Models

```bash
python src/models/train_model.py --config config/model_params.yaml
```

This trains all four models (Random Forest, Decision Tree, Logistic Regression, LightGBM) using the UNSW-NB15 training partition.

### Training a Specific Model

```bash
python src/models/train_model.py --model random_forest
python src/models/train_model.py --model decision_tree
python src/models/train_model.py --model logistic_regression
python src/models/train_model.py --model lightgbm
```

### Hyperparameter Configuration

Model hyperparameters are defined in `config/model_params.yaml`:

```yaml
Random Forest:
  n_estimators: 100
  max_depth: 10
  min_samples_split: 5
  min_samples_leaf: 2
  criterion: gini
  max_features: sqrt
  bootstrap: true

Decision Tree:
  max_depth: 8
  min_samples_split: 10
  min_samples_leaf: 5
  criterion: entropy
  splitter: best

Logistic Regression:
  C: 1.0
  solver: lbfgs
  max_iter: 1000
  penalty: l2
  class_weight: balanced

LightGBM:
  n_estimators: 100
  learning_rate: 0.1
  num_leaves: 31
  min_child_samples: 20
  subsample: 0.8
  colsample_bytree: 0.8
  class_weight: balanced
  metric: multi_logloss
```

### Expected Benchmark Training Times (4 GB RAM, Dual-Core)

| Model | Approx. Training Time |
|-------|----------------------|
| Random Forest | ~5–8 minutes |
| Decision Tree | ~2–3 minutes |
| Logistic Regression | ~1–2 minutes |
| LightGBM | ~3–5 minutes |

---

## Running the IDS

### Single Pipeline Steps

**Using Optimised Architecture (dpkt):**
```bash
python src/packet_parser/dpkt_parser.py --input data/sample.pcap --output results/alerts.log
```

**Using Baseline Architecture (Scapy):**
```bash
python src/packet_parser/scapy_parser.py --input data/sample.pcap --output results/alerts.log
```

**Extracting Features from PCAP:**
```bash
python src/feature_extraction/extract_features.py --input data/sample.pcap --output data/features.csv
```

**Running Classification on Features:**
```bash
python src/models/predict.py --input data/features.csv --model models/random_forest.pkl --output results/predictions.csv
```

**Generating Explainable Alerts:**
```bash
python src/explainability/explain.py --input results/alerts.log --output results/explanations.txt
```

### Full Pipeline Execution (PCAP → Alerts)

```bash
python src/packet_parser/dpkt_parser.py --input data/sample.pcap --output results/flows.csv
python src/models/predict.py --input results/flows.csv --output results/predictions.csv
python src/explainability/explain.py --input results/predictions.csv --output results/alerts.txt
```

### GUI Mode

**Linux:**
```bash
python src/gui/ids_gui.py
```

**Windows:**
```cmd
python src\gui\ids_gui.py
```

---

## Synthetic Attack Scenarios

Scripts are provided to generate synthetic PCAPs reflecting regionally relevant cyber threats.

### Available Scenarios

| Scenario | Description | Packet Volume |
|----------|-------------|---------------|
| Mobile Money Brute-Force | Credential stuffing against mobile payment APIs | ~10,000 |
| Smishing-Related Behaviour | Phishing payload distribution over IP | ~8,000 |
| Command-and-Control Beaconing | Low-volume DNS/ICMP egress tunnelling | ~5,000 |
| Normal Baseline Traffic | Standard HTTP/DNS institutional communication | ~15,000 |

### Generate All Synthetic Scenarios

```bash
mkdir -p data/synthetic

python scripts/generate_bruteforce.py --output data/synthetic/bruteforce.pcap
python scripts/generate_smishing.py --output data/synthetic/smishing.pcap
python scripts/generate_c2_beacon.py --output data/synthetic/c2_beacon.pcap
python scripts/generate_baseline.py --output data/synthetic/baseline.pcap

echo "Synthetic PCAP files generated successfully in data/synthetic/."
```

---

## Results Summary

### Predictive Performance

| Model | Accuracy | F1-Score | FPR | Inference Latency | Peak Memory |
|:---|:---:|:---:|:---:|:---:|:---:|
| Random Forest | 89.2% | 0.87 ± 0.01 | 0.03 ± 0.005 | 110 ± 8 ms | ~280 MB |
| Decision Tree | 84.6% | 0.83 ± 0.02 | 0.05 ± 0.008 | 40 ± 5 ms | ~65 MB |
| Logistic Regression | 78.4% | 0.72 ± 0.015 | 0.08 ± 0.010 | 20 ± 3 ms | ~42 MB |
| LightGBM | 81.7% | 0.76 ± 0.02 | 0.06 ± 0.009 | 80 ± 7 ms | ~110 MB |

### System Architecture Comparison

| Metric | Baseline (Scapy) | Optimised (dpkt) | Improvement |
|:---|:---:|:---:|:---:|
| Analysis Time (200 MB PCAP) | 1,482.4 s | 126.9 s | **11.7× faster** |
| Peak Memory Usage | ~540 MB | ~280 MB | **48% reduction** |
| Out-of-Memory Events | Observed (>300 MB) | None observed | Stable streaming |

### Synthetic Attack Scenario Evaluation

| Simulated Scenario | Equivalent Attack | Detection Result | Confidence |
|:---|:---|:---:|:---:|
| Mobile Money Brute-Force | Brute Force | Detected | 74% |
| Smishing-Related Behaviour | Web Attack | Detected | 71% |
| Command-and-Control Beaconing | Backdoor | Detected | 75% |
| Normal Institutional Traffic | Benign | No false positives | 97% |

### Explainability Evaluation

| Participant Group | Participants | Mean Confidence (1–5) | Comprehension Rate |
|:---|:---:|:---:|:---:|
| ICT Professionals | 5 | 4.8 | 100% |
| University Interns | 5 | 4.5 | 100% |
| **Overall** | **10** | **4.65** | **100%** |

### Feature Ablation Study

| Number of Features | F1-Score | Inference Latency |
|:---:|:---:|:---:|
| 49 | 0.87 | 190 ms |
| 30 | 0.87 | 160 ms |
| 20 | 0.87 | 135 ms |
| **11 (Selected)** | **0.87** | **110 ms** |
| 8 | 0.84 | 85 ms |
| 5 | 0.78 | 60 ms |

**Key Finding:** Reducing features from 49 to 11 maintained predictive power (F1 = 0.87) while lowering latency by 42%.

---

## Citation

If you use this repository in academic work, please cite the associated thesis and dataset:

```bibtex
@mastersthesis{Tembo2026LightweightIDS,
  author  = {Tembo, O'Brien Mwaanza},
  title   = {Design and Evaluation of a Lightweight, Explainable Supervised Machine Learning-Based Intrusion Detection System for Low-Resource Environments},
  school  = {Mulungushi University},
  address = {Lusaka, Zambia},
  year    = {2026}
}

@inproceedings{Moustafa2015UNSWNB15,
  author    = {Moustafa, N. and Slay, J.},
  title     = {UNSW-NB15: A Comprehensive Data Set for Network Intrusion Detection Systems},
  booktitle = {2015 Military Communications and Information Systems Conference (MilCIS)},
  pages     = {1--6},
  year      = {2015},
  doi       = {10.1109/MilCIS.2015.7348942}
}
```

---

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.

```
MIT License

Copyright (c) 2026 O'Brien Mwaanza Tembo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Contact

**Author:** O'Brien Mwaanza Tembo
- Email: obrien.m.tembo@gmail.com
- Institution: Mulungushi University, Zambia
- Department: Computer Science and Information Technology

**Supervisor:** Dr. Brian Halubanza
- Email: bhalubanza@mu.ac.zm
- Institution: Mulungushi University, Zambia
- Department: Computer Science and Information Technology

---

## Acknowledgements

This research was supported by the Department of Computer Science and Information Technology, Mulungushi University. Special acknowledgment to ZICTA for publishing the cybersecurity reports that guided the problem definition, and to the open-source communities behind scikit-learn, LightGBM, Scapy, and dpkt that made this implementation possible.

This research received no external funding.

---

## Disclaimer

The views and conclusions expressed in this repository are those of the author and do not necessarily reflect the official policies or positions of Mulungushi University or ZICTA.
