# Day 16 — ColumnTransformer, Missing Values & Encoding

Today I learned how to preprocess data before giving it to a Machine Learning model.

## Topics Covered

- Train/Test Split
- Missing Value Imputation
- Ordinal Encoding
- One-Hot Encoding
- Label Encoding
- Preprocessing without ColumnTransformer
- Preprocessing with ColumnTransformer
- Why ColumnTransformer is needed
- `fit_transform()` vs `transform()`
- Data Leakage

---

# 1. Why Do We Need Data Preprocessing?

Machine Learning algorithms usually cannot directly understand categorical data such as:

```text
gender = Male / Female
city = Delhi / Mumbai / Kolkata
cough = Mild / Strong
```

We need to convert categorical data into numerical form.

We may also have missing values:

```text
fever
-----
103
NaN
101
98
NaN
```

These missing values need to be handled before training the model.

Therefore, **data preprocessing** is required.

---

# 2. Dataset

The dataset used today is:

```text
covid_toy.csv
```

Columns:

```text
age
gender
fever
cough
city
has_covid
```

Example:

| age | gender | fever | cough | city | has_covid |
|---:|---|---:|---|---|---|
| 60 | Male | 103.0 | Mild | Kolkata | No |
| 27 | Male | 100.0 | Mild | Delhi | Yes |
| 42 | Male | 101.0 | Mild | Delhi | No |
| 31 | Female | 98.0 | Mild | Kolkata | No |
| 65 | Female | 101.0 | Mild | Mumbai | No |

Column types:

| Column | Type | Preprocessing |
|---|---|---|
| `age` | Numerical | No encoding |
| `fever` | Numerical | Missing value imputation |
| `cough` | Categorical + ordered | Ordinal Encoding |
| `gender` | Categorical | One-Hot Encoding |
| `city` | Categorical | One-Hot Encoding |
| `has_covid` | Target | Label Encoding if required |

---

# 3. Train/Test Split

First separate features and target.

```python
X = df.drop(columns=['has_covid'])
y = df['has_covid']
```

Then split the data:

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=2
)
```

---

# 4. Which Encoding Should I Use?

The basic rule is:

```text
             Categorical Column
                     |
          Does it have an order?
             /             \
           YES              NO
            |                |
      Ordinal Encoding     OHE
```

For the target:

```text
Categorical Target
        |
        ↓
Label Encoding
```

## One-Hot Encoding

Use OHE when categories have **no meaningful order**.

Example:

```text
city:

Delhi
Mumbai
Kolkata
```

There is no:

```text
Delhi < Mumbai < Kolkata
```

Therefore:

```python
OneHotEncoder()
```

is appropriate.

---

## Ordinal Encoding

Use Ordinal Encoding when categories have a meaningful order.

Example:

```text
cough:

Mild < Strong
```

Therefore:

```python
OrdinalEncoder(
    categories=[['Mild', 'Strong']]
)
```

Encoding:

```text
Mild   → 0
Strong → 1
```

Other examples:

```text
Poor < Average < Good < Excellent
```

```text
Small < Medium < Large
```

---

## Label Encoding

Label Encoding is commonly used for a categorical target.

Example:

```text
has_covid:

No  → 0
Yes → 1
```

Do not blindly use Label Encoding for categorical feature columns such as:

```text
Delhi   → 0
Mumbai  → 1
Kolkata → 2
```

because this can create an artificial numerical order.

---

# 5. Missing Value Imputation

The `fever` column contains missing values.

We can use `SimpleImputer`.

```python
from sklearn.impute import SimpleImputer

si = SimpleImputer()

X_train_fever = si.fit_transform(
    X_train[['fever']]
)

X_test_fever = si.transform(
    X_test[['fever']]
)
```

Important:

```text
Training Data → fit_transform()
Test Data     → transform()
```

---

# 6. WITHOUT ColumnTransformer

Before using ColumnTransformer, we can preprocess every column separately.

## Step 1 — Impute fever

```python
si = SimpleImputer()

X_train_fever = si.fit_transform(
    X_train[['fever']]
)

X_test_fever = si.transform(
    X_test[['fever']]
)
```

## Step 2 — Ordinal Encoding for cough

```python
from sklearn.preprocessing import OrdinalEncoder

oe = OrdinalEncoder(
    categories=[['Mild', 'Strong']]
)

X_train_cough = oe.fit_transform(
    X_train[['cough']]
)

X_test_cough = oe.transform(
    X_test[['cough']]
)
```

## Step 3 — One-Hot Encoding for gender and city

```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(
    drop='first',
    sparse_output=False
)

