# Web Scrapping Project

## What is web scraping?

Web scraping is the process of automatically collecting data from websites. Instead of manually copying information from a webpage, a script can visit the page, read the HTML content, extract the needed information, and save it in a structured format such as CSV or Excel.

This project does that for company data from AmbitionBox. It extracts useful business information such as company name, rating, reviews, salary data, and job counts, then stores the results in a CSV file.

---

## Project description

This project is a Python-based web scraping script that fetches company listings from AmbitionBox and organizes the data into a table. The script uses:

- Python
- Requests for downloading web pages
- BeautifulSoup for parsing HTML
- Pandas for storing and exporting data

The final output is saved in the file named ambitionbox_companies.csv.

---

## Objective

The main goal of this project is to collect company information from a website in a simple and reusable way. This is useful for:

- data analysis
- market research
- comparing companies
- building datasets for reports
- learning web scraping with Python

---

## What data is extracted?

The scraper collects the following information for each company:

- Company name
- Rating
- Number of reviews
- Salary information
- Job count

The extracted records are stored in a DataFrame and then exported to CSV.

---

## Technologies used

- Python 3
- requests
- BeautifulSoup
- pandas
- lxml

---

## Project structure

web_scrapping/
├── README.md
├── Untitled.ipynb
├── ambitionbox_companies.csv
└── .ipynb_checkpoints/

---

## How the project works

1. A URL is requested using the requests library.
2. The page HTML is downloaded.
3. BeautifulSoup parses the HTML content.
4. The relevant company cards are selected.
5. Specific information is extracted from each card.
6. The data is combined into a Pandas DataFrame.
7. The final dataset is saved to CSV.

---

## Example workflow

The notebook includes logic to:

- send a browser-like User-Agent header
- fetch the AmbitionBox page
- find each company card
- extract the company name and rating
- split text content to locate review, salary, and job values
- collect all records across multiple pages
- save them in a CSV file

---

## Output file

The generated dataset is saved as:

- ambitionbox_companies.csv

This file contains rows for multiple companies with columns such as:

- Company
- Rating
- Reviews
- Salaries
- Jobs

---

## Setup

Make sure you have Python installed, then install the required libraries:

pip install pandas requests beautifulsoup4 lxml

---

## Running the project

Open the notebook file and run the cells in order. The script will:

- fetch the data,
- parse the HTML,
- create a DataFrame,
- and export the result to CSV.

---

## Notes

- Some websites block automated scraping, so headers and delays may be needed.
- Websites change their HTML structure over time, so scraping logic may need updates.
- Always check the website's terms of service and use scraping responsibly.

---

## Summary

This project is a beginner-friendly example of web scraping using Python. It demonstrates how to collect structured data from a real website and save it into a CSV file for later analysis.

It is a good practical project for learning:

- HTTP requests
- HTML parsing
- data extraction
- data cleaning
- CSV export

---

## License

This project is for learning and educational purposes.

