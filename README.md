# World Layoffs Data Cleaning Project 🧹

## Project Overview
In this project, I performed a complete data cleaning process on a dataset containing information about world layoffs. Using **MySQL Workbench**, I transformed the raw data into a clean, usable format ready for analysis.

The goal was to simulate a real-world data engineering task: taking messy, raw data and standardizing it to ensure accuracy and consistency.

## 🛠 Tools Used
* **SQL Flavor:** MySQL
* **IDE:** MySQL Workbench

## 📋 Key Steps & Techniques

### 1. Removing Duplicates
The raw data contained duplicate entries that needed to be removed to avoid skewing the analysis.
* **Technique:** I used `ROW_NUMBER()` and `PARTITION BY` to identify unique rows based on all columns.
* **Process:** I created a staging table (`layoffs_staging2`) to safely delete the duplicates identified by the window function.

```sql
-- Snippet of the duplicate identification logic
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, ...
) AS row_num
FROM layoffs_staging;
```
### 2. Standardizing Data
I found several inconsistencies in the text data that needed fixing:
Company Names: Used TRIM() to remove unnecessary whitespace.
Industries: Standardized distinct values (e.g., merging "Crypto Currency" and "CryptoCurrency" into just "Crypto").
Countries: Fixed data entry errors, such as removing a trailing period from "United States.".
Date Format: Converted the date column from a string (text) format to a standard SQL DATE format using STR_TO_DATE.

### 3. Handling Null & Missing Values
This was the most complex part of the cleaning process.
Populating Missing Data: I identified companies with missing industry data. I used a Self-Join to look up if the same company had the industry populated in another row and filled the null values accordingly.
Removing Useless Data: Rows where both total_laid_off and percentage_laid_off were NULL provided no value for the analysis, so they were removed.
-- Logic to populate null industries based on existing data
UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
    ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL
AND t2.industry IS NOT NULL;

### 4. Final Cleanup
Removed the helper columns used for the cleaning process (like row_num).
The final table is now optimized and clean for Exploratory Data Analysis (EDA).

### Skills Demonstrated
Window Functions: ROW_NUMBER()
Joins: Self-Join for populating missing data.
CTEs: Using Common Table Expressions to organize complex queries.
Data Type Casting: Converting text to date formats.
Staging Tables: Creating temporary tables to preserve raw data integrity.