X_train_gender_city = ohe.fit_transform(
    X_train[['gender', 'city']]
)

X_test_gender_city = ohe.transform(
    X_test[['gender', 'city']]
)
```

## Step 4 — Extract age

`age` is already numerical.

Therefore, no encoding is required.

```python
X_train_age = X_train[['age']].values

X_test_age = X_test[['age']].values
```

## Step 5 — Combine Everything

Now we have:

```text
X_train_age
X_train_fever
X_train_cough
X_train_gender_city
```

We need to combine them.

```python
import numpy as np

X_train_transformed = np.concatenate(
    (
        X_train_age,
        X_train_fever,
        X_train_cough,
        X_train_gender_city
    ),
    axis=1
)

X_test_transformed = np.concatenate(
    (
        X_test_age,
        X_test_fever,
        X_test_cough,
        X_test_gender_city
    ),
    axis=1
)
```

---

# 7. Problems With Manual Preprocessing

Manual preprocessing works, but it becomes difficult when there are many columns.

For example:

```text
100 columns
      ↓
20 columns need imputation
30 columns need OHE
10 columns need Ordinal Encoding
40 columns are numerical
      ↓
Many separate transformations
      ↓
Manually combine everything
```

This creates problems:

- More code
- Difficult to maintain
- More chances of mistakes
- Need to manually combine arrays
- Need to remember which transformer belongs to which column
- Difficult to build a clean ML pipeline

This is why we use:

# ColumnTransformer

---

# 8. What Is ColumnTransformer?

`ColumnTransformer` is a scikit-learn tool that allows us to apply **different preprocessing techniques to different columns**.

In simple words:

> **ColumnTransformer tells Python which transformation should be applied to which column.**

For example:

```text
fever
  ↓
SimpleImputer

cough
  ↓
OrdinalEncoder

gender + city
  ↓
OneHotEncoder

age
  ↓
passthrough
```

All of these transformations can be defined together.

---

# 9. Why Do We Need ColumnTransformer?

Different columns require different preprocessing.

For our dataset:

```text
age
```

is already numerical.

```text
fever
```

is numerical but has missing values.

```text
cough
```

is categorical and ordered.

```text
gender
city
```

are categorical without meaningful order.

Therefore:

```text
Column       Transformation
--------------------------------
age          passthrough
fever        SimpleImputer
cough        OrdinalEncoder
gender       OneHotEncoder
city         OneHotEncoder
```

ColumnTransformer allows us to handle all of this in one object.

---

# 10. WITH ColumnTransformer

Import it:

```python
from sklearn.compose import ColumnTransformer
```

Create the transformer:

```python
transformer = ColumnTransformer(
    transformers=[
        (
            'fever_imputer',
            SimpleImputer(),
            ['fever']
        ),

        (
            'cough_ordinal',
            OrdinalEncoder(
                categories=[['Mild', 'Strong']]
            ),
            ['cough']
        ),

        (
            'gender_city_ohe',
            OneHotEncoder(
                drop='first',
                sparse_output=False
            ),
            ['gender', 'city']
        )
    ],
    
    remainder='passthrough'
)
```

---

# 11. Understanding ColumnTransformer

This:

```python
(
    'fever_imputer',
    SimpleImputer(),
    ['fever']
)
```

means:

```text
Column:
fever

Transformation:
SimpleImputer
```

This:

```python
(
    'cough_ordinal',
    OrdinalEncoder(
        categories=[['Mild', 'Strong']]
    ),
    ['cough']
)
```

means:

```text
Column:
cough

Transformation:
OrdinalEncoder
```

This:

```python
(
    'gender_city_ohe',
    OneHotEncoder(
        drop='first',
        sparse_output=False
    ),
    ['gender', 'city']
)
```

means:

```text
Columns:
gender
city

Transformation:
OneHotEncoder
```

---

# 12. What Is `remainder='passthrough'`?

We explicitly transform:

```text
fever
cough
gender
city
```

But what about:

```text
age
```

`age` is already numerical, so we don't need to transform it.

Therefore:

```python
remainder='passthrough'
```

means:

> Keep the columns that were not explicitly transformed.

So:

```text
age → unchanged
```

Without:

```python
remainder='passthrough'
```

the remaining columns would normally be dropped.

---

# 13. Fit and Transform

For training data:

```python
X_train_transformed = transformer.fit_transform(
    X_train
)
```

For test data:

```python
X_test_transformed = transformer.transform(
    X_test
)
```

The rule is:

```text
TRAIN DATA
    ↓
fit_transform()
```

```text
TEST DATA
    ↓
