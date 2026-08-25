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
