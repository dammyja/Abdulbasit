                           HR Analytics Capstone Overview
Data Cleaning and Transformation Process – HR Analytics Capstone
Overview
The Raw_Employee Data sheet was carefully cleaned and standardized to make the dataset reliable for analysis, Pivot Tables, and dashboard creation. The process involved removing inconsistencies, correcting data types, filling missing values, creating calculated columns, and linking the dataset to the Lookup Department and Lookup_Performance sheets using VLOOKUP.
1. Cleaning and Standardizing the Raw Employee Data
Removing duplicates
Completed
Duplicate employee records were removed using Data → Remove Duplicates. The Emp ID column was used as the primary unique identifier to ensure that each employee appeared only once in the dataset.
Standardizing data types
Completed
Each column was converted to the appropriate Excel data type:
Column	Data Type
Emp ID	Text
Full Name	Text
Dept Code	Text
Hire Date	Date (Short Date)
Salary	Number/Currency
Perf_Score	Whole Number
Employment Status	Text
The Hire Date column was formatted as Short Date so all dates appeared consistently and aligned properly throughout the table.
Using TRIM to clean department codes
Completed
Some department codes contained extra spaces, for example IT02 . To avoid lookup errors, the TRIM function was used inside the VLOOKUP formula:
Department lookup formula
Copy
=VLOOKUP(TRIM(C2),'Lookup Department'!$A$1:$B$7,2,FALSE)
TRIM(C2) removes leading and trailing spaces before the lookup is performed. This ensured that values such as IT02 matched IT02 correctly in the lookup table.
2. Connecting the Department Lookup Table
The Lookup Department sheet contained the mapping between department codes and department names.
Dept_Code	Department_Name
HR01	Human Resources
IT02	Information Technology
FIN03	Finance
MKT04	Marketing
SAL05	Sales
OPS06	Operations
The Department_Name column in the raw table was populated with:
VLOOKUP with TRIM
Copy
=VLOOKUP(TRIM(C2),'Lookup Department'!$A$1:$B$7,2,FALSE)
This formula searches the department code in column C and returns the full department name from the lookup sheet.
3. Creating the Performance Band
The Lookup_Performance sheet defined score ranges and bonus percentages.
Score Range	Band
0–49	Needs Improvement
50–69	Developing
70–84	Achieving
85–94	Exceeding
95–100	Outstanding
The Performance Bond column in the raw table was created using a nested IF formula:
Performance band formula
Copy
This formula converts the numeric Perf_Score into a descriptive performance category.
4. Calculating Eligible Bonus Amount
After the performance band was created, the bonus amount was calculated from the employee salary and bonus percentage.
Eligible bonus formula
Copy
This formula automatically calculates the projected bonus based on the employee’s performance category.
5. Calculating Years of Service with DATEDIF
The Years of Service column was calculated from the hire date using DATEDIF.
Years of service formula
Copy
Explanation:
•	D2 = Hire Date
•	TODAY() = current date
•	"Y" returns complete years of service
•	"YM" returns remaining months after the full years
Example: an employee hired on 24-Dec-2017 displays 8 Years, 6 Months (depending on the current date).
6. Combining First and Last Name into Full Name
When first and last names are stored separately, they can be combined safely using either CONCAT or TEXTJOIN.
Using CONCAT
Copy
=CONCAT(A2," ",B2)
This joins the first name in A2 and the last name in B2 with a space between them.
Using TEXTJOIN
Copy
=TEXTJOIN(" ",TRUE,A2,B2)
TEXTJOIN is more reliable because it ignores blank cells automatically.
Example: Emily + Brown becomes Emily Brown.
7. Handling Missing Salary Values
Blank salary cells were filled using the average salary of the dataset.
Step 1: Calculate the average salary
Average salary
Copy
=AVERAGE(F:F)
Step 2: Fill blanks
Fill missing salary
Copy
=IF(F2="",$M$1,F2)
Where M1 contains the calculated average salary. This method preserves existing salaries while replacing only blank cells.
8. Working with Employment Status
The Employment Status column was standardized so that values such as Active, Left, and Resigned were consistent across the dataset.
The Attrition Rate helper column then identified employees who had left the organization:
Attrition flag formula
Copy
This formula returns Yes for employees who left or resigned and No for all active employees.
9. Building the Attrition Rate Table
The attrition analysis was created from the cleaned raw table using a Pivot Table.
Pivot Table setup
•	Rows: Department_Name
•	Values: Count of Emp ID
•	Filter: Attrition Rate = Yes
Attrition rate formula
The attrition percentage was calculated as:
Attrition Rate = (Employees Who Left ÷ Total Employees) × 100
This produced the department-level attrition percentages used in the dashboard.
10. Final Data Quality Checks
Before creating Pivot Tables and the dashboard, the following checks were completed:
•	All dates displayed in Short Date format.
•	Text fields cleaned with TRIM.
•	No duplicate Emp ID values.
•	Missing salary values filled with the dataset average.
•	Department names successfully returned from the lookup table.
•	Performance bands correctly matched to score ranges.
•	Bonus amounts calculated without errors.
•	Years of service updated automatically using TODAY().
•	Employment status standardized for accurate attrition reporting.
The Raw_Employee Data sheet was transformed from a messy HR export into a clean, analysis-ready dataset. By using TRIM, VLOOKUP, DATEDIF, CONCAT, TEXTJOIN, IF, and IFS functions, the dataset was successfully connected to the lookup tables, enriched with calculated columns, standardized for reporting, and prepared for Pivot Table analysis and dashboard visualization. These steps ensured accurate department mapping, performance classification, bonus calculation, service-year tracking, and attrition measurement across the organization.
Important correction for your report
The column in your workbook is labeled “Performance Bond”, but based on the lookup table it actually represents a Performance Band (Needs Improvement, Developing, Achieving, Exceeding, Outstanding). In your written report, use Performance Band instead of Performance Bond for better accuracy and professionalism.


