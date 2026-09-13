# Scikit-Learn Pipeline — Titanic Survival Prediction

A hands-on Machine Learning project demonstrating how to build an **end-to-end Scikit-Learn Pipeline** for the Titanic Survival Prediction problem.

The project covers two approaches:

1. **Prediction without a Pipeline** — preprocessing and prediction are performed manually as separate steps.
2. **Prediction using a Pipeline** — preprocessing, feature selection, and model training are combined into a single Scikit-Learn pipeline.

The main purpose of this project is to understand how Scikit-Learn `Pipeline`, `ColumnTransformer`, preprocessing techniques, feature selection, cross-validation, and hyperparameter tuning work together.

---

## 📌 Project Objective

The objective is to build a classification model that takes the following passenger information:

* `Pclass`
* `Sex`
* `Age`
* `SibSp`
* `Parch`
* `Fare`
* `Embarked`

and predicts whether the passenger **survived or did not survive**.

### Target

```text
Survived

0 → Did not survive
1 → Survived
```

---

## 📂 Dataset

The project uses the Titanic `train.csv` dataset.

The original dataset contains the following columns:

```text
PassengerId
Survived
Pclass
Name
Sex
Age
SibSp
Parch
Ticket
Fare
Cabin
Embarked
```

During preprocessing, the following columns are removed:

```text
PassengerId
Name
Ticket
Cabin
```

The remaining features used for prediction are:

```text
Pclass
Sex
Age
SibSp
Parch
Fare
Embarked
```

---

# 🔄 Machine Learning Workflow

The project follows this workflow:

```text
Titanic Dataset
       │
       ▼
Remove Unnecessary Columns
       │
       ▼
Train / Test Split
       │
       ▼
Handle Missing Values
       │
       ▼
One-Hot Encoding
       │
       ▼
Min-Max Scaling
       │
       ▼
Feature Selection
       │
       ▼
Decision Tree Classifier
       │
       ▼
Prediction
       │
       ▼
Model Evaluation
```

---

# 🧹 1. Data Preprocessing

After loading the dataset, unnecessary columns are removed:

```python
df.drop(
    columns=['PassengerId', 'Name', 'Ticket', 'Cabin'],
    inplace=True
)
```

The data is then divided into training and testing sets using:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

---

# 🩹 2. Handling Missing Values

The dataset contains missing values in:

* `Age`
* `Embarked`

The project uses `SimpleImputer` to handle these missing values.

### Age

The default `SimpleImputer` strategy is used for `Age`:

```python
SimpleImputer()
```

The fitted pipeline calculates the mean age as approximately:

```text
29.49884615
```

### Embarked

For `Embarked`, the most frequent value is used:

```python
SimpleImputer(strategy='most_frequent')
```

The most frequent value found by the fitted transformer is:

```text
S
```

---

# 🔤 3. One-Hot Encoding

The categorical features:

```text
Sex
Embarked
```

are converted into numerical features using:

```python
OneHotEncoder(
    sparse_output=False,
    handle_unknown='ignore'
)
```

Using:

```text
handle_unknown='ignore'
```

allows the transformer to handle categories that were not present during training.

---

# 📏 4. Feature Scaling

After encoding, the project applies:

```python
MinMaxScaler()
```

to the transformed features.

The scaler converts the feature values into a common range.

The pipeline applies `MinMaxScaler` to the first 10 transformed columns.

---

# 🎯 5. Feature Selection

The project uses:

```python
SelectKBest(
    score_func=chi2,
    k=8
)
```

The `chi2` statistical test is used as the scoring function.

The initial pipeline selects the **8 best features** before passing them to the classifier.

---

# 🌳 6. Decision Tree Classifier

The final Machine Learning model is:

```python
DecisionTreeClassifier()
```

The Decision Tree receives the selected features and performs the final classification.

The complete pipeline is:

```text
ColumnTransformer
      ↓
Missing Value Imputation
      ↓
ColumnTransformer
      ↓
One-Hot Encoding
      ↓
ColumnTransformer
      ↓
MinMaxScaler
      ↓
SelectKBest + chi2
      ↓
DecisionTreeClassifier
```

The fitted pipeline contains these five stages:

```text
columntransformer-1
columntransformer-2
columntransformer-3
selectkbest
decisiontreeclassifier
```

---

# 🔗 7. Creating the Pipeline

The pipeline combines all preprocessing and modeling steps:

