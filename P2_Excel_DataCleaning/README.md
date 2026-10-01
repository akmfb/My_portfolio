## Project Overview
This project focuses on cleaning raw dataset containing healthcare admissions and transactions. The original file included inconsistent formatting, missing value, incorrect date formats, duplicate records, and non-standardized categories.

All cleaning was performed in Microsoft Excel, using formulas, power query, and validation tools.

## Dataset Description
Source: [Kaggle - Healthcare Data Cleaning (hard)](https://www.kaggle.com/datasets/nudratabbas/healthcare-data-cleaning-hard/data)<br>
Files: 4 csv files <br>
Columns: 25

### Key fields
- admission_id
- patient_id
- billing_id
- insurance_provider
- department

### Initial issues identified
- Mixed date formats (DD/MM/YYYY/, DD-MM-YYYY, MM.DD.YYYY, text dates)
- Amounts stored as text
- Duplicate rows
- Extra spaces and non-printable characters in text fields
- Missing values
- Inconsistent category labels

## Data Cleaning Steps
### 1. Removed duplicates
- 


