# 🚢 Titanic Survival Prediction Using Scikit-Learn Pipeline

## 📌 Overview
This project builds a **Machine Learning Pipeline** for predicting passenger survival on the Titanic using Scikit-Learn. The pipeline efficiently handles **data preprocessing, imputation, encoding**, and **model selection** with **GridSearchCV**.

## 🏗️ Pipeline Components
### **1️⃣ ColumnTransformer (`trf1`)**
The `ColumnTransformer` preprocesses data by:
- **Handling missing values**
  - `SimpleImputer()` replaces missing values in `Age` with the mean.
  - `SimpleImputer(strategy='most_frequent')` fills missing values in `Embarked`.
- **Encoding categorical variables**
  - `OneHotEncoder(handle_unknown='ignore')` encodes `Sex` and `Embarked` features into numerical format.

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

trf1 = ColumnTransformer([
    ('impute_age', SimpleImputer(), ['Age']),
    ('impute_embarked', SimpleImputer(strategy='most_frequent'), ['Embarked']),
    ('encode_categorical', OneHotEncoder(handle_unknown='ignore'), ['Sex', 'Embarked'])
], remainder='passthrough')
```

### **2️⃣ Creating a Pipeline**
The pipeline integrates preprocessing (`trf1`) with a classifier, such as `RandomForestClassifier`.

```python
from sklearn.pipeline import Pipeline
from sklearn.ensemble import RandomForestClassifier

pipe = Pipeline([
    ('transform', trf1),
    ('model', RandomForestClassifier())
])
```

### **3️⃣ Hyperparameter Tuning with `GridSearchCV`**
We optimize the model using `GridSearchCV` to find the best hyperparameters.

```python
from sklearn.model_selection import GridSearchCV

params = {
    'model__n_estimators': [100, 200],
    'model__max_depth': [None, 10, 20]
}

grid = GridSearchCV(pipe, params, cv=5, scoring='accuracy')
grid.fit(X_train, y_train)
```

## 🚀 How to Run
1. Install dependencies:  
   ```bash
   pip install -r requirements.txt
   ```
2. Load and preprocess the dataset.
3. Train the pipeline using `GridSearchCV`.
4. Evaluate the model performance.

## 📊 Expected Output
After training, the pipeline will output:
- **Best hyperparameters** found by `GridSearchCV`
- **Accuracy score** on validation data

## 📜 Conclusion
This pipeline automates the Titanic survival prediction process with minimal manual intervention, ensuring efficient preprocessing and model tuning.

---
🚀 **Happy Coding!**

