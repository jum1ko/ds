# Project Brief

## Project Overview

**Project Name:** How Bitter is the Beer? - Kaggle Competition ML Project

**Type:** Machine Learning Regression Challenge

**Competition:** [Kaggle - How Bitter is the Beer](https://www.kaggle.com/competitions/how-bitter-is-the-beer)

**Repository:** https://github.com/jum1ko/ds

---

## Core Requirements

### Problem Statement
Build a machine learning model to predict beer bitterness (IBU - International Bittering Units) based on beer characteristics including color, alcohol content, density, and other attributes.

### Task Type
**Regression** - Predicting continuous IBU values (typically 0-100+ range)

### Primary Goal
Minimize Root Mean Squared Error (RMSE) on the test set to accurately predict beer bitterness.

### Key Constraints
- Must use provided dataset (beer_train.csv, beer_test.csv)
- Target variable `ibu` only available in training data
- Model must generalize to unseen test data
- Submission format: CSV with predictions for test set

---

## Project Scope

### In Scope
- Data exploration and analysis (EDA)
- Feature engineering from mixed data types (text, categorical, numerical)
- Model development and training
- Hyperparameter optimization
- Model evaluation and validation
- Submission file generation for Kaggle

### Out of Scope (Current Phase)
- Production deployment (future phase)
- Real-time serving API (future phase)
- Continuous model retraining pipeline (future phase)

---

## Success Criteria

### Technical Metrics
- **Primary:** Minimize RMSE on test set
- **Secondary:** Stable cross-validation performance
- **Tertiary:** Model interpretability and feature importance

### Practical Metrics
- Reproducible end-to-end pipeline
- Well-documented code and experiments
- Clear project structure for collaboration

### Target Performance
- **Baseline Goal:** RMSE < 15 IBU
- **Good Performance:** RMSE < 10 IBU  
- **Excellent Performance:** RMSE < 5 IBU

---

## Project Timeline

**Current Phase:** Initial Setup & Baseline Development

**Planned Phases:**
1. ✅ Project initialization & Memory Bank setup
2. ⏳ Data exploration and analysis (EDA)
3. ⏳ Baseline model development
4. ⏳ Feature engineering iterations
5. ⏳ Advanced model experimentation
6. ⏳ Hyperparameter optimization
7. ⏳ Final model selection and submission

---

## Key Stakeholders

- **Primary User:** Kaggle competition participant (jum1ko)
- **End Users:** Beer manufacturers (indirect - through competition use case)
- **Domain:** Food & Beverage Industry / Brewing

---

## Project Structure (Planned)

```
ds/
├── data/                    # Raw competition data
├── notebooks/              # EDA and experiments (Jupyter)
├── src/                    # Production code
│   ├── processing/        # Data preprocessing pipelines
│   ├── training/          # Model training scripts
│   └── app/               # Future: API service (FastAPI)
├── artifacts/             # Saved models, scalers, encoders
├── submissions/           # Kaggle submission files
├── memory-bank/           # Project documentation
├── requirements.txt       # Python dependencies
├── Dockerfile            # Future: Containerization
└── README.md             # Project overview
```
