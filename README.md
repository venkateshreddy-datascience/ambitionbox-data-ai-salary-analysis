# AmbitionBox Data & AI Jobs: Salary Analysis

An end-to-end Python project: I scraped public salary pages from AmbitionBox for data and AI roles, cleaned the data with pandas and regex, analysed it, and turned it into charts and insights.

**Project question:** Which data or AI role, which city and which skills should a beginner focus on?

## Data source
- Website: [AmbitionBox](https://www.ambitionbox.com/) public salary pages (for example '/profile/data-scientist-salary')
- Collected in October 2026 with Python
- Roles scraped: Data Analyst, Data Scientist, Junior Data Scientist, Senior Data Scientist, Data Engineer, Machine Learning Engineer, AI Engineer, Business Analyst, Data Science Analyst
- Places: All India + Bengaluru, Hyderabad, Pune, Mumbai, Gurgaon, Chennai, New Delhi, Noida

## Project workflow
Website selection -> Data collection -> Data understanding -> Data cleaning -> Data modeling (new columns, optional regression) -> EDA -> Visualization -> Insights and recommendations

## What the scraper does
1. Builds a URL for each role, city and page number.
2. Downloads the page with 'requests' (with a browser User-Agent, and a 2 second pause between requests).
3. Finds the '__NEXT_DATA__' JSON inside the page with 'BeautifulSoup' and reads it with 'json'.
4. Collects the values into Python lists, builds DataFrames, and saves them as CSV files.

## Dataset
| File | Rows | What it holds |
|---|---|---|
| 'raw_company_salaries.csv' | 2,333 x 14 | raw company-wise salary rows |
| 'clean_company_salaries.csv' | 2,158 x 18 | cleaned main table, 743 companies |
| 'clean_salary_reports.csv' | 400 | individual salary reports (repeats removed) |
| 'clean_role_skills.csv' | 1,125 | skills asked for in job listings |
| 'clean_role_summary.csv' | 72 | site totals per role and place |

## Data cleaning steps
1. Removed 2 duplicate rows (same role, place and company).
2. Removed rows with no salary (0 found).
3. Dropped the Junior Data Scientist role (173 rows) because it had almost no data.
4. Cleaned text with regex 're.sub' (for example 'new-delhi' becomes 'New Delhi', extra spaces removed).
5. Converted salary from rupees to lakhs per year (LPA).
6. Filled missing experience with the median (none were missing) and fixed data types.
7. Flagged, but did not delete, 29 rows where min, average and max salary do not line up and 77 salary outliers (IQR rule). These are real high-paying companies.
8. Individual reports: converted months to years and removed 3,200 repeated reports (the site shows the same reports on every city page), leaving 400.
9. Added 3 new columns: 'avg_exp', 'exp_group', 'salary_range_lpa'.

## Data dictionary (`clean_company_salaries.csv`)
| Column | Meaning |
|---|---|
| role | job role page |
| location | All India or one of 8 cities |
| company | company name |
| company_id | AmbitionBox company id |
| min_exp, max_exp | experience range in years seen for that role at that company |
| data_points | number of salary reports behind the average (small = less reliable) |
| job_count | live openings at that company for that role |
| avg_ctc_lpa, min_ctc_lpa, max_ctc_lpa | average, lowest and highest yearly salary in lakhs |
| typical_min_ctc_lpa, typical_max_ctc_lpa | usual salary band in lakhs |
| salary_check_flag | True if min, average and max do not line up (kept) |
| salary_outlier | True if the salary is an outlier by the IQR rule (kept) |
| avg_exp | (min_exp + max_exp) / 2 |
| exp_group | Fresher (0-2), Mid (3-5), Senior (6+) from avg_exp |
| salary_range_lpa | max_ctc_lpa minus min_ctc_lpa |

## Key findings
- Median company salary is about **12.4 LPA**. The middle 50% of companies pay between 8.0 and 19.5 LPA.
- **Role matters most:** Senior Data Scientist 28.7, Data Scientist 16.8, AI Engineer 12.2, Data Engineer 11.3, ML Engineer 10.7, Business Analyst 10.1, Data Analyst 5.9 LPA (medians). A Data Scientist's median is about 2.8 times a Data Analyst's.
- **City:** Gurgaon (15.6 LPA) and Bengaluru (14.1) pay the most, Pune (11.1) the least.
- **Top paying companies** (at least 50 reports): Visa 49.2, Caterpillar 42.2, Walmart 39.7, PayPal 35.8, Microsoft 34.9 LPA.
- **Experience:** median pay goes from 6.0 LPA at 1 year to 15.5 LPA at 5 years (400 reports, correlation 0.57).
- **Skills:** Python and SQL are the most asked-for skills, about 2.5 times more than AWS.
- **Openings:** Data Engineer (2,318) and Business Analyst (1,789) have more openings than Data Scientist (1,267).

![Salary by role](images/role_salary.png)
![Salary by city](images/city_salary.png)
![Top paying companies](images/top_companies.png)
![Salary vs experience](images/experience_salary.png)

## Limitations
- Salaries are company averages for a role, not one person's pay.
- The experience analysis uses only 400 individual reports.
- Openings are one all-India number per role, and one job can be counted under several roles.
- Some groups are small (for example Data Science Analyst has 46 rows), so single cells in the role vs city heatmap should not be over-read.
- Correlation shows a pattern, not a cause.
- Data comes from one website at one point in time (October 2026). Passout year was not available.

## How to run
```
pip install -r requirements.txt
jupyter notebook AmbitionBox.ipynb
```
Scraping takes about 10 to 15 minutes. To skip it, run the first cell (imports) and then start from the "DATA CLEANING" cell. The raw CSV files are included.

## Tools
Python, requests, BeautifulSoup, json, regex ('re'), pandas, NumPy, Matplotlib, Seaborn, scikit-learn, Jupyter Notebook

## Disclaimer
The data was collected from AmbitionBox's public pages for learning purposes only. All data belongs to AmbitionBox.

## Author
Venkatesh Reddy Munagala | [LinkedIn](www.linkedin.com/in/venkatesh-reddy-munagala-77b285425) | [GitHub](https://github.com/venkateshreddy-datascience)
