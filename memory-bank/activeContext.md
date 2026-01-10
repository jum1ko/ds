# Active Context

## Current Work Focus

### Immediate Priority
**Memory Bank Initialization** - Setting up project documentation and structure

**Status:** ✅ In Progress - Creating Memory Bank files

### Next Steps (After Memory Bank Setup)
1. **Data Exploration (EDA)** - Understand the beer dataset structure and characteristics
2. **Baseline Model Development** - Implement simple regression baseline
3. **Feature Engineering** - Extract features from text and categorical data
4. **Dependency Setup** - Populate requirements.txt with necessary packages

---

## Recent Changes

### Completed
- ✅ Project repository created and connected to GitHub (https://github.com/jum1ko/ds.git)
- ✅ Git configuration set up (user.name: jum1ko, credential helper: osxkeychain)
- ✅ Initial commit made with .gitignore and requirements.txt
- ✅ Data folder added to repository (beer_train.csv, beer_test.csv, sample_submission.csv)
- ✅ Fixed .gitignore to allow data/ folder tracking (removed `*` wildcard that was ignoring everything)
- ✅ README.md created with project overview
- ✅ Memory Bank structure initialized
- ✅ Feature Engineering Hypotheses documented in activeContext.md

### In Progress
- 🔄 Memory Bank files creation (this task)

---

## Active Decisions & Considerations

### Decision 1: Project Structure
**Decision:** Follow planned structure from README.md
- `notebooks/` for exploratory work
- `src/` for production code
- `artifacts/` for saved models
- `submissions/` for Kaggle submissions

**Status:** Structure planned, directories not yet created

**Rationale:** Clear separation between experimentation and production code

### Decision 2: Initial Approach
**Decision:** Start with iterative development (baseline → improvements)

**Status:** Not yet implemented

**Rationale:** Ensures working end-to-end pipeline before optimization

### Decision 3: Text Feature Processing
**Decision:** Need to evaluate best approach for `description` and `name` fields

**Options:**
- Simple TF-IDF
- Keyword extraction (hop-related terms)
- Embeddings (if needed)

**Status:** Decision pending EDA

### Decision 4: Model Selection for Baseline
**Decision:** Start with Linear/Ridge Regression

**Status:** Decision made, not implemented

**Rationale:** Simple, interpretable, fast to train

---

## Current Blockers

### Blocker 1: Empty requirements.txt
**Issue:** No dependencies installed yet

**Impact:** Cannot run any data analysis or modeling code

**Resolution:** Need to populate requirements.txt with initial packages (pandas, numpy, scikit-learn, jupyter)

**Priority:** High - blocks all development

### Blocker 2: No EDA Completed
**Issue:** Haven't explored the data yet

**Impact:** Don't understand data quality, distributions, relationships

**Resolution:** Need to create first EDA notebook

**Priority:** High - needed before feature engineering

---

## Active Questions

1. **Data Quality:** What are the missing value patterns? Any outliers?
2. **Feature Relationships:** How do ABV, SRM, gravity relate to IBU?
3. **Text Content:** What information is in `description` field? Is it useful?
4. **Distribution:** What is the distribution of IBU values? Is it normal?
5. **Categories:** What are the unique values in `glass` and `available` fields?

---

## Feature Engineering Hypotheses

*Гипотезы для экспериментов после реализации базового feature engineering. Тестировать последовательно по приоритету после получения baseline RMSE.*

### Гипотеза 1: Стиль пива как предиктор IBU
**Гипотеза:** Извлечение стиля пива (Stout, Porter, Wheat, Lager) из `name` и `description` улучшит предсказание IBU, так как разные стили имеют характерные диапазоны горечи.

**Реализация:**
- Извлечь стили: ['Stout', 'Porter', 'Wheat', 'Lager', 'Pilsner', 'Saison', 'Belgian', 'Imperial']
- Создать бинарные флаги: `is_stout`, `is_porter`, `is_wheat`
- Создать категориальную переменную `beer_style`

**Метрика проверки:** Feature importance в модели или улучшение RMSE

**Приоритет:** Средний (если базовая модель уже работает хорошо)

---

### Гипотеза 2: Биннинг числовых признаков
**Гипотеза:** Создание категориальных признаков из числовых (бининг ABV, SRM) захватит нелинейные зависимости, которые линейные модели не улавливают.

**Реализация:**
```python
df['abv_binned'] = pd.cut(df['abv'], bins=[0, 4, 5.5, 7, 10, 15], 
                          labels=['Light', 'Standard', 'Strong', 'Very Strong', 'Extreme'])
df['srm_binned'] = pd.cut(df['srm_clean'], bins=[0, 5, 10, 20, 40, 100],
                          labels=['Pale', 'Gold', 'Amber', 'Brown', 'Black'])
```

**Метрика проверки:** Улучшение RMSE в Ridge/Linear моделях

**Приоритет:** Низкий (в первую очередь попробовать для градиентного бустинга)

---

### Гипотеза 3: Логарифмические преобразования
**Гипотеза:** Логарифмирование ABV, SRM, gravity нормализует распределения и улучшит производительность линейных моделей.

**Реализация:**
```python
df['abv_log'] = np.log1p(df['abv'])
df['srm_log'] = np.log1p(df['srm_clean'])
df['gravity_log'] = np.log1p(df['originalGravity'] - 1)
```

**Метрика проверки:** Проверить асимметрию распределений (skewness), затем сравнить RMSE

**Приоритет:** Низкий (проверить только если распределения сильно асимметричны)

---

### Гипотеза 4: Упрощение glass и available
**Гипотеза:** Создание упрощенных категорий для `glass` (топ-5 → остальные "Other") и `available` (Year Round / Seasonal / Limited) улучшит модель, уменьшив размерность без потери сигнала.

**Реализация:**
```python
top_glasses = df['glass'].value_counts().head(5).index.tolist()
df['glass_top'] = df['glass'].apply(lambda x: x if x in top_glasses else 'Other')

df['available_simplified'] = df['available'].apply(lambda x: 
    'Year Round' if 'year round' in str(x).lower() else
    'Seasonal' if any(w in str(x).lower() for w in ['spring', 'summer', 'fall', 'winter']) else
    'Limited'
)
```

**Метрика проверки:** Сравнить feature importance full encoding vs simplified

**Приоритет:** Средний (реализовать после baseline, если есть переобучение)

---

### Гипотеза 5: Метрики длины описания
**Гипотеза:** Длина описания (`description_length`, `description_word_count`) коррелирует со сложностью пива и может быть слабым предиктором IBU.

**Реализация:**
```python
df['description_length'] = df['description'].str.len()
df['description_word_count'] = df['description'].str.split().str.len()
```

**Метрика проверки:** Корреляция с IBU и feature importance

**Приоритет:** Низкий (слабый сигнал, проверить после более важных фичей)

---

### Гипотеза 6: Упоминания процессов варки
**Гипотеза:** Упоминания процессов ("dry hopped", "barrel", "aged", "fermented") указывают на более сложное/специальное пиво с потенциально другим профилем IBU.

**Реализация:**
```python
brewing_keywords = ['dry hopped', 'dry hop', 'barrel', 'aged', 'fermented']
df['brewing_process_mentions'] = df['description'].str.lower().apply(
    lambda x: sum(1 for kw in brewing_keywords if kw in str(x))
)
```

**Метрика проверки:** Проверить распределение IBU для пива с/без этих упоминаний

**Приоритет:** Низкий (экспериментальная фича)

---

### Гипотеза 7: Расширенные взаимодействия
**Гипотеза:** Расширенные взаимодействия (`gravity_srm`, `abv_gravity_ratio`) захватят сложные нелинейные эффекты между компонентами пива.

**Реализация:**
```python
df['gravity_srm'] = df['originalGravity'] * df['srm_clean']
df['abv_gravity_ratio'] = df['abv'] / (df['originalGravity'] - 1)
```

**Метрика проверки:** Feature importance и сравнение RMSE

**Приоритет:** Средний (реализовать после базовых взаимодействий)

---

### Гипотеза 8: Эмоциональные/интенсивные слова
**Гипотеза:** Наличие интенсивных слов ("intense", "bold", "aggressive", "huge") в описании может указывать на высокий IBU.

**Реализация:**
```python
intensity_words = ['intense', 'bold', 'aggressive', 'huge', 'massive', 'big']
df['intensity_score'] = df['description'].str.lower().apply(
    lambda x: sum(1 for word in intensity_words if word in str(x))
)
```

**Метрика проверки:** Корреляция с IBU

**Приоритет:** Низкий (экспериментальная фича, может быть шумом)

---

### Гипотеза 9: TF-IDF из description
**Гипотеза:** TF-IDF векторизация `description` извлечет семантические паттерны, которые простые keyword-based фичи пропускают.

**Реализация:**
```python
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf = TfidfVectorizer(max_features=50, stop_words='english', ngram_range=(1, 2))
description_tfidf = tfidf.fit_transform(df['description'])
```

**Метрика проверки:** Feature importance top TF-IDF фичей, улучшение RMSE

**Приоритет:** Низкий (только если keyword-based фичи недостаточны, увеличивает размерность)

---

### Гипотеза 10: Флаг безалкогольного пива
**Гипотеза:** Безалкогольное пиво (ABV < 1.0) имеет другой профиль IBU из-за производственных процессов.

**Реализация:**
```python
df['is_non_alcoholic'] = (df['abv'] < 1.0).astype(int)
```

**Метрика проверки:** Проверить распределение IBU для non-alcoholic vs regular

**Приоритет:** Средний (реализовать если таких примеров достаточно для статистики)

---

### Процесс проверки гипотез

1. **Базовая реализация:** Сначала реализовать критичные фичи (SRM очистка, hop_mentions_count, базовые взаимодействия)
2. **Baseline метрика:** Получить baseline RMSE с минимальным набором фичей
3. **Итеративное тестирование:** Последовательно добавлять гипотезы по приоритету (Средний → Низкий)
4. **Валидация:** Для каждой гипотезы: добавить фичу → обучить модель → сравнить RMSE
5. **Документация:** Сохранять успешные фичи, удалять неуспешные, документировать результаты

### Статус гипотез

- **Не начато:** Все гипотезы (после реализации базовых фичей)
- **В процессе:** -
- **Проверено, успешно:** -
- **Проверено, неуспешно:** -

---

## Next Session Goals

### Short-term (This Session)
- [ ] Complete Memory Bank initialization
- [ ] Verify all Memory Bank files are created correctly
- [ ] Commit Memory Bank to repository

### Immediate Next Steps (Next Session)
- [ ] Set up dependencies (populate requirements.txt)
- [ ] Create notebooks/ directory
- [ ] Create first EDA notebook
- [ ] Explore data distributions and relationships
- [ ] Identify data quality issues

### Medium-term Goals
- [ ] Build baseline regression model
- [ ] Implement basic feature engineering
- [ ] Evaluate baseline performance
- [ ] Iterate on features and models

---

## Context for Next Developer/AI Session

### Important Files to Read First
1. **memory-bank/projectbrief.md** - Understand overall project goals
2. **memory-bank/systemPatterns.md** - Understand architecture decisions
3. **README.md** - Quick project overview
4. **data/beer_train.csv** - The actual dataset (load and explore)

### Key Information
- **Target Variable:** `ibu` (only in beer_train.csv)
- **Evaluation Metric:** RMSE (lower is better)
- **Approach:** Iterative - start simple, improve incrementally
- **Current Phase:** Initial setup and EDA

### Assumptions to Verify
- Training and test data have same feature distributions
- No data leakage between train/test
- All features are available for prediction (no missing in test set)

---

## Active Patterns & Practices

### Code Organization
- Exploratory work → `notebooks/`
- Reusable code → `src/`
- Keep notebooks for documentation, move proven code to src/

### Development Workflow
1. Explore in notebook
2. Extract working code to src/
3. Test and validate
4. Iterate

### Git Practices
- Commit logical units (EDA, feature engineering, model training)
- Don't commit large model files (use .gitignore)
- Keep commits focused and documented

---

## Recent Insights

### Data Observation (From README)
- Mixed data types require different preprocessing approaches
- Text features (`description`, `name`) may contain valuable signals
- Categorical features need encoding strategy
- Numerical features may benefit from scaling

### Technical Insight
- Need to handle potential missing values in features
- Feature interactions might be important (e.g., ABV × gravity)
- Domain knowledge suggests relationships: hops → IBU, malt → SRM

---

## Reminders for Next Steps

1. **Don't skip EDA** - Understanding data is critical before modeling
2. **Start with baseline** - Get end-to-end pipeline working first
3. **Document decisions** - Update Memory Bank as patterns emerge
4. **Keep it simple initially** - Complex models can come later
5. **Track experiments** - Log what works and what doesn't
