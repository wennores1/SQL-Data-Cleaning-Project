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
-- Logic to populate null industries based on existing data
UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
    ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL
AND t2.industry IS NOT NULL;
