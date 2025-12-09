# Layoffs Data Cleaning \& Processing (MySQL)



This project contains a complete SQL workflow for transforming a messy layoffs dataset into a clean, analysis-ready dataset using MySQL.  

The cleaning pipeline is fully scripted in **main.sql**, making it easy to re-run and fully transparent.



The project follows a structured approach:

1\. Import raw CSV - `layoffs\_raw`

2\. Copy to staging - `layoffs\_staging`

3\. Perform all cleaning, conversions, and validation

4\. Output a high-quality cleaned dataset for analysis



### \##Project Files



\### main.sql

The full SQL workflow, including:

\- Database \& table creation  

\- Raw → staging pipeline  

\- Whitespace trimming  

\- Date conversion  

\- Numeric cleaning  

\- Country standardization  

\- Location cleaning  

\- Duplicate removal  

\- Month \& year extraction  

\- Continent assignment  

\- Final data completeness summary  



\### layoffs\_raw.csv

The raw dataset exported from Excel, loaded exactly as-is.



\### layoffs\_cleaned.csv

The cleaned dataset exported from MySQL workbench.





### \##Data Cleaning Strategy



The project uses a two-table approach for clarity and safety:



\### 1. layoffs\_raw — Raw Import Table



A flexible table where \*\*all columns are VARCHAR\*\*, allowing the CSV to load without errors or assumptions.



This preserves the original dataset unchanged.



\### 2. layoffs\_staging — Cleaning \& Transformation Table



This table is created using:



-- sql

CREATE TABLE layoffs\_staging LIKE layoffs\_raw;



All cleaning occurs here, including:



* trimming whitespace



* converting dates



* validating and converting numbers



* handling missing values



* removing duplicates



* standardizing country names



* extracting month/year



* mapping countries → continents



* cleaning location text



This table becomes the final cleaned dataset.



### \##How to Use the Script



**1. Create database \& tables**



Run the first section of main.sql to create:



* layoffs\_db



* layoffs\_raw



* layoffs\_staging





**2. Import your CSV**



LOAD DATA INFILE '/path/to/layoffs.csv'

INTO TABLE layoffs\_raw

FIELDS TERMINATED BY ','

ENCLOSED BY '"'

LINES TERMINATED BY '\\n'

IGNORE 1 ROWS;



You may need to:



* adjust line endings on Windows



* move the file into MySQL’s secure\_file\_priv folder





**3. Run the cleaning steps**



The script executes a full, structured cleaning pipeline.

Below is an overview aligned exactly with the SQL.



\## Detailed Cleaning Steps (Aligned with main.sql)



* Trim whitespace from all text fields



Ensures consistency before conversions.



* Normalize country names



**Example:**



"united arab emirates" → "uae"



* Clean location values



Remove suffixes like:

**, Non-U.S.**



* Convert date strings to actual MySQL DATE type



Both date and date\_added are cleaned and converted using STR\_TO\_DATE().



* Remove meaningless rows



Rows with BOTH:



1. total\_laid\_off = empty
2. percentage\_laid\_off = empty



are removed.



* Convert numeric text → typed numeric columns



Two new validated columns are created:



**total\_laid\_off\_int**



Converted only if numeric.



**percentage\_laid\_off\_decimal**



Converted only if a valid number.



Invalid values become NULL instead of breaking the dataset.



* Remove duplicates



Using a temporary auto-increment column + ROW\_NUMBER:



Duplicates are removed based on:



1. company
2. location
3. date
4. country



* Drop the original messy numeric columns



After validated columns are created, the VARCHAR versions are removed.



* Add continent column



Countries are mapped into:



1. North America
2. Europe
3. Asia
4. South America
5. Ocenia
6. Africa
7. Middle East
8. Other



* Summary of data completeness



The script ends with a SELECT that reports:



1. rows with both numeric fields
2. rows missing percentage
3. rows missing totals
4. rows missing both





\##Final Output



The final cleaned dataset lives in:



\*\*layoffs\_staging\*\*



It includes:



* clean dates



* validated integers and decimals



* continent



* month \& year



* cleaned location names



* standardized country names



* no duplicates



* no empty rows



This is the dataset you should use for:



* dashboards



* analysis



* Power BI



* visualizations





\## Notes \& Tips



* If the CSV date format changes, update the STR\_TO\_DATE() format string.



* Keeping layoffs\_raw untouched allows easy rollback



\## Future Enhancements



* Add automated validation rules



* Add a country lookup reference table



* Export cleaned data directly via SQL



