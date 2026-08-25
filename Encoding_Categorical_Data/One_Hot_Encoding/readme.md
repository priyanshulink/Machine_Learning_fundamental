One-Hot Encoding

Overview

This notebook covers One-Hot Encoding using Pandas and Scikit-Learn, plus handling categorical columns with many categories.
# One-Hot Encoding

## Topics Covered

- One-Hot Encoding using Pandas
- `pd.get_dummies()`
- One-Hot Encoding using Scikit-Learn
- `OneHotEncoder`
- `drop='first'`
- `sparse_output=False`
- `fit_transform()` and `transform()`
- Combining encoded and numerical features
- High-cardinality categorical data
- Threshold-based rare-category handling
- Replacing rare categories with `uncommon`

---

# 1. One-Hot Encoding

One-Hot Encoding converts categorical values into numerical columns.

For example:

### Before Encoding

| Fuel |
|---|
| Petrol |
| Diesel |
| CNG |

### After Encoding

| Petrol | Diesel | CNG |
|---:|---:|---:|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

Each category gets its own column.

The value is:

- `1` → category is present
- `0` → category is not present

---

# 2. One-Hot Encoding Using Pandas

Pandas provides the `get_dummies()` function for One-Hot Encoding.

```python
import pandas as pd

pd.get_dummies(
    df,
    columns=['fuel', 'owner'],
    dtype=int,
    drop_first=True
)
```
- columns selects categorical columns.

- dtype=int produces integer values.

- drop_first=True drops the first category.

Problem With Pandas Encoding

The notebook notes that Pandas does not remember which encoded category was assigned to which position. This matters when separately transforming training and testing data.

For this reason, Scikit-Learn's OneHotEncoder is used.

## 3. One-Hot Encoding Using Scikit-Learn

The notebook splits the data as follows:

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    df.iloc[:, 0:4],
    df.iloc[:, -1],
    test_size=0.2,
    random_state=2
)
```

Then:

```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(
    drop='first',
    sparse_output=False,
    dtype=np.int32
)
```

### Parameters

- `drop='first'` → drops the first category.
- `sparse_output=False` → returns a normal NumPy array.
- `dtype=np.int32` → stores encoded values as 32-bit integers.

---

## 4. Training and Testing Transformation

### Training Data

```python
X_train_new = ohe.fit_transform(
    X_train[['fuel', 'owner']]
)
```

### Test Data

```python
X_test_new = ohe.transform(
    X_test[['fuel', 'owner']]
)
```

### `fit_transform()`

`fit_transform()` performs two operations:

1. **Fit** → learns the categories from the training data.
2. **Transform** → converts the categories into numerical values.

### `transform()`

`transform()` applies the already learned encoding to the test data.

### Correct Workflow

```python
X_train_new = ohe.fit_transform(
    X_train[['fuel', 'owner']]
)

X_test_new = ohe.transform(
    X_test[['fuel', 'owner']]
)
```

> **Important:** We use `fit_transform()` on training data and only `transform()` on test data because the encoder should learn the categories from the training data.

---

## 5. Combining Features

The notebook combines `brand` and `km_driven` with the encoded columns:

```python
np.hstack(
    (
        X_train[['brand', 'km_driven']].values,
        X_train_new
    )
)
```

### What is `np.hstack()`?

`np.hstack()` means **horizontal stacking**.

It combines arrays horizontally.

For example:

```text
brand       km_driven       encoded fuel       encoded owner
  |             |                  |                  |
  └─────────────┴──────────────────┴──────────────────┘
                         ↓
                  Final Feature Array
```

---

## 6. High-Cardinality Categorical Data

The notebook discusses categorical data with many categories, using the `brand` column as an example.

### What is High Cardinality?

High cardinality means that a categorical column contains a **large number of unique categories**.

For example:

```text
brand
-----
Maruti
Hyundai
Honda
Toyota
BMW
Audi
Mercedes
...
```

If every car brand is One-Hot Encoded separately, a large number of new columns can be created.

This can make the dataset unnecessarily large.

---

## 7. Handling Rare Categories With a Threshold

To handle rare categories, the notebook uses a frequency-based threshold.

### Step 1: Calculate Brand Frequencies

```python
counts = df['brand'].value_counts()
```

`value_counts()` tells us how many times each brand occurs in the dataset.

### Step 2: View Unique Brands

```python
df['brand'].unique()
```

This displays all the unique values in the `brand` column.

### Step 3: Set a Threshold

The notebook defines:

```python
thrashold = 100
```

The idea is:

```text
Brand occurs > 100 times
        ↓
Keep the brand

Brand occurs ≤ 100 times
        ↓
