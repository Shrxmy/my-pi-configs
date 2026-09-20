---
name: "machine-learning-expert"
description: "Expert in machine learning algorithms, model training, hyperparameter tuning, and production ML pipelines. Use when building classification/regression models, tuning hyperparameters, evaluating model performance, or implementing ML workflows. Covers scikit-learn, XGBoost, LightGBM, PyTorch, TensorFlow, and MLOps practices."
compatibility: pi
---

# Machine Learning Expert

Comprehensive ML expertise from model selection to production deployment.

## Core Workflows

### 1. Data Preparation

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split

# Load and prepare data
df = pd.read_csv('data.csv')
X = df.drop('target', axis=1)
y = df['target']

# Handle missing values
X = X.fillna(X.median())

# Encode categorical variables
X = pd.get_dummies(X, columns=['category'], drop_first=True)

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

### 2. Model Training

```python
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from xgboost import XGBClassifier
from sklearn.linear_model import LogisticRegression

# Baseline models
models = {
    'logistic': LogisticRegression(max_iter=1000),
    'rf': RandomForestClassifier(n_estimators=100),
    'xgb': XGBClassifier(n_estimators=100, use_label_encoder=False, eval_metric='logloss')
}

# Train and evaluate
for name, model in models.items():
    model.fit(X_train, y_train)
    score = model.score(X_test, y_test)
    print(f"{name}: {score:.4f}")
```

### 3. Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV

# Grid search
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [3, 5, 7],
    'learning_rate': [0.01, 0.1, 0.2]
}

grid_search = GridSearchCV(
    XGBClassifier(),
    param_grid,
    cv=5,
    scoring='roc_auc',
    n_jobs=-1
)
grid_search.fit(X_train, y_train)

print(f"Best params: {grid_search.best_params_}")
print(f"Best score: {grid_search.best_score_:.4f}")
```

### 4. Model Evaluation

```python
from sklearn.metrics import (accuracy_score, precision_score, recall_score,
                            f1_score, roc_auc_score, confusion_matrix,
                            classification_report)

y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]

# Metrics
print(f"Accuracy: {accuracy_score(y_test, y_pred):.4f}")
print(f"Precision: {precision_score(y_test, y_pred):.4f}")
print(f"Recall: {recall_score(y_test, y_pred):.4f}")
print(f"F1: {f1_score(y_test, y_pred):.4f}")
print(f"ROC-AUC: {roc_auc_score(y_test, y_prob):.4f}")

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)
```

### 5. Feature Importance

```python
import matplotlib.pyplot as plt

# Feature importance
feature_importance = pd.DataFrame({
    'feature': X.columns,
    'importance': model.feature_importances_
}).sort_values('importance', ascending=False)

# Plot
plt.figure(figsize=(10, 6))
plt.barh(feature_importance['feature'][:20],
         feature_importance['importance'][:20])
plt.xlabel('Importance')
plt.title('Feature Importance')
plt.tight_layout()
plt.show()
```

### 6. Cross-Validation

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# Stratified k-fold
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Cross-validation scores
cv_scores = cross_val_score(model, X, y, cv=skf, scoring='roc_auc')
print(f"CV AUC: {cv_scores.mean():.4f} (+/- {cv_scores.std() * 2:.4f})")
```

## Model Selection Guide

| Problem Type | Recommended Models |
|--------------|-------------------|
| Binary classification | XGBoost, LightGBM, Logistic Regression |
| Multi-class | Random Forest, XGBoost, Neural Networks |
| Regression | XGBoost, Ridge, ElasticNet |
| Imbalanced data | XGBoost with scale_pos_weight, SMOTE |
| High-dimensional | ElasticNet, SVM with RBF kernel |

## Pipeline Best Practices

1. **Always split data** before any transformations
2. **Use pipelines** for reproducibility:
```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('model', XGBClassifier())
])
```

3. **Save models** with joblib:
```python
import joblib
joblib.dump(model, 'model.joblib')
```

4. **Log experiments** with MLflow:
```python
import mlflow
mlflow.xgboost.autolog()
```

## Quick Reference

```python
# Essential imports
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.metrics import accuracy_score, roc_auc_score

# Quick model training
model = XGBClassifier(n_estimators=100, learning_rate=0.1)
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```