# 📂 Working with JSON & SQL

This notebook demonstrates how to work with **JSON data**, consume **REST APIs**, and establish a connection between **Python** and **MySQL**. It covers reading JSON files, fetching JSON data from an API, and preparing Python for MySQL database connectivity.

---

## 📚 Topics Covered

- Reading Local JSON Files
- Reading JSON Data from an API
- Installing MySQL Connector
- Connecting Python with MySQL

---

## 🛠️ Technologies Used

- Python
- Pandas
- JSON
- REST API
- MySQL
- MySQL Connector for Python
- Jupyter Notebook

---

## 📁 Project Structure

```
Working_with_JSON_SQL/
│
├── test.json
├── Working_with_JSON_SQL.ipynb
└── README.md
```

---

## 📖 Contents

### 1. Import Pandas

```python
import pandas as pd
```

---

### 2. Load Local JSON File

```python
df = pd.read_json("test.json")
df.head()
```

---

### 3. Load JSON Data from an API

```python
df = pd.read_json("https://api.exchangerate-api.com/v4/latest/INR")
df.head()
```

This demonstrates how to fetch live JSON data directly from a public REST API.

---

### 4. Install MySQL Connector

```python
!pip install mysql-connector-python
```

---

### 5. Connect Python to MySQL

Example:

```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="your_password",
    database="world"
)

print(conn.is_connected())
```

---

## 📌 Learning Outcomes

After completing this notebook, you will be able to:

- Read JSON files using Pandas.
- Fetch JSON data from REST APIs.
- Understand JSON data structures.
- Install the MySQL Connector package.
- Connect Python applications to a MySQL database.
- Prepare data for further analysis using Pandas.

---

## 📦 Installation

Install the required packages before running the notebook.

```bash
pip install pandas
pip install mysql-connector-python
```

---

## 📖 Libraries Used

| Library | Purpose |
|----------|---------|
| pandas | Read and manipulate JSON data |
| mysql.connector | Connect Python with MySQL |
| requests *(optional)* | Fetch API data |

---

## 🎯 Prerequisites

- Python 3.x
- Jupyter Notebook / JupyterLab
- MySQL Server (optional for database connection)
- Internet connection (for API examples)

---

## 🚀 Future Topics

- Reading Nested JSON
- Flattening JSON using `json_normalize()`
- Writing JSON Files
- Exporting JSON to CSV
- Importing SQL Tables into Pandas
- Executing SQL Queries with Python
- CRUD Operations using MySQL Connector

---

## 👨‍💻 Author

**Priyanshu Singh**

Learning Machine Learning, Data Analysis, and Data Engineering through practical projects.