```python
pipe = Pipeline([
    ('trf1', trf1),
    ('trf2', trf2),
    ('trf3', trf3),
    ('trf4', trf4),
    ('trf5', trf5)
])
```

The project also demonstrates the alternative `make_pipeline()` syntax:

```python
pipe = make_pipeline(
    trf1,
    trf2,
    trf3,
    trf4,
    trf5
)
```

### Pipeline vs make_pipeline

The project demonstrates that:

* `Pipeline` requires explicit names for the steps.
* `make_pipeline` automatically generates the step names.

---

# 🏋️ 8. Model Training

The complete pipeline is trained using:

```python
pipe.fit(X_train, y_train)
```

Once fitted, the pipeline contains the preprocessing transformations and trained Decision Tree model together.

---

# 🔍 9. Exploring the Pipeline

The project demonstrates how to inspect individual pipeline steps:

```python
pipe.named_steps
```

This allows you to see the transformers and model contained inside the pipeline.

For example, the fitted imputer can be inspected using:

```python
pipe.named_steps[
    'columntransformer-1'
].transformers_[0][1].statistics_
```

---

# 📊 10. Model Evaluation

Predictions are generated using:

```python
y_pred = pipe.predict(X_test)
```

The test-set accuracy obtained in the notebook is:

```text
0.6256983240223464
```

or approximately:

```text
62.57%
```

> Note: this README reports the result produced by the uploaded notebook. It does not replace or modify the experiment results.

---

# 🔁 11. Cross-Validation

The project also demonstrates cross-validation directly on the pipeline using:

```python
cross_val_score(
    pipe,
    X_train,
    y_train,
    cv=5,
    scoring='accuracy'
).mean()
```

The recorded mean cross-validation accuracy is approximately:

```text
0.6391214419383433
```

or:

```text
63.91%
```

This demonstrates that the complete preprocessing and model workflow can be evaluated together during cross-validation.

---

# ⚙️ 12. GridSearchCV

The project uses `GridSearchCV` to experiment with different pipeline parameters.

The parameter grid contains:

```python
params = {
    'selectkbest__k': [1, 2, 3, 4, 5, 6, 7, 8],
    'decisiontreeclassifier__max_depth': [1, 2, 3, 4, 5]
}
```

This searches combinations of:

### SelectKBest

```text
k = 1 → 8
```

### Decision Tree

```text
max_depth = 1 → 5
```

Grid search is performed using:

```python
grid = GridSearchCV(
    pipe,
    params,
    cv=5,
    scoring='accuracy'
)

grid.fit(X_train, y_train)
```

The recorded best cross-validation score is:

```text
0.6391214419383433
```

approximately:

```text
63.91%
```

The recorded best parameters are:

```python
{
    'decisiontreeclassifier__max_depth': 1,
    'selectkbest__k': 1
}
```

---

# 💾 13. Exporting the Pipeline

After training, the complete pipeline is saved using Python's `pickle` module:

```python
import pickle

pickle.dump(
    pipe,
    open('pipe.pkl', 'wb')
)
```

The repository therefore contains the serialized pipeline:

```text
pipe.pkl
```

This allows the trained preprocessing and model workflow to be loaded later without rebuilding every preprocessing step manually.

---

# 🔮 14. Prediction Using the Pipeline

The saved pipeline can be loaded with:

```python
import pickle

pipe = pickle.load(
    open('pipe.pkl', 'rb')
)
```

A new passenger is represented as a Pandas DataFrame:

```python
test_input = pd.DataFrame(
    [[2, 'male', 31.0, 0, 0, 10.5, 'S']],
    columns=[
        'Pclass',
        'Sex',
        'Age',
        'SibSp',
        'Parch',
        'Fare',
        'Embarked'
    ]
)
```

Prediction can then be performed directly:

```python
pipe.predict(test_input)
```

For the example passenger, the recorded prediction is:

```text
[0]
```

meaning:

```text
Passenger did not survive
```

---

# 🔀 15. Prediction Without a Pipeline

The project also contains a separate notebook showing how prediction can be performed manually without using a complete pipeline.

In this approach, the individual preprocessing objects are loaded separately:

```python
ohe_sex = pickle.load(
    open('models/ohe_sex.pkl', 'rb')
)

ohe_embarked = pickle.load(
    open('models/ohe_embarked.pkl', 'rb')
)

clf = pickle.load(
    open('models/clf.pkl', 'rb')
)
```

The input is manually transformed through:

