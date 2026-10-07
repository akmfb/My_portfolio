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
- admission_date
- discharge_date
- amount
- severity

### Initial issues identified
- Mixed date formats (DD/MM/YY, DD-MM-YY, MM.DD.YY, text dates)
- Amounts stored as text
- Duplicate rows
- Extra spaces and non-printable characters in text fields
- Missing values
- Inconsistent category labels
- Incorrect diagnosis description
- Some Admission and Discharge dates are swapped

## Data Cleaning Steps
### 1. Removed duplicates
- Used **Remove duplicates** tool in Excel
- Removed 15 duplicate rows in **Patients** table
### 2. Standardized Date Formats
The dataset contained multiple inconsistent date formats, making it impossible to sort, filter, or analyze patient timelines. To resolve this, I decomposed each raw date into Year, Month, and Day components using Excel formulas, then reconstructed them into a unified YYYY-MM-DD format.
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
- Standardized **Gender**, **Payment_Status**, and **Insurance_provider** columns
- Created a unique **Department** table and applied **Fuzzy Merge** to standardize and replace inconsistent department names
### 5. Created and Removed Columns
- Separated address and created new columns for Address, City, State, and Zip code
- Added an age column using **DATEDIF** formula
- Added a **length_of_stay** column by calculating the difference between **admission_date** and **discharge_date**
- Removed **attending_doctor_id** column as it does not have any relationship to any of the table

## Before & After Samples
### Raw Data
|Date_of_birth|Amount|Department|
|-------------|------|----------|
|September 13, 1978|USD 5,344.32|Onco|
|7.12.1976|$20.00|GeneralSurgery|
|01-25-54|5711|CARDIOLOGY
|07/16/1997|10209.6588471341|Peds|

### Cleaned Data
|Date_of_birth|Amount|Department|
|-------------|------|----------|
|1978-09-13|$5,344.32|Oncology|
|1976-07-12|$20.00|General Surgery|
|1954-01-25|$5,711.00|Cardiology|
|1997-07-16|$10,209.66|Pediatrics|

### Screenshots
<details>
  <summary><b>Raw Tables</b></summary> 

![](raw_patients.png)
![](raw_admissions.png)
![](raw_billing.png)
![](raw_diagnosis.png)
</details>
<details>
  <summary><b>Cleaned Tables</b></summary>

![](cleaned_patients.png)
![](cleaned_admissions.png)
![](cleaned_billing.png)
![](cleaned_diagnosis.png)
</details>

## Tools and Techniques Used
- Excel formulas: TRIM(), CLEAN(), PROPER(), LEFT(), RIGHT(), MID(), IFERROR(), XLOOKUP(), DATEDIFF(), QUARTILE.INC()
- Conditional Formatting
- Data validation
- Text to Split
- Power Query
- Remove Duplicates
- Scatter Plots chart
- IQR

## Challenges & Decisions
- Chose to remove **attending_doctor_id** column as it did not have any relationship to the other tables
- Utilized Artificial Intelligence to give the correct diagnosis description to each code. The results have been used as a lookup table for each code

## Final Dataset Summary
After cleaning, and validating all four tables, the final dataset now contains:
- Fully standardized dates across all tables (YYYY-MM-DD)
- Clean numeric fields, including currency formatted billing amounts
- Cleaned text fields (trimmed, normalized, corrected spellings, and standardized categories)
- No duplicate records across all tables
- Consistent category labels for gender, payment status, insurance provider, and department name
- New derived fields:
  - Age
  - Length of Stay
- Outlier analysis using the IQR method:
  - Admissions: 94 missing length of stay values due to missing admission or discharge dates and 274 length of stay outliers (flagged, not removed)
  - Billing: 61 missing billing amounts, 60 billing amount outliers (flagged,  not removed)
      - 20 records show negative amounts marked as "Paid" which is incorrect. These records are flagged as anomalies and excludes from the calculation. 
- Correlation checks performed using scatter plot:
  - No correlation between Length of stay and Severity
  - No correlation between billing amount and diagnosis count
  - No correlation between billing amount and length of stay
- All outliers and missing values are kept to preserve dataset completeness; **outliers were flagged rather than deleted**
- All formula columns were pasted as values to keep the final dataset clean and stable.
