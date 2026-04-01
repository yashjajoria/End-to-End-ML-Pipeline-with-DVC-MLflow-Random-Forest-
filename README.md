# End-to-End ML Pipeline with DVC & MLflow

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![DVC](https://img.shields.io/badge/DVC-Data%20Version%20Control-13ADC7)
![MLflow](https://img.shields.io/badge/MLflow-Experiment%20Tracking-0B5FFF)
![License](https://img.shields.io/badge/License-MIT-green)

An end-to-end, **reproducible machine learning pipeline** that trains a **Random Forest Classifier** on the **Pima Indians Diabetes** dataset. The project uses:

- **DVC (Data Version Control)** for dataset and pipeline versioning
- **MLflow** for experiment tracking (params, metrics, and artifacts)

The goal is to provide a clean, modular, beginner-friendly template for running ML experiments with strong reproducibility and collaboration practices.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Project Structure](#project-structure)
- [Pipeline Stages](#pipeline-stages)
- [Installation](#installation)
- [Running the Pipeline (DVC)](#running-the-pipeline-dvc)
- [Experiment Tracking (MLflow)](#experiment-tracking-mlflow)
- [Usage Example](#usage-example)
- [Tech Stack](#tech-stack)
- [Project Goals](#project-goals)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview

This repository demonstrates how to build and maintain a complete ML workflow using modern MLOps tooling. Instead of running scripts manually, the workflow is captured as a **DVC pipeline** so you can:

- Version datasets and intermediate artifacts
- Re-run the full workflow with a single command (`dvc repro`)
- Track each experiment run with **MLflow**, including hyperparameters and model metrics

The model used is a **Random Forest Classifier**, which is a strong baseline for tabular binary classification.

---

## Key Features

- **Data versioning with DVC** (raw/processed data and model artifacts)
- **Pipeline automation**: `preprocess → train → evaluate`
- **Experiment tracking with MLflow**
  - Log hyperparameters, metrics, and artifacts (models, plots, etc.)
- **Reproducibility**
  - Deterministic pipeline stages + parameterized training (`params.yaml`)
- **Modular, clean project structure**
  - Separate scripts for preprocessing, training, and evaluation

---

## Project Structure

```text
.
├── data/
│   ├── raw/               # Raw dataset
│   └── processed/         # Processed / feature-ready dataset
├── models/                # Saved trained models (artifacts)
├── src/
│   ├── preprocess.py      # Data preprocessing script
│   ├── train.py           # Model training script
│   └── evaluate.py        # Model evaluation script
├── dvc.yaml               # DVC pipeline definition (stages)
├── params.yaml            # Hyperparameters / pipeline parameters
└── README.md
```

---

## Pipeline Stages

The pipeline is defined in `dvc.yaml` and consists of three main stages:

### 1) Preprocessing

**Script:** `src/preprocess.py`

- Reads the raw dataset from `data/raw/`
- Cleans and prepares data for training
- Writes the processed dataset to `data/processed/`

### 2) Training

**Script:** `src/train.py`

- Loads processed data from `data/processed/`
- Trains a **Random Forest Classifier**
- Saves the trained model under `models/`
- Logs params, metrics, and artifacts to **MLflow**

### 3) Evaluation

**Script:** `src/evaluate.py`

- Loads the trained model from `models/`
- Evaluates performance on a validation/test split
- Logs evaluation metrics to **MLflow**

---

## Installation

### Prerequisites

- **Python 3.9+**
- **pip**
- **Git**
- **DVC**
- **MLflow**

> Note: If you use a DVC remote (S3/GCS/Azure/Drive/etc.), you’ll also need credentials for that remote.

### Setup

1) **Clone the repository**

```bash
git clone https://github.com/yashjajoria/End-to-End-ML-Pipeline-with-DVC-MLflow-Random-Forest-.git
cd End-to-End-ML-Pipeline-with-DVC-MLflow-Random-Forest-
```

2) **Create & activate a virtual environment**

```bash
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows (PowerShell)
# .venv\Scripts\Activate.ps1
```

3) **Install dependencies**

If your repo includes `requirements.txt`:

```bash
pip install -r requirements.txt
```

Otherwise, install the essentials:

```bash
pip install dvc mlflow scikit-learn pandas numpy matplotlib
```

4) **Initialize DVC (first time only)**

```bash
dvc init
```

---

## Running the Pipeline (DVC)

### Reproduce the pipeline

Run all stages that are out-of-date (based on data, code, and parameters):

```bash
dvc repro
```

### Visualize the pipeline DAG

```bash
dvc dag
```

### Helpful DVC commands

- Check what would re-run:

```bash
dvc status
```

- Re-run a specific stage (use the stage name from `dvc.yaml`):

```bash
dvc repro <stage-name>
```

- Pull tracked data/artifacts (when using a DVC remote):

```bash
dvc pull
```

---

## Experiment Tracking (MLflow)

MLflow is used to track:

- **Parameters** (e.g., `n_estimators`, `max_depth`, `random_state`)
- **Metrics** (e.g., accuracy, precision, recall, F1, ROC-AUC)
- **Artifacts** (trained model files, plots, confusion matrix, etc.)

### Start the MLflow UI

From the project root:

```bash
mlflow ui
```

Then open (default):

- http://127.0.0.1:5000

### Typical workflow

1) Update hyperparameters in `params.yaml`
2) Re-run the pipeline:

```bash
dvc repro
```

3) Compare runs in the MLflow UI

---

## Usage Example

A quick end-to-end run after installing dependencies:

```bash
# (Optional) if data is stored in a DVC remote
# dvc pull

# Run the full pipeline
dvc repro

# Inspect the pipeline graph
dvc dag

# View experiments
mlflow ui
```

---

## Tech Stack

- **Language:** Python
- **ML / Data:** pandas, NumPy, scikit-learn
- **Pipeline & Data Versioning:** DVC
- **Experiment Tracking:** MLflow
- **Version Control:** Git & GitHub

---

## Project Goals

- **Reproducibility:** Anyone can reproduce results by running the same pipeline.
- **Experimentation:** Rapid iteration with tracked history via MLflow.
- **Collaboration:** A consistent workflow that teams can share, review, and extend.

---

## Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch
3. Commit with clear messages
4. Open a Pull Request describing what you changed and why

---
