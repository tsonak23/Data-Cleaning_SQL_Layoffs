 Data-Cleaning_Oracle SQL
 
<!-- BLUE INSTRUCTIONS -->
<div style="color:#1f6feb; font-weight:600;">
🔹 ** Project Title : Layoffs_In_Tech Firm_Data cleaning using Oracle SQL.**
Workflow:  Sequence followed for this data cleaning project  will be:
           1.Creating another copy of working table to make sure we do not run out with raw data in any case. 
           2.Check and remove duplicates if any.
           3.Standardize data and remove errors. 
           4.Look for null values and understand if required or to remove.
           5.Remove columns or rows that are not necessary.

Query with explanation : 

After creating a copy of raw data , we will be using **row-num by partition** function as there was no unique key column in the dataset.

</div>

<br>

<!-- ORANGE CODE BOX -->
<div style="background-color:#0d1117; color:#f0883e; padding:12px; border-radius:8px; font-family:monospace;">

SELECT
    ROW_NUMBER()
    OVER(PARTITION BY company, location, industry, total_laid_off, percentage_laid_off,
                      layoff_date, stage, country, funds_raised_millions
         ORDER BY
             company DESC
    ) AS rom_num
FROM
    layoffs_staging;
</div>



 Project Title : Layoffs_In_Tech Firm_Data cleaning using Oracle SQL.

 Workflow:  Sequence followed for this data cleaning project  will be:
           1.Creating another copy of working table to make sure we do not run out with raw data in any case. 
           2.Check and remove duplicates if any.
           3.Standardize data and remove errors. 
           4.Look for null values and understand if required or to remove.
           5.Remove columns or rows that are not necessary.

Query with explanation : 

--After creating a copy of raw data , we will be using **row-num by partition** function as there was no unique key column in the dataset.

SELECT
    ROW_NUMBER()
    OVER(PARTITION BY company, location, industry, total_laid_off, percentage_laid_off,
                      layoff_date, stage, country, funds_raised_millions
         ORDER BY
             company DESC
    ) AS rom_num
FROM
    layoffs_staging;


  
           
 Learnings : 



