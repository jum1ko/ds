# System Patterns

## Architecture Overview

### Current Architecture (Competition Phase)

```
┌─────────────────────────────────────────────────────────────┐
│                      Data Layer                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │ beer_train   │  │  beer_test   │  │ sample_sub   │     │
│  │    .csv      │  │    .csv      │  │    .csv      │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                  Processing Layer                            │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Feature Engineering Pipeline                         │  │
│  │  - Text processing (name, description)                │  │
│  │  - Categorical encoding (isOrganic, glass)            │  │
│  │  - Numerical scaling (abv, gravity, srm)              │  │
│  │  - Feature interactions                               │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Modeling Layer                            │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Model Training & Validation                          │  │
│  │  - Cross-validation                                   │  │
│  │  - Hyperparameter tuning                              │  │
│  │  - Model selection                                    │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Output Layer                               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Prediction & Submission                              │  │
│  │  - Test set predictions                               │  │
│  │  - CSV submission file                                │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Design Patterns in Use

### 1. Iterative Development Pattern
**Philosophy:** Start simple, iterate and improve

**Implementation:**
- **Baseline:** Simple linear regression or Ridge regression
- **Iteration 1:** Feature engineering (text processing, interactions)
- **Iteration 2:** Advanced models (XGBoost, LightGBM, CatBoost)
- **Iteration 3:** Ensemble methods and optimization

**Rationale:** Ensures working end-to-end pipeline before optimization

### 2. Pipeline Pattern
**Philosophy:** Modular, reusable data processing steps

**Components:**
- Data loading
- Feature engineering
- Model training
- Prediction generation

**Benefits:**
- Reproducibility
- Easy experimentation
- Clear separation of concerns

### 3. Cross-Validation Pattern
**Philosophy:** Robust model evaluation

**Implementation:**
- K-fold cross-validation for model selection
- Holdout validation set for final evaluation
- Stratified splits if needed for category distribution

### 4. Feature Store Pattern (Planned)
**Philosophy:** Centralized feature definitions

**Future Implementation:**
- Feature engineering functions in `src/processing/`
- Feature registry/catalog
- Versioned feature pipelines

---

## Component Relationships

### Data Flow
```
Raw CSV → Pandas DataFrame → Feature Engineering → NumPy Arrays → Model → Predictions → CSV
```

### Code Organization
```
notebooks/          # Exploratory analysis (Jupyter)
    └── EDA         # Data exploration
    └── experiments # Model experiments

src/
    ├── processing/ # Reusable feature engineering
    │   ├── text_processing.py
    │   ├── categorical_encoding.py
    │   └── feature_engineering.py
    ├── training/   # Model training scripts
    │   ├── train.py
    │   └── evaluate.py
    └── app/        # Future: API service
        └── api.py  # FastAPI endpoints
```

---

## Key Technical Decisions

### 1. Mixed Data Type Handling
**Decision:** Separate processing pipelines for text, categorical, and numerical features

**Rationale:** 
- Different feature types require different preprocessing
- Allows independent optimization of each pipeline
- Enables feature selection per type

### 2. Model Selection Strategy
**Decision:** Start with gradient boosting (XGBoost/LightGBM) after baseline

**Rationale:**
- Handles mixed data types well
- Captures non-linear relationships
- Good performance on tabular data
- Feature importance for interpretability

### 3. Evaluation Strategy
**Decision:** K-fold cross-validation + holdout validation

**Rationale:**
- More robust than single train/test split
- Better estimate of generalization error
- Reduces overfitting risk

### 4. Code Structure
**Decision:** Separation of notebooks (exploration) and src (production code)

**Rationale:**
- Notebooks for rapid iteration and visualization
- src/ for reusable, tested code
- Clear transition from experiment to production

---

## Data Patterns

### Feature Types
1. **Identifiers:** `id` - Not used for prediction
2. **Text Features:** `name`, `description` - Require NLP processing
3. **Categorical:** `isOrganic`, `glass`, `available` - Require encoding
4. **Numerical:** `abv`, `originalGravity`, `srm` - May need scaling
5. **Target:** `ibu` - Continuous regression target

### Expected Relationships
- **ABV ↔ IBU:** Higher alcohol often correlates with higher IBU (both from recipe strength)
- **SRM ↔ IBU:** Color may indicate malt/hop balance
- **Description → IBU:** Text may contain hop-related keywords ("hoppy", "bitter", "IPA")
- **Original Gravity ↔ ABV:** Density correlates with alcohol content

---

## Future Architecture (Production)

```
┌──────────────┐
│   FastAPI    │ ← HTTP Requests (beer features)
│   Service    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Feature      │ ← Real-time feature engineering
│ Engineering  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Model       │ ← Loaded model artifact
│  Inference   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Response    │ → JSON: {ibu: 45.2, confidence: 0.92}
│  (IBU + CI)  │
└──────────────┘
```

---

## Patterns to Avoid

1. **Data Leakage:** Never use future information (test set features during training)
2. **Target Encoding Leakage:** Careful with target encoding on categorical features
3. **Overfitting:** Avoid over-optimizing to public leaderboard
4. **Feature Engineering in Notebooks:** Move proven features to src/processing/

---

## Dependencies Between Components

- **Feature Engineering** → Required before **Model Training**
- **Model Training** → Requires **Cross-Validation** setup
- **Prediction** → Requires trained **Model** + **Feature Engineering** pipeline
- **Submission** → Requires **Predictions** in correct format