```text
Raw Input
   ↓
Sex Encoding
   ↓
Embarked Encoding
   ↓
Feature Arrangement
   ↓
Classifier
   ↓
Prediction
```

For the same example input:

```text
[2, 'male', 31.0, 0, 0, 10.5, 'S']
```

the recorded prediction is:

```text
[0]
```

---

# 🆚 Pipeline vs Without Pipeline

| Without Pipeline                                 | With Pipeline                                     |
| ------------------------------------------------ | ------------------------------------------------- |
| Preprocessing is performed manually              | Preprocessing is combined                         |
| Multiple objects must be managed                 | One pipeline object can be managed                |
| Input transformations must be performed manually | `pipe.predict()` handles transformations          |
| More preprocessing code                          | More compact inference workflow                   |
| Encoders and classifier are stored separately    | Complete workflow can be serialized as `pipe.pkl` |

The project demonstrates both approaches to make the difference between manual preprocessing and an integrated Scikit-Learn pipeline clear.

---

# 📁 Project Files

The project contains notebooks and supporting files for the different stages of the workflow.

```text
sklearn-pipeline/
│
├── titanic-using-pipeline.ipynb
│
├── predict-using-pipeline.ipynb
│
├── Predict-without-pipeline.ipynb
│
├── pipe.pkl
│
├── train.csv
│
└── models/
    ├── ohe_sex.pkl
    ├── ohe_embarked.pkl
    └── clf.pkl
```

### File Description

| File                             | Description                                                                                             |
| -------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `titanic-using-pipeline.ipynb`   | Complete Titanic preprocessing, pipeline creation, evaluation, cross-validation and GridSearch workflow |
| `predict-using-pipeline.ipynb`   | Loads `pipe.pkl` and performs prediction directly                                                       |
| `Predict-without-pipeline.ipynb` | Demonstrates manual preprocessing and prediction without a complete pipeline                            |
| `pipe.pkl`                       | Serialized trained Scikit-Learn pipeline                                                                |
| `train.csv`                      | Titanic training dataset                                                                                |
| `models/`                        | Separately saved preprocessing encoders and classifier used by the non-pipeline approach                |

---

# 🛠️ Technologies Used

* Python
* NumPy
* Pandas
* Scikit-Learn
* Jupyter Notebook
* Pickle

### Main Scikit-Learn Components

```text
train_test_split
ColumnTransformer
SimpleImputer
OneHotEncoder
MinMaxScaler
Pipeline
make_pipeline
SelectKBest
chi2
DecisionTreeClassifier
cross_val_score
GridSearchCV
accuracy_score
```

---

# 🚀 How to Run

## 1. Clone the Repository

```bash
git clone https://github.com/priyanshulink/Machine_Learning_fundamental.git
```

## 2. Navigate to the Pipeline Directory

```bash
cd Machine_Learning_fundamental/sklearn-pipeline
```

## 3. Install Required Libraries

```bash
pip install numpy pandas scikit-learn jupyter
```

## 4. Start Jupyter Notebook

```bash
jupyter notebook
```

## 5. Run the Main Notebook

Open:

```text
titanic-using-pipeline.ipynb
```

Run the notebook from beginning to end to reproduce the preprocessing, pipeline creation, model training, evaluation, cross-validation, and GridSearch experiments.

---

# 📚 What I Learned

This project demonstrates practical use of:

* Data preprocessing
* Missing-value handling
* Categorical encoding
* Feature scaling
* Feature selection
* Decision Tree classification
* Scikit-Learn Pipeline
* ColumnTransformer
* `Pipeline` vs `make_pipeline`
* Pipeline inspection using `named_steps`
* Cross-validation
* GridSearchCV
* Hyperparameter tuning
* Model serialization with Pickle
* Prediction using a saved pipeline
* Difference between manual preprocessing and pipeline-based prediction

---

# 🎯 Key Takeaway

The main concept demonstrated by this project is that preprocessing and machine learning steps can be organized into a single reusable workflow.

Instead of manually performing:

```text
Imputation
   ↓
Encoding
   ↓
Scaling
   ↓
Feature Selection
   ↓
Prediction
```

the pipeline allows the complete process to be represented as:

```python
pipe.predict(test_input)
```

This makes the prediction workflow simpler because the saved `pipe.pkl` contains the complete fitted transformation and classification workflow.

---

# 👨‍💻 Author

**Priyanshu Singh**

GitHub:

https://github.com/priyanshulink

Repository:

https://github.com/priyanshulink/Machine_Learning_fundamental
