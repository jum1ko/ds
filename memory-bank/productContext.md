# Product Context

## Why This Project Exists

### Business Problem
The beer brewing industry faces significant challenges in predicting beer bitterness (IBU) accurately:

1. **Cost of Physical Testing:** Measuring IBU requires laboratory testing, which is expensive and time-consuming
2. **Quality Control:** Breweries need consistent bitterness across batches
3. **Recipe Development:** New beer formulations need IBU predictions before production
4. **Standardization:** Ensuring taste consistency across production runs

### Industry Impact
- **Cost Reduction:** Eliminate need for physical testing of every sample
- **Faster Iteration:** Develop new recipes without extensive lab work
- **Quality Assurance:** Predict and control bitterness before brewing
- **Innovation:** Enable experimentation with new ingredients and processes

---

## Problems This Project Solves

### Primary Problem
**Accurate IBU Prediction:** Predict beer bitterness from observable characteristics (color, alcohol, density, description) without physical testing.

### Secondary Problems
1. **Mixed Data Types:** Handle text (descriptions), categorical (glass type, organic), and numerical features (ABV, gravity, SRM)
2. **Feature Engineering:** Extract meaningful signals from unstructured text data
3. **Model Generalization:** Build robust models that work on diverse beer styles

---

## How It Should Work

### User Workflow (Current - Competition Phase)

1. **Data Input:** Provide beer characteristics (train/test CSV files)
2. **Model Processing:** 
   - Feature engineering from mixed data types
   - Model prediction generation
3. **Output:** CSV file with IBU predictions for test set
4. **Evaluation:** Submit to Kaggle for RMSE scoring

### Future Production Workflow (Planned)

1. **Input:** New beer recipe/formulation characteristics
2. **API Call:** Send features to prediction service
3. **Processing:** Real-time feature engineering and model inference
4. **Output:** IBU prediction with confidence intervals
5. **Application:** Use prediction to adjust recipe or proceed with brewing

---

## User Experience Goals

### For Data Scientist (Current)
- **Clear Project Structure:** Easy navigation and code organization
- **Reproducible Experiments:** All experiments logged and reproducible
- **Fast Iteration:** Quick feedback loop for model improvements
- **Documentation:** Clear understanding of data, features, and model decisions

### For End Users (Future)
- **Fast Predictions:** < 1 second response time for API calls
- **Accurate Results:** High confidence in IBU predictions
- **Easy Integration:** Simple API for incorporating into brewing workflows
- **Transparent:** Feature importance and prediction explanations

---

## Success Metrics (Product Perspective)

### Competition Phase
- **Leaderboard Position:** High ranking on Kaggle leaderboard
- **Model Performance:** Low RMSE indicating accurate predictions
- **Code Quality:** Clean, maintainable, well-documented codebase

### Production Phase (Future)
- **Adoption:** Breweries use the system for recipe development
- **Accuracy:** Production predictions match laboratory measurements
- **Reliability:** System availability > 99%
- **User Satisfaction:** Brewers trust and rely on predictions

---

## Key Assumptions

1. **Data Quality:** Training and test data are representative of real-world beer characteristics
2. **Feature Availability:** All required features (ABV, SRM, gravity, etc.) are available for new beers
3. **Linearity:** IBU can be predicted from observable characteristics (not requiring chemical analysis)
4. **Generalization:** Models trained on competition data will generalize to real brewery data

---

## Domain Knowledge

### Beer Characteristics
- **IBU (International Bittering Units):** Measure of beer bitterness (0-100+)
- **ABV (Alcohol By Volume):** Percentage of alcohol content
- **SRM (Standard Reference Method):** Color scale for beer (related to malt)
- **Original Gravity:** Density of wort before fermentation (indicates sugar content)

### Brewing Process
- Hops addition timing and quantity affects IBU
- Malt selection influences color (SRM) and flavor
- Fermentation process affects final alcohol content (ABV)
- Recipe descriptions often contain clues about ingredients and process
