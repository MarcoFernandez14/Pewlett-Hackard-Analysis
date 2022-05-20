# Pewlett-Hackard-Analysis
## Evironment
SQL
PostgreSQL 11.16
pgAdmin

## Overview
The purpose of the porject is to build a database analysis for Pewlett Hackard in order to help the company to look ahead and prepare for the “silver tsunami” as many current employees reach retirement. Also, the analysis will help to identify the employees elegible for a mentorship program.

## Results  
* By: 1) filtering the ***employees*** table: (birth_date BETWEEN '1952-01-01' AND '1955-12-31') AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31') 
      2) joining the ***employees*** table with the ***dept_emp*** table  
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),
  we can obtain the count of the employees that are ready for retirement by department.  

![Retirement count by dept]()   

* By: 1) filtering the ***employees*** table: (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
      2) joining the ***employees*** table with the ***titles*** table  
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),
  we can obtain the count of unique employees titles that are ready for retirement.  

![Retiring titles]()  

* By: 1) filtering the ***employees*** table: (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
      2) joining the ***employees*** table with the ***titles*** table and the ***dept_emp*** table 
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),
  we can obtain a list of 1,449 unique employees that can be part of the mentorship program.  

![Mentorship elegibility]()  

* The mentorship elegibility table can be groupe by tittle to obtain the table below.

![Mentorship elegibility by tittle]()  
 
  
    
  
      






## Summary: Provide high-level responses to the following questions, then provide two additional queries or tables that may provide more insight into the upcoming "silver tsunami."
How many roles will need to be filled as the "silver tsunami" begins to make an impact?
Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?
