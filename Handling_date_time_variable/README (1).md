# Working with Date and Time in Pandas

This notebook demonstrates how to work with **date and time data using Pandas**.

## 📌 Topics Covered

- Convert a column to `datetime`
- Extract year, month, and day
- Extract day of the week
- Get weekday names
- Identify weekends
- Extract week of the year
- Extract quarter
- Create semester/half-year information
- Calculate the difference between dates
- Calculate elapsed days, months, minutes, and hours
- Extract hour, minute, second, and time from a datetime column

---

## 1. Import Libraries

```python
import pandas as pd
import numpy as np
```

---

## 2. Load the Dataset

```python
date = pd.read_csv("orders.csv")
time = pd.read_csv("messages.csv")

date.head()
time.head()
```

---

## 3. Check Data Types

Initially, the `date` column is usually stored as an `object`.

```python
date.info()
time.info()
```

---

# Working with Date

## 4. Convert Column to Datetime

Convert the `date` column from `object` to Pandas `datetime`.

```python
date['date'] = pd.to_datetime(date['date'])

date.info()
```

Now we can use Pandas datetime functions such as `.dt.year`, `.dt.month`, `.dt.day`, etc.

---

## 5. Extract Year

```python
date['date_year'] = date['date'].dt.year

date.head()
```

---

## 6. Extract Month

### Month as a Number

```python
date['date_month'] = date['date'].dt.month

date.head()
```

Example:

```text
January  -> 1
August   -> 8
December -> 12
```

### Month as a Name

```python
date['date_month'] = date['date'].dt.month_name()

date.head()
```

Example:

```text
January
August
December
```

---

## 7. Extract Day

```python
date['date_day'] = date['date'].dt.day

date.head()
```

This extracts the day of the month.

Example:

```text
2019-12-10 -> 10
2018-08-15 -> 15
```

---

## 8. Extract Day of Week

Pandas represents Monday as `0` and Sunday as `6`.

```python
date['date_ofweek'] = date['date'].dt.dayofweek

date.head()
```

Mapping:

```text
Monday    -> 0
Tuesday   -> 1
Wednesday -> 2
Thursday  -> 3
Friday    -> 4
Saturday  -> 5
Sunday    -> 6
```

---

## 9. Extract Day of Week Name

```python
date['date_dow_name'] = date['date'].dt.day_name()

date.head()
```

Example:

```text
Tuesday
Wednesday
Saturday
Sunday
```

---

## 10. Check Whether Date is Weekend

Saturday and Sunday are considered weekends.

```python
date['date_is_weekend'] = np.where(
    date['date_dow_name'].isin(['Sunday', 'Saturday']),
    0,
    1
)

date.head()
```

Here:

```text
0 -> Weekend
1 -> Weekday
```

If you prefer the more intuitive convention:

```python
date['date_is_weekend'] = np.where(
    date['date_dow_name'].isin(['Sunday', 'Saturday']),
    1,
    0
)
```

Then:

```text
1 -> Weekend
0 -> Weekday
```

---

## 11. Extract Week of the Year

Use ISO calendar week numbers:

```python
date['date_week'] = date['date'].dt.isocalendar().week

date.head()
```

Example:

```text
2019-12-10 -> Week 50
2018-08-15 -> Week 33
```

---

## 12. Extract Quarter

A year is divided into four quarters:

```text
Q1 -> January to March
Q2 -> April to June
Q3 -> July to September
Q4 -> October to December
```

Code:

```python
date['Quarter'] = date['date'].dt.quarter

date.head()
```

---

## 13. Extract Semester / Half-Year

We can divide the year into two semesters:

```text
Semester 1 -> Q1 + Q2
Semester 2 -> Q3 + Q4
```

Code:

```python
date['semester'] = np.where(
    date['Quarter'].isin([1, 2]),
    1,
    2
)

date.head()
```

---

# Date Difference

## 14. Get Today's Date and Time

```python
import datetime

today = datetime.datetime.today()

today
```

---

## 15. Calculate Difference Between Today and a Date

```python
today - date['date']
```

The result is a `timedelta` containing the difference between the current date and each date in the DataFrame.

---

## 16. Get Difference in Days

```python
(today - date['date']).dt.days
```

This returns only the number of complete days.

---

## 17. Calculate Months Passed

A simple approximate method is to divide the number of days by 30:

```python
np.round(
    (today - date['date']).dt.days / 30,
    0
)
```

> Note: This is an approximation because months do not all have exactly 30 days.

For many date-analysis tasks, using an actual calendar-based month calculation is more accurate.

---

# Working with Time

## 18. Convert Time Column to Datetime

```python
time['date'] = pd.to_datetime(time['date'])

time.head()
```

---

## 19. Extract Hour

```python
time['hours'] = time['date'].dt.hour
```

Example:

```text
23:40:00 -> 23
00:50:00 -> 0
```

---

## 20. Extract Minute

```python
time['min'] = time['date'].dt.minute
```

Example:

```text
23:40:00 -> 40
00:50:00 -> 50
```

---

## 21. Extract Seconds

```python
time['sec'] = time['date'].dt.second
```

Example:

```text
23:40:15 -> 15
```

---

## 22. Extract Time Only

```python
time['time'] = time['date'].dt.time

time.head()
```

Example:

```text
00:50:00
23:40:00
00:21:00
```

---

# Time Difference

## 23. Difference in Seconds

```python
(today - time['date']).dt.total_seconds()
```

This returns the difference in **seconds**.

---

## 24. Difference in Minutes

```python
(today - time['date']).dt.total_seconds() / 60
```

This converts seconds into minutes.

---

## 25. Difference in Hours

```python
(today - time['date']).dt.total_seconds() / 3600
```

This converts seconds into hours.

---

# Quick Reference

| Task | Pandas Code |
|---|---|
| Convert to datetime | `pd.to_datetime()` |
| Year | `.dt.year` |
| Month number | `.dt.month` |
| Month name | `.dt.month_name()` |
| Day | `.dt.day` |
| Day of week number | `.dt.dayofweek` |
| Day name | `.dt.day_name()` |
| Week of year | `.dt.isocalendar().week` |
| Quarter | `.dt.quarter` |
| Hour | `.dt.hour` |
| Minute | `.dt.minute` |
| Second | `.dt.second` |
| Time | `.dt.time` |
| Difference in days | `.dt.days` |
| Difference in seconds | `.dt.total_seconds()` |
| Difference in minutes | `.dt.total_seconds() / 60` |
| Difference in hours | `.dt.total_seconds() / 3600` |

---

## 🎯 Key Learning

The most important step when working with dates in Pandas is converting the column to `datetime`:

```python
df['date'] = pd.to_datetime(df['date'])
```

After that, the `.dt` accessor makes it easy to extract useful date and time features:

```python
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['day_name'] = df['date'].dt.day_name()
df['hour'] = df['date'].dt.hour
```

These features are useful for **EDA, data analysis, feature engineering, time-series analysis, and machine learning**.
