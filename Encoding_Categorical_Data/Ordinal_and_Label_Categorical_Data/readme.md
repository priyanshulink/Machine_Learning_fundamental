Day 6 - Categorical Encoding in Machine Learning

This repository contains my learning and practice work from Day 6 of my 100 Days of Machine Learning journey.

📚 What is Categorical Encoding?

In a dataset, some columns contain categories or text values instead of numbers.

For example:

| Gender | Review | Education | Purchased |
|--------|--------|-----------|-----------|
| Male | Good | PG | Yes |
| Female | Poor | UG | No |
| Male | Average | School | Yes |
| Female | Good | PG | No |


Most Machine Learning algorithms work with numerical data. Therefore, we need to convert categorical values into numbers.

This process is called Categorical Encoding.

Categorical Encoding is the process of converting categorical/text data into numerical values so that Machine Learning algorithms can process it.

Example:

Yes → 1
No  → 0

Two important encoding techniques I learned are:

Ordinal Encoding

Label Encoding

1. 🔢 Ordinal Encoding

The word ordinal means that the categories have a meaningful order or ranking.

### Example

#### Before Encoding

| Review  |
|---------|
| Good    |
| Poor    |
| Average |
| Good    |

#### After Encoding

| Review  | Encoded |
|---------|--------:|
| Good    |       2 |
| Poor    |       0 |
| Average |       1 |
| Good    |       2 |

Python Implementation

from sklearn.preprocessing import OrdinalEncoder

oe = OrdinalEncoder()

X_train = oe.fit_transform(X_train)
X_test = oe.transform(X_test)

When to use Ordinal Encoding?

Use it when categories have a natural order.

Examples:

Poor < Average < Good

Low < Medium < High

School < UG < PG

2. 🏷️ Label Encoding

Label Encoding converts categorical values into numerical labels.

For example, the purchased column can contain:

Yes
No
Yes
No

After Label Encoding:

Yes → 1
No  → 0

Python Implementation

from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()

y_train = le.fit_transform(y_train)
y_test = le.transform(y_test)

We can check the learned classes using:

le.classes_

Example output:

['No' 'Yes']

This means:

No  → 0
Yes → 1

3. 🔍 Difference Between Ordinal Encoding and Label Encoding

Feature

Ordinal Encoding

Label Encoding

Main use

Categorical features

Usually categorical target

Input

Can handle multiple feature columns

One 1D column

Categories have order?

Yes, normally

Not necessarily

Example

Poor → Average → Good

No → Yes

Scikit-learn

OrdinalEncoder

LabelEncoder

Easy way to remember

Categorical Encoding = Converting categories into numbers.

Ordinal Encoding = Categories with meaningful order → numbers.

Label Encoding = Categorical target variable → numerical labels.

4. 📊 My Dataset Example

In my dataset, I used:

review

education

purchased

The structure is:

                 DATA
                   |
        ┌──────────┴──────────┐
        ↓                     ↓
     Features               Target
 X = review, education   y = purchased
        |                     |
        ↓                     ↓
 Ordinal Encoding       Label Encoding
        |                     |
        ↓                     ↓
 Poor → 0                 No → 0
 Average → 1              Yes → 1
 Good → 2

Feature and Target Selection

X = df.iloc[:, 2:4]
y = df.iloc[:, 4]

Here:

X contains review and education

y contains purchased

5. 🔀 Train-Test Split

Before encoding and model training, I learned how to split the dataset into training and testing data.

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

Meaning

80% → Training data

20% → Testing data

random_state=42 → Makes the split reproducible

6. ⚠️ Important Learning: Fit and Transform

A very important concept I learned is:

y_train = le.fit_transform(y_train)
y_test = le.transform(y_test)

We fit the encoder only on the training data and then use the same encoder to transform the test data.

This helps prevent data leakage.

Do not do this:

y_train = le.fit_transform(y_train)
y_test = le.fit_transform(y_test)

because the encoder may learn a different mapping from the test data.

🛠️ Technologies Used

Python

Pandas

Scikit-learn

Jupyter Notebook

🎯 Key Takeaways

Categorical data needs to be converted into numerical form for many Machine Learning algorithms.

Ordinal Encoding is useful when categories have a meaningful order.

Label Encoding can be used for a categorical target variable.

X represents features and y represents the target.

Training and testing data should be separated before building the model.

The encoder should be fitted on training data and then used to transform test data.

🚀 100 Days of Machine Learning

Day 6 Complete!

Learning Machine Learning step by step through practical implementation.

Consistency over perfection. One day at a time. 💻📊

#100DaysOfML #MachineLearning #Python #DataScience #ScikitLearn #DataPreprocessing #CategoricalEncoding #LabelEncoding #OrdinalEncoding #LearningInPublic
