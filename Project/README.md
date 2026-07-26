# Logistic Regression Classification

A simple Machine Learning classification project built using **Scikit-learn**. This project demonstrates the complete ML workflow, including data preprocessing, train-test splitting, model training, prediction, and evaluation using Logistic Regression.

---

##  Project Overview

This project covers the following Machine Learning steps:

- Importing required libraries
- Loading the dataset
- Exploratory Data Analysis (EDA)
- Feature selection
- Train-Test Split
- Training a Logistic Regression model
- Making predictions
- Evaluating model performance

---

##  Technologies Used

- Python
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- Google Colab / Jupyter Notebook

---

##  Libraries Used

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score
```

---

##  Project Workflow

### 1. Import Libraries

Import all required Python libraries.

### 2. Load Dataset

Load the dataset using Pandas.

```python
df = pd.read_csv("placement.csv")
```

---

### 3. Data Exploration

- View dataset
- Check shape
- Check missing values
- Statistical summary

```python
df.head()
df.info()
```

---

### 4. Split Features and Target

```python
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
```

---

### 5. Train-Test Split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    x_train,
    y_train,
    test_size=0.1,
)
```

---

### 6. Create Logistic Regression Model

```python
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
```

---

### 7. Train the Model

```python
clf.fit(X_train, y_train)
```

---

### 8. Make Predictions

```python
y_pred = clf.predict(X_test)
```

---

### 9. Evaluate the Model

```python
from sklearn.metrics import accuracy_score

accuracy = accuracy_score(y_test, y_pred)
print(accuracy)
```

---

## 📈 Model Performance

| Metric | Value |
|----------|--------|
| Accuracy | **1.00 (100%)** |

> The model achieved **100% accuracy** on the test dataset.

---

##  Project Structure

```
Logistic-Regression/
│
├── LogisticRegression.ipynb
├── README.md
└── placement.csv
```

---

##  How to Run

1. Clone the repository

```bash
git clone https://github.com/your-username/Logistic-Regression.git
```

2. Install dependencies

```bash
pip install numpy pandas matplotlib scikit-learn
```

3. Open Jupyter Notebook or Google Colab.

4. Run all notebook cells.

---

##  Machine Learning Pipeline

```
Dataset
   │
   ▼
Data Preprocessing
   │
   ▼
Train-Test Split
   │
   ▼
Model Training
   │
   ▼
Prediction
   │
   ▼
Accuracy Evaluation
```

---

##  Concepts Covered

- Classification
- Logistic Regression
- Train-Test Split
- Model Training
- Prediction
- Accuracy Score
- Scikit-learn

---

##  Future Improvements

- Feature Scaling
- Cross Validation
- Hyperparameter Tuning
- Confusion Matrix
- Precision, Recall, and F1 Score
- ROC Curve & AUC
- Model Deployment using Flask/FastAPI

---

##  Author

**Priyanshu Singh**

GitHub: https://github.com/priyanshulink

---

## ⭐ If you found this project helpful, consider giving it a star!
