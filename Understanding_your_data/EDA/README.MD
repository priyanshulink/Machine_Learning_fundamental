# 📊 Exploratory Data Analysis (EDA)

## What is EDA?

**Exploratory Data Analysis (EDA)** is the process of **understanding, cleaning, exploring, and analyzing a dataset before applying Machine Learning algorithms**.

EDA helps us understand:

* What data we have
* How the data is structured
* Which columns are numerical or categorical
* Whether missing values are present
* Whether duplicate values exist
* How variables are distributed
* Whether outliers are present
* Relationships between different variables
* Important patterns and trends in the data

In simple words:

> **EDA = Understand your data before building a Machine Learning model.**

---

# 🔄 Steps of EDA

A typical EDA process can be performed in the following steps:

```text
                 DATASET
                    ↓
          1. Understand the Data
                    ↓
          2. Check Data Structure
                    ↓
          3. Clean the Data
                    ↓
          4. Univariate Analysis
                    ↓
          5. Bivariate Analysis
                    ↓
         6. Multivariate Analysis
                    ↓
          7. Find Patterns & Insights
                    ↓
          8. Prepare Data for ML
```

---

# 1️⃣ Understand the Dataset

First, load the dataset and take a basic look at it.

```python
import pandas as pd

df = pd.read_csv("dataset.csv")
```

### View first rows

```python
df.head()
```

### View random rows

```python
df.sample(5)
```

### Check dataset size

```python
df.shape
```

`shape` gives:

```text
(rows, columns)
```

### Check column names

```python
df.columns
```

---

# 2️⃣ Understand Data Structure

Use:

```python
df.info()
```

This helps identify:

* Number of rows
* Column names
* Data types
* Missing values
* Memory usage

You can also check data types using:

```python
df.dtypes
```

---

# 3️⃣ Check Data Quality

Before analysis, check whether the dataset contains problems.

## Missing Values

```python
df.isnull().sum()
```

If missing values exist, decide whether to:

* Remove the rows
* Fill them with mean/median/mode
* Use another appropriate method

---

## Duplicate Values

Check duplicates:

```python
df.duplicated().sum()
```

Remove duplicates:

```python
df.drop_duplicates(inplace=True)
```

---

# 4️⃣ Descriptive Statistics

For numerical columns:

```python
df.describe()
```

This provides:

* Count
* Mean
* Standard deviation
* Minimum
* 25th percentile
* Median
* 75th percentile
* Maximum

Example:

```python
df['Age'].describe()
```

---

# 5️⃣ Univariate Analysis

**Univariate analysis means analyzing one variable at a time.**

### For Numerical Data

Use:

* Histogram
* Boxplot
* `describe()`

