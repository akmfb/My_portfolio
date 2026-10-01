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
- Mixed date formats (DD/MM/YY, DD-MM-YY, MM.DD.YY, text dates)
- Amounts stored as text
- Duplicate rows
- Extra spaces and non-printable characters in text fields
- Missing values
- Inconsistent category labels

## Data Cleaning Steps
### 1. Removed duplicates
- Used **Remove duplicates** tool in Excel
- Removed 15 duplicate rows in **Patients** table
### 2. Standardized Date Formats
- Created 3 columns (Year, Month, Day) to extract dates using formula:
```formula
Month =IFERROR(VALUE(IF(LEFT(C2,2)>"12",MID(C2,4,2),LEFT(C2,2))),"")
Day =IFERROR(VALUE(IF(AND(LEFT(C2,2)>"12",MID(C2,4,2)>="12"),LEFT(C2,2),MID(C2,4,2))),"")
Year=IFERROR(2000+RIGHT(C2,2),"")
YMD =IFERROR(DATE(F2,D2,E2),C2)
```
- Used **Power Query** to format text dates into date
- Used formatting to standardized all dates into YYYY-MM-DD
### 3. Corrected Data types
- Converted **Amount** into numeric and formatted as currency
- Cleaned text fields using:
  - **TRIM()**
  - **CLEAN()**
  - **PROPER()**
- Corrected spelling mistakes from the **address column**
### 4. Fixed Category Inconsistencies
- Standardized **Department**, **Gender**, **Payment_Status**, and **Insurance_provider** columns
- 
