# Pewlett-Hackard-Analysis
## Environment
SQL
PostgreSQL 11.16
pgAdmin

## Overview
The purpose of the project is to build a database analysis for Pewlett Hackard in order to help the company to look ahead and prepare for the “silver tsunami” as many current employees reach retirement. Also, the analysis will help to identify the employees eligible for a mentorship program.

## Results    
* By: 1) filtering the ***employees*** table: (birth_date BETWEEN '1952-01-01' AND '1955-12-31') AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31')   
      2) joining the ***employees*** table with the ***dept_emp*** table  
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),  
  we can obtain the count of the employees that are ready for retirement by department.  

![Retirement count by dept](https://github.com/MarcoFernandez14/Pewlett-Hackard-Analysis/blob/main/Data/Retirement%20count%20by%20dept.png)   

* By: 1) filtering the ***employees*** table: (birth_date BETWEEN '1952-01-01' AND '1955-12-31')  
      2) joining the ***employees*** table with the ***titles*** table  
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),  
  we can obtain the count of unique employees titles that are ready for retirement.    

![Retiring titles](https://github.com/MarcoFernandez14/Pewlett-Hackard-Analysis/blob/main/Data/Retiring%20titles.png)    

* By: 1) filtering the ***employees*** table: (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')  
      2) joining the ***employees*** table with the ***titles*** table and the ***dept_emp*** table   
      3) filtering only the current employees in the joined table: o_date = ('9999-01-01'),  
  we can obtain a list of 1,549 unique employees that can be part of the mentorship program.    

![Mentorship eligibility](https://github.com/MarcoFernandez14/Pewlett-Hackard-Analysis/blob/main/Data/Mentorship%20elegibility.png)    

* The mentorship eligibility table can be grouped by tittle to obtain the table below.  

![Mentorship eligibility by tittle](https://github.com/MarcoFernandez14/Pewlett-Hackard-Analysis/blob/main/Data/Mentorship%20elegibility%20by%20title.png)    
 
## Summary
It seems that if we consider current employees that were hired between 1952 an d1955, Pewlett-Hackard should prepare to fill 72,458 vacant positions once the "silver tsunami" begins to make an impact.  
When we analyze the ratio of employees qualified to mentor versus the retiring employess by department, we can observe that the ratio is around 2% in all departments. Pewlett-Hackard should determine if that ratio is enough to train all the newcomers efficiently. Please see image below.  
![Retiring vs mentor by department](https://github.com/MarcoFernandez14/Pewlett-Hackard-Analysis/blob/main/Data/Retiring%20vs%20mentor%20by%20department.png)  

In order to obtain this information, the additional queries are:

-------------------------------------------------
-- Retiring by deparment
SELECT e.emp_no, 
	e.first_name,
	e.last_name,
	de.dept_no, 
    de.to_date
INTO retiring_deptno
FROM employees AS e
	JOIN dept_emp as de
		ON (e.emp_no = de.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY emp_no ASC;

SELECT rd.emp_no, 
	rd.dept_no,
	rd.to_date,
    d.dept_name
INTO retiring_deptname
FROM retiring_deptno AS rd
	JOIN departments as d
		ON (rd.dept_no = d.dept_no)
ORDER BY emp_no ASC;

-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (rn.emp_no) rn.emp_no,
rn.to_date,
rn.dept_name
INTO unique_dept
FROM retiring_deptname AS rn
WHERE (rn.to_date = '9999-01-01')
ORDER BY emp_no, rn.to_date DESC;

-- Retiring Departments table
SELECT COUNT(ud.emp_no),
    ud.dept_name
INTO retiring_dept
FROM unique_dept as ud
GROUP BY ud.dept_name
ORDER BY count DESC;

------------------------------------
--Mentorship by deparment
SELECT e.emp_no, 
	e.first_name,
	e.last_name,
	de.dept_no, 
    de.to_date
INTO mentor_deptno
FROM employees AS e
	JOIN dept_emp as de
		ON (e.emp_no = de.emp_no)
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY emp_no ASC;

SELECT md.emp_no, 
	md.dept_no,
	md.to_date,
    d.dept_name
INTO mentor_deptname
FROM mentor_deptno AS md
	JOIN departments as d
		ON (md.dept_no = d.dept_no)
ORDER BY emp_no ASC;

-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (mn.emp_no) mn.emp_no,
mn.to_date,
mn.dept_name
INTO mentorunique_dept
FROM mentor_deptname AS mn
WHERE (mn.to_date = '9999-01-01')
ORDER BY emp_no, mn.to_date DESC;

-- Mentorship Departments table
SELECT COUNT(md.emp_no),
    md.dept_name
INTO mentor_dept
FROM mentorunique_dept as md
GROUP BY md.dept_name
ORDER BY count DESC;

-- Retiring vs Mentorship by department
SELECT rd.count AS count_retiring,
    md.count AS count_mentor,
	rd.dept_name
	--INTO retvsmen_deptname
FROM retiring_dept AS rd
	JOIN mentor_dept as md
		ON (rd.dept_name = md.dept_name)
ORDER BY dept_name ASC;


