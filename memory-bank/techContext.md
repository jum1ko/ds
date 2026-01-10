# Technical Context

## Technologies Used

### Core ML Stack
- **Python 3.x** - Primary programming language
- **Pandas** - Data manipulation and analysis
- **NumPy** - Numerical computations
- **Scikit-learn** - Baseline models, preprocessing, evaluation metrics
- **XGBoost / LightGBM / CatBoost** - Advanced gradient boosting models (planned)

### Data Processing
- **Pandas** - CSV reading, dataframes, data cleaning
- **NLTK / spaCy** - Natural language processing for text features (planned)
- **TF-IDF / CountVectorizer** - Text feature extraction (planned)

### Visualization & Exploration
- **Matplotlib** - Basic plotting
- **Seaborn** - Statistical visualizations
- **Jupyter Notebooks** - Interactive exploration and documentation

### Model Development
- **Scikit-learn** - Linear models, preprocessing pipelines
- **Optuna / Hyperopt** - Hyperparameter optimization (planned)
- **MLflow** - Experiment tracking (optional, planned)

### Development Tools
- **Git** - Version control (connected to GitHub)
- **Virtual Environment** - Python dependency isolation (.venv/)
- **Docker** - Containerization (planned for API service)

---

## Development Setup

### Environment Requirements
- **Python:** 3.8+ (check with `python --version`)
- **Package Manager:** pip (via requirements.txt)
- **Virtual Environment:** `.venv/` directory (already created)

### Setup Steps

1. **Create/Activate Virtual Environment:**
```bash
python -m venv .venv
source .venv/bin/activate  # On macOS/Linux
```

2. **Install Dependencies:**
```bash
pip install -r requirements.txt
```

3. **Current Dependencies:**
   - requirements.txt is currently empty (to be populated)

### Planned Dependencies

```txt
# Core Data Science
pandas>=1.5.0
numpy>=1.23.0
scikit-learn>=1.2.0

# Advanced Models
xgboost>=2.0.0
lightgbm>=4.0.0
catboost>=1.2.0

# Visualization
matplotlib>=3.6.0
seaborn>=0.12.0

# Notebooks
jupyter>=1.0.0
ipykernel>=6.20.0

# NLP (if needed)
nltk>=3.8.0
spacy>=3.5.0

# Hyperparameter Tuning
optuna>=3.2.0

# API (future)
fastapi>=0.100.0
uvicorn>=0.23.0
pydantic>=2.0.0
```

---

## Technical Constraints

### Data Constraints
- **Training Data:** Fixed dataset (beer_train.csv) - cannot augment with external data (competition rules)
- **Test Data:** No target labels available (blind test set)
- **Submission Format:** CSV with specific columns (id, ibu predictions)

### Computational Constraints
- **Local Development:** Limited by local machine resources
- **Model Size:** Should be reasonable for deployment (future consideration)
- **Inference Speed:** For future API, target < 1 second per prediction

### Platform Constraints
- **Kaggle Submission:** Must use Kaggle's submission format
- **File Size Limits:** Kaggle may have file size limits for submissions
- **Runtime Limits:** Kaggle kernels have runtime limits (if used)

---

## Dependencies & Versions

### Current Status
- **requirements.txt:** Empty (needs initialization)
- **Python Environment:** Virtual environment exists (.venv/)
- **Git:** Configured with remote: https://github.com/jum1ko/ds.git

### Version Management Strategy
- **requirements.txt:** Pin major versions (>=) for flexibility
- **Virtual Environment:** Isolated per project
- **Git:** Track requirements.txt, ignore .venv/

---

## Development Workflow

### Local Development
1. **Activate virtual environment:** `source .venv/bin/activate`
2. **Install/Update packages:** `pip install -r requirements.txt`
3. **Jupyter Notebooks:** Launch from project root
4. **Run scripts:** `python src/training/train.py`

### Experiment Workflow
1. **Exploration:** Use notebooks/ for EDA and experiments
2. **Code Extraction:** Move proven code to src/ for reusability
3. **Testing:** Test feature engineering and model code
4. **Submission:** Generate predictions and CSV files

### Git Workflow
- **Branch Strategy:** Main branch for stable code
- **Commits:** Feature-based commits (EDA, model training, submission)
- **Ignore:** .venv/, __pycache__/, *.ipynb_checkpoints/, artifacts/ (large files)

---

## External Dependencies

### Data Sources
- **Primary:** Competition dataset (beer_train.csv, beer_test.csv)
- **External Data:** Not allowed per competition rules (unless specified)

### APIs & Services
- **Current:** None (offline development)
- **Future:** FastAPI service for predictions (self-hosted)

### Infrastructure
- **Current:** Local development (laptop/server)
- **Future:** Docker containers for API deployment

---

## Performance Considerations

### Model Training
- **Baseline Models:** Fast training (< 1 minute)
- **Gradient Boosting:** Moderate training time (5-30 minutes depending on hyperparameters)
- **Ensemble Methods:** Longer training (may require hours)

### Inference
- **Single Prediction:** < 100ms target (future API)
- **Batch Predictions:** Parallel processing for test set

### Data Processing
- **Feature Engineering:** Cached intermediate results when possible
- **Text Processing:** Pre-compute embeddings/features if computationally expensive

---

## Security & Best Practices

### Data Handling
- **Sensitive Data:** None in this project (public competition data)
- **Credentials:** Use environment variables for API keys (if needed in future)

### Code Quality
- **Type Hints:** Use Python type hints for better code clarity
- **Documentation:** Docstrings for functions and classes
- **Linting:** Consider using flake8 or black for code formatting

### Reproducibility
- **Random Seeds:** Set random seeds for reproducibility
- **Version Control:** Track all code changes in Git
- **Experiments:** Log hyperparameters and results (future: MLflow)

---

## Platform-Specific Notes

### macOS (Current Environment)
- **Python:** System Python or Homebrew Python
- **Virtual Environment:** Standard venv module
- **Package Installation:** pip (may need --user flag if permissions issue)

### Future Deployment
- **Docker:** Linux-based containers for API service
- **Cloud:** Could deploy to AWS, GCP, or Azure (if needed)

---

## Known Technical Issues

### Current Issues
- **requirements.txt:** Empty - needs initial dependency setup
- **No Python code:** Project structure exists but code not yet written

### Potential Issues
- **Memory:** Large datasets may require chunked processing
- **Text Processing:** May need GPU for transformer models (if used)
- **Dependencies:** Version conflicts between packages (manage with virtual env)
