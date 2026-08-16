📊 Understanding Your Data

This folder contains my practical work on Understanding Data using Python and Pandas as part of my Machine Learning learning journey.

The notebook uses a real-world AmbitionBox company dataset to practice the basic steps of Exploratory Data Analysis (EDA) and understand the structure and quality of a dataset.

📁 Files
Untitled.ipynb — Jupyter Notebook containing the data analysis
ambitionbox_companies.csv — Dataset containing company information
readme.md — Documentation for this folder
📌 Dataset

The dataset contains information about companies collected from AmbitionBox company listing pages.

Columns
Column	Description
Company	Name of the company
Rating	Company rating
Reviews	Number of reviews listed
Salaries	Number of salary records listed
Jobs	Number of jobs listed
Dataset Size
Rows: 980
Columns: 5
🔍 What I Learned
1. Loading Data

Used Pandas to load the CSV dataset:

import pandas as pd

df = pd.read_csv("ambitionbox_companies.csv")
2. Understanding Dataset Size

Used:

df.shape

This returns the number of rows and columns in the dataset.

3. Viewing the Data

Used:

df.head()

to view the first few rows.

Used:

df.sample(5)

to randomly select five rows from the dataset.

4. Understanding Data Types

Used:

df.info()

This helps identify:

Number of rows
Column names
Missing values
Data types
Memory usage

In this dataset, Rating is numerical, while Company, Reviews, Salaries, and Jobs are currently stored as object/string values.

5. Checking Missing Values

Used:

df.isnull().sum()

The dataset currently contains no missing values in the five columns.

6. Descriptive Statistics

Used:

df.describe()

For the Rating column:

Mean: 3.785
Median: 3.8
Minimum: 1.3
Maximum: 4.9

This helps understand the central tendency and spread of numerical data.

7. Checking Duplicate Rows

Used:

df.duplicated().sum()

The dataset contains 0 duplicate rows.

8. Correlation

Used:

df.corr(numeric_only=True)

This checks the correlation between numerical columns.

At the current stage, only Rating is stored as a numerical column, so the correlation output only contains the Rating column.
