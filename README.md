Data-Cleaning_Oracle SQL
 
 ** Project Title : Layoffs_In_Tech Firm_Data cleaning using Oracle SQL.**
 
 Workflow: Sequence followed for the data cleaning project will be:

           1.Creating another copy of working table to make sure we do not run out with raw data in any case.
           2.Check and remove duplicates if any.
           3.Standardize data and remove errors.
           4.Look for null values and understand if required or to remove.
           5.Remove columns or rows that are not necessary.


Query with explanation :

--After creating a copy of raw data , added a column using partition for finding duplicates--


SELECT
    ROW_NUMBER()
    OVER(PARTITION BY company, location, industry, total_laid_off, percentage_laid_off,
                      layoff_date, stage, country, funds_raised_millions
         ORDER BY
             company DESC
    ) AS rom_num
FROM
    layoffs_staging;

WITH Duplicate_CTE AS (
SELECT  company, location, industry, total_laid_off, percentage_laid_off, Layoff_date, stage,country, funds_raised_millions,
row_number ()
over(
PARTITION by company, location, industry, total_laid_off, percentage_laid_off, Layoff_date, stage,country, funds_raised_millions
order BY company DESC) as rom_num
FROM layoffs_staging)

select * from duplicate_cte
where row_num>1;


--Created copy of working table with an addition of row_num column--

insert into layoffs_staging2 (company, location, industry, total_laid_off, percentage_laid_off, Layoff_date, stage,country, funds_raised_millions,row_num)
SELECT  company, location, industry, total_laid_off, percentage_laid_off, Layoff_date, stage,country, funds_raised_millions ,
row_number ()
over(
PARTITION by company, location, industry, total_laid_off, percentage_laid_off, Layoff_date, stage,country, funds_raised_millions
order BY company DESC) as row_num
FROM layoffs_staging;

select * from layoffs_staging2;

--Checked and deleted all the rows having duplicates--

Delete
from layoffs_staging2
where row_num>1;

select * from layoffs_staging2;


--Removed unwanted white spaces from column 'Company'--

select company,trim(company)
from layoffs_staging2

update layoffs_staging2
set company = trim(company)


--Cleaned the 'Industry' column by removing extra spaces from identical values--

select * from layoffs_staging2
where industry like 'Crypto%'

update layoffs_staging2
set industry = 'Crypto'
where industry like 'Crypto%'


--Cleaned the 'Location' column by removing extra spaces from identical values--


select distinct(location) from layoffs_staging2

select distinct(country) from layoffs_staging2
order by 1

select distinct(country), trim(trailing '.' from country)
from layoffs_staging2

update layoffs_staging2
set industry = trim(trailing '.' from country)
where country like 'United States%'

select * from layoffs_staging2;


--Changed data format--

select to_char(layoff_date, 'MM/DD/YYYY') FROM layoffs_staging2

update layoffs_staging2
set layoff_date = to_char(layoff_date, 'MM/DD/YYYY');


--Checked for the rows where both columns mentioned are null or blank--

select * from layoffs_staging
where total_laid_off is null and
percentage_laid_off is null

select distinct(industry) from layoffs_staging2
select * from layoffs_staging2
where industry is null
or
industry = '';


--Populated null values for mentioned columns--

MERGE INTO layoffs_staging2 t1
USING layoffs_staging2 t2
ON (t1.company = t2.company)
WHEN MATCHED THEN
UPDATE SET t1.industry = t2.industry
WHERE t1.industry IS NULL
  AND t2.industry IS NOT NULL;


 UPDATE layoffs_staging2 t1
SET industry = (
    SELECT MIN(t2.industry)
    FROM layoffs_staging2 t2
    WHERE t1.company = t2.company
      AND t2.industry IS NOT NULL
)
WHERE (t1.industry IS NULL OR t1.industry = '');



--Deleted rows where both mentioned columns have null values--

select * from layoffs_Staging2
where total_laid_off is null
and percentage_laid_off is null

Delete
from layoffs_Staging2
where total_laid_off is null
and percentage_laid_off is null;


--Removed row column--

select * from layoffs_staging2;

alter table layoffs_staging2
drop column row_num;