transform()
```

---

# 14. Why Don't We Fit on Test Data?

Wrong:

```python
X_train_transformed = transformer.fit_transform(X_train)

X_test_transformed = transformer.fit_transform(X_test)
```

Correct:

```python
X_train_transformed = transformer.fit_transform(X_train)

X_test_transformed = transformer.transform(X_test)
```

Why?

Because `fit()` means:

> Learn something from the data.

For example:

- Imputer learns the value used to fill missing data.
- Encoder learns categories.
- Scaler learns mean and standard deviation.

If we fit on test data, the preprocessing learns information from the test set.

This is called:

# Data Leakage

We want the test data to behave like **unseen data**.

Therefore:

```text
Training Data
     ↓
     fit()
     ↓
Learn preprocessing rules
     ↓
transform training data
```

Then:

```text
Test Data
     ↓
Use already learned rules
     ↓
transform()
```

---

# 15. Complete ColumnTransformer Code

```python
import numpy as np
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, OrdinalEncoder
from sklearn.compose import ColumnTransformer


# Load dataset
df = pd.read_csv('covid_toy.csv')


# Separate X and y
X = df.drop(columns=['has_covid'])
y = df['has_covid']


# Train/Test Split
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=2
)


# Create ColumnTransformer
transformer = ColumnTransformer(
    transformers=[
        
        # fever → Missing Value Imputation
        (
            'fever_imputer',
            SimpleImputer(),
            ['fever']
        ),

        # cough → Ordinal Encoding
        (
            'cough_ordinal',
            OrdinalEncoder(
                categories=[['Mild', 'Strong']]
            ),
            ['cough']
        ),

        # gender + city → One-Hot Encoding
        (
            'gender_city_ohe',
            OneHotEncoder(
                drop='first',
                sparse_output=False
            ),
            ['gender', 'city']
        )
    ],

    # age → keep unchanged
    remainder='passthrough'
)


# Fit and transform training data
X_train_transformed = transformer.fit_transform(
    X_train
)


# Transform test data
X_test_transformed = transformer.transform(
    X_test
)


# Check shape
print("X_train shape:", X_train_transformed.shape)
print("X_test shape:", X_test_transformed.shape)
```

---

# 16. WITHOUT vs WITH ColumnTransformer

| Without ColumnTransformer | With ColumnTransformer |
|---|---|
| More code | Less code |
| Transform columns separately | Define transformations together |
| Manually combine arrays | Automatically combines results |
| More chances of mistakes | More organized |
| Harder with many columns | Easy with many columns |
| Good for learning | Better for practical ML workflows |

---

# 17. Visual Understanding

## Without ColumnTransformer

```text
             DATA
               |
      ┌────────┼────────┐
      ↓        ↓        ↓
    fever    cough    gender/city
      ↓        ↓        ↓
  Imputer   Ordinal    OHE
      ↓        ↓        ↓
      └────────┼────────┘
               ↓
         concatenate()
               ↓
        Final transformed X
```

## With ColumnTransformer

```text
                 DATA
                   |
                   ↓
          ColumnTransformer
                   |
       ┌───────────┼───────────┐
       ↓           ↓           ↓
     fever       cough     gender/city
       ↓           ↓           ↓
   Imputer      Ordinal        OHE
       │           │           │
       └───────────┼───────────┘
                   ↓
          age → passthrough
                   ↓
          Final transformed X
```

---

# 18. Key Takeaways

### Encoding

```text
No meaningful order
        ↓
One-Hot Encoding
```

```text
Meaningful order
        ↓
Ordinal Encoding
```

```text
Categorical target
        ↓
Label Encoding
```

### Missing Values

```text
Missing values
      ↓
SimpleImputer
```

### ColumnTransformer

```text
ColumnTransformer
       ↓
Different transformations
       ↓
Different columns
       ↓
Combined automatically
```

### Most Important Rule

```text
X_train → fit_transform()

X_test → transform()
```

Never fit the preprocessing object separately on test data.

---

# 19. What I Learned Today

Today I learned that different columns can require different preprocessing techniques.

I learned:

- How to handle missing values using `SimpleImputer`
- How to use One-Hot Encoding
- How to use Ordinal Encoding
- When Label Encoding is useful
- How to preprocess data without ColumnTransformer
- Problems with manual preprocessing
- What `ColumnTransformer` is
- Why `ColumnTransformer` is needed
- How to apply different transformations to different columns
- How `remainder='passthrough'` works
- Difference between `fit_transform()` and `transform()`
- What data leakage is
- Why preprocessing should be learned only from training data
