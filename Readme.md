# Layoffs Data Cleaning & Processing (MySQL)

This project contains a complete SQL workflow for transforming a messy layoffs dataset into a clean, analysis-ready dataset using MySQL.  
The cleaning pipeline is fully scripted in main.sql, making it easy to re-run and fully transparent.

The project follows a structured approach:
1. Import raw CSV → `layoffs_raw`
2. Copy to staging → `layoffs_staging`
3. Perform all cleaning, conversions, and validation
4. Output a high-quality cleaned dataset for analysis

---

## Project Files

### main.sql
The full SQL workflow, including:
- Database & table creation  
- Raw → staging pipeline  
- Whitespace trimming  
- Date conversion  
- Numeric cleaning  
- Country standardization  
- Location cleaning  
- Duplicate removal  
- Month & year extraction  
- Continent assignment  
- Final data completeness summary  

### layoffs.csv
The raw dataset exported from Excel, loaded exactly as-is.

### layoffs_cleaned.csv 
The cleaned dataset exported from MySQL workbench.

---

## Data Cleaning Strategy

The project uses a two-table approach for clarity and safety:

---

### 1. layoffs_raw — Raw Import Table

A flexible table where all columns are VARCHAR, allowing the CSV to load without errors or assumptions.

This preserves the original dataset unchanged.

---

### 2. layoffs_staging — Cleaning & Transformation Table**

This table is created using:

```sql
CREATE TABLE layoffs_staging LIKE layoffs_raw; ```sql


All cleaning occurs here, including:

trimming whitespace

converting dates

validating and converting numbers

handling missing values

removing duplicates

standardizing country names

extracting month/year

mapping countries → continents

cleaning location text

This table becomes the final cleaned dataset.