Example:

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.histplot(df['Age'], kde=True)
plt.show()
```

Boxplot:

```python
sns.boxplot(x=df['Age'])
plt.show()
```

### For Categorical Data

Use:

* Countplot
* Value counts

```python
df['Gender'].value_counts()
```

```python
sns.countplot(x=df['Gender'])
plt.show()
```

### Purpose of Univariate Analysis

It helps identify:

* Distribution
* Frequency
* Central tendency
* Spread
* Outliers
* Skewness

---

# 6️⃣ Bivariate Analysis

**Bivariate analysis means analyzing two variables together.**

It helps us understand the **relationship between two variables**.

### Numerical + Numerical

Use scatter plot:

```python
sns.scatterplot(x=df['Age'], y=df['Salary'])
plt.show()
```

### Categorical + Numerical

Use boxplot:

```python
sns.boxplot(x=df['Gender'], y=df['Salary'])
plt.show()
```

### Categorical + Categorical

Use cross-tabulation:

```python
pd.crosstab(df['Gender'], df['Purchased'])
```

---

# 7️⃣ Multivariate Analysis

**Multivariate analysis means analyzing more than two variables together.**

One common technique is a **correlation heatmap**.

```python
sns.heatmap(df.corr(numeric_only=True), annot=True)
plt.show()
```

This helps understand relationships between multiple numerical variables.

---

# 📈 Important Visualization Techniques

| Purpose                          | Visualization |
| -------------------------------- | ------------- |
| Numerical distribution           | Histogram     |
| Outlier detection                | Boxplot       |
| Category frequency               | Countplot     |
| Two numerical variables          | Scatterplot   |
| Numerical comparison by category | Boxplot       |
| Correlation                      | Heatmap       |
| Category comparison              | Barplot       |

---

# 🧹 Data Cleaning During EDA

During EDA, we may need to:

### Handle Missing Values

```python
df.isnull().sum()
```

### Remove Duplicates

```python
df.drop_duplicates(inplace=True)
```

### Convert Data Types

```python
df['Age'] = df['Age'].astype(int)
```

### Handle Outliers

Use a boxplot to identify unusual values:

```python
sns.boxplot(x=df['Age'])
plt.show()
```

### Rename Columns

```python
df.rename(columns={'old_name': 'new_name'}, inplace=True)
```

---

# 🔍 Correlation Analysis

Correlation tells us how strongly two numerical variables are related.

```python
df.corr(numeric_only=True)
```

Visualization:

```python
sns.heatmap(
    df.corr(numeric_only=True),
    annot=True
)

plt.show()
```

### Correlation values

```text
+1  → Strong positive relationship
 0  → No linear relationship
-1  → Strong negative relationship
```

For example:

```text
Study Hours ↑
     ↓
Marks ↑
```

This could indicate a positive relationship.

---

# 🧠 EDA Cheat Sheet

```text
Load Data
    ↓
df.head()
    ↓
df.shape
    ↓
df.info()
    ↓
df.describe()
    ↓
Check Missing Values
    ↓
Check Duplicates
    ↓
Univariate Analysis
    ↓
Bivariate Analysis
    ↓
Multivariate Analysis
    ↓
Find Patterns & Insights
    ↓
Data Cleaning
    ↓
Prepare Data for ML
```

---

# 🛠️ Libraries Used for EDA

The most commonly used Python libraries are:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Pandas

Used for:

* Loading data
* Data manipulation
* Data cleaning
* Data analysis

### NumPy

Used for:

* Numerical operations
* Arrays
* Mathematical calculations

### Matplotlib

Used for:

* Basic data visualization
* Charts and graphs

### Seaborn

Used for:

* Statistical visualization
* Histograms
* Boxplots
* Countplots
* Heatmaps
* Scatterplots

---

# 🎯 Why is EDA Important in Machine Learning?

EDA is important because a Machine Learning model is only as good as the data given to it.

EDA helps us:

✅ Understand the dataset
✅ Find missing values
✅ Find duplicate records
✅ Detect outliers
✅ Understand distributions
✅ Find relationships between variables
✅ Identify useful features
✅ Detect data-quality problems
✅ Decide which preprocessing techniques are needed
✅ Prepare better data for Machine Learning

---

# 🚀 Final EDA Workflow

```text
             Raw Dataset
                  ↓
            Load the Data
                  ↓
          Understand the Data
                  ↓
        Check Missing Values
                  ↓
        Check Duplicate Values
                  ↓
        Check Data Types
                  ↓
       Descriptive Statistics
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
   Univariate          Bivariate
   Analysis             Analysis
        ↓                   ↓
        └─────────┬─────────┘
                  ↓
        Multivariate Analysis
                  ↓
          Visualization
                  ↓
        Find Insights/Patterns
                  ↓
           Clean the Data
                  ↓
       Feature Engineering
                  ↓
        Machine Learning 🚀
```

## 📚 Key Concept

> **EDA is not just about making graphs. It is about asking questions, understanding the data, finding problems, discovering patterns, and preparing the dataset for Machine Learning.**

This README can be used as a **general EDA guide** for future datasets and projects.