Treat as rare/uncommon
```

### Step 4: Find Rare Brands

```python
rpl = counts[counts <= thrashold].index
```

This selects the brands whose frequency is **100 or less**.

Here:

- `counts` → frequency of each brand
- `counts <= thrashold` → selects rare brands
- `.index` → gets the names of those brands

---

## 8. Replacing Rare Brands With `uncommon`

The rare brands are replaced with a single category called `uncommon`.

```python
df['brand'].replace(
    rpl,
    'uncommon'
)
```

### Before

```text
Maruti
Hyundai
Honda
BMW
SmallBrand1
SmallBrand2
SmallBrand3
```

### After

```text
Maruti
Hyundai
Honda
BMW
uncommon
uncommon
uncommon
```

Instead of creating separate One-Hot Encoding columns for every rare brand, all rare brands are grouped into one category:

```text
uncommon
```

---

## 9. One-Hot Encoding the Grouped Brand Column

After replacing rare brands, One-Hot Encoding is applied:

```python
pd.get_dummies(
    df['brand'].replace(rpl, 'uncommon'),
    dtype=np.int32
)
```

The result contains columns for the frequent brands and one column for:

```text
uncommon
```

We can also view a random sample:

```python
pd.get_dummies(
    df['brand'].replace(rpl, 'uncommon'),
    dtype=np.int32
).sample(5)
```

---

## 10. Complete Workflow

```text
                 Categorical Data
                        |
                        ↓
                 One-Hot Encoding
                        |
               ┌────────┴────────┐
               ↓                 ↓
        Few Categories      Many Categories
               |                 |
               ↓                 ↓
        One-Hot Encode     Calculate Frequency
                                 |
                                 ↓
                          Set a Threshold
                                 |
                                 ↓
                      Identify Rare Categories
                                 |
                                 ↓
                    Replace with "uncommon"
                                 |
                                 ↓
                         One-Hot Encode
```

---

## 11. Important Code

### Pandas One-Hot Encoding

```python
pd.get_dummies(
    df,
    columns=['fuel', 'owner'],
    dtype=int,
    drop_first=True
)
```

### Scikit-Learn One-Hot Encoding

```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(
    drop='first',
    sparse_output=False,
    dtype=np.int32
)
```

### Training Data

```python
X_train_new = ohe.fit_transform(
    X_train[['fuel', 'owner']]
)
```

### Testing Data

```python
X_test_new = ohe.transform(
    X_test[['fuel', 'owner']]
)
```

### Combining Features

```python
np.hstack(
    (
        X_train[['brand', 'km_driven']].values,
        X_train_new
    )
)
```

### Find Rare Categories

```python
counts = df['brand'].value_counts()

thrashold = 100

rpl = counts[counts <= thrashold].index
```

### Replace Rare Categories

```python
df['brand'].replace(
    rpl,
    'uncommon'
)
```

### One-Hot Encode After Replacement

```python
pd.get_dummies(
    df['brand'].replace(rpl, 'uncommon'),
    dtype=np.int32
)
```

---

## 12. Key Takeaways

- One-Hot Encoding converts categorical data into numerical columns.
- Pandas provides `pd.get_dummies()`.
- Scikit-Learn provides `OneHotEncoder`.
- `drop='first'` drops the first encoded category.
- `sparse_output=False` returns a normal NumPy array.
- `dtype=np.int32` stores encoded values as 32-bit integers.
- `fit_transform()` learns and transforms the training data.
- `transform()` applies the learned mapping to the test data.
- `np.hstack()` can combine encoded and other feature columns.
- High-cardinality categorical columns can create many encoded columns.
- The `brand` column is an example of high-cardinality categorical data.
- `value_counts()` can be used to find the frequency of categories.
- A threshold can be used to identify rare categories.
- Rare categories can be grouped into `uncommon`.
- The notebook uses a threshold of `100` for the `brand` column.

---

## 13. Libraries Used

```python
import numpy as np
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import OneHotEncoder
```

---

# 🚀 100 Days of Machine Learning — Day 6

Today I practiced **One-Hot Encoding** using Pandas and Scikit-Learn.

### Concepts Learned

- One-Hot Encoding
- `pd.get_dummies()`
- `OneHotEncoder`
- `drop='first'`
- `sparse_output=False`
- `dtype=np.int32`
- `fit_transform()`
- `transform()`
- Train-test encoding
- `np.hstack()`
- High-cardinality categorical data
- Frequency-based rare category handling
- `value_counts()`
- Threshold-based encoding
- Grouping rare categories into `uncommon`

> **Consistency over perfection. One concept at a time. 💻📊**

#100DaysOfML #MachineLearning #Python #Pandas #ScikitLearn #DataScience #OneHotEncoding #DataPreprocessing #LearningInPublic
