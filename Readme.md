# Layoffs Data Cleaning \& Processing (MySQL)



This project documents the full data-cleaning pipeline I built to transform a messy layoffs dataset into a clean, analysis-ready table using MySQL. The goal was to take a raw CSV export and systematically clean, validate, and standardize the data while keeping the process repeatable.



Everything here reflects the actual steps I took while working through the dataset.



### Project Files



**main.sql**

Full SQL workflow: schema creation → raw import → cleaning → type conversions → duplicate removal.



**layoffs.csv**

The unmodified dataset I started with (exported from Excel).



### Project Approach



I split the workflow into two main tables:



1. **layoffs\_raw**



A flexible, all-VARCHAR landing table meant to take the CSV exactly as it comes.

No assumptions. No type errors. Just the raw data.



2\. **layoffs\_staging**



A second table used for all cleaning work:

* trimming whitespace
* converting date strings
* validating and converting numbers
* handling missing fields
* removing duplicates
* creating cleaned numeric/date columns
* dropping the original messy ones



Separating raw vs. cleaned data made the cleanup process easier to test and re-run at any time.



### How to Run the Script



**1. Create the Database + Tables**



Run the first section of main.sql to create:



* the layoffs\_db database
* layoffs\_raw (raw import table)
* layoffs\_staging (cleaned table)



**2. Import the CSV**



Once layoffs\_raw exists, import your CSV file.



LOAD DATA INFILE '/path/to/layoffs.csv'

INTO TABLE layoffs\_raw

FIELDS TERMINATED BY ','

ENCLOSED BY '"'

LINES TERMINATED BY '\\n'

IGNORE 1 ROWS;



**Notes**



* You may need to adjust the path depending on your OS.
* MySQL might require placing the CSV inside the directory allowed by secure\_file\_priv.





**3. Run the Data Cleaning Sectio**n



The rest of main.sql performs the actual transformations.

Here are the major steps I implemented:



* &nbsp;Trim all strings



Removes leading/trailing whitespace from every column.



* &nbsp;Convert date fields to proper DATE type



Used STR\_TO\_DATE() assuming MM/DD/YYYY format.



* &nbsp;Remove fully empty entries



If both total layoffs + percentage layoffs were blank, the row is removed.



* &nbsp;Safely convert numeric fields



1. total\_laid\_off → INT
2. percentage\_laid\_off → DECIMAL(5,2)

Values are converted only when valid.



* &nbsp;Identify and remove duplicate rows



Used ROW\_NUMBER() in a CTE, based on:

company, location, date, country



* &nbsp;Drop old raw text columns



Once the cleaned columns were created and validated, the original VARCHAR versions were removed.





### Final Output



Your final, cleaned dataset is stored in:



###### **layoffs\_staging**



This table contains:



* real date types
* validated numeric columns
* standardized text
* no duplicate rows
* no empty placeholder entries



This is the table to use for any analysis, dashboards, or visualizations.



### Notes \& Tips



* If your dates use a different format, update the STR\_TO\_DATE() format string.
* Windows users may need to use LINES TERMINATED BY '\\r\\n'.
* Because the workflow is fully scripted, you can clear the tables and re-run the whole process anytime.
