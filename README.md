# SQL Challenge - Employee Database
It is a beautiful spring day, and it is two weeks since you have been hired as a new data engineer at Pewlett Hackard. Your first major task is a research project on employees of the corporation from the 1980s and 1990s. All that remain of the database of employees from that period are six CSV files.
In this assignment, you will design the tables to hold data in the CSVs, import the CSVs into a SQL database, and answer questions about the data. In other words, you will perform:
- Data Engineering
- Data Analysis

## SQL Challenge Structure
```
SQL Challenge
|__ data/                   # contains employee data
|
|__ images/                 # contains images of employee_db ERD and output graphs
|
|
|__ .gitignore              # gitignore file
|
|__ README.md               # readme file
|
|__ queries.sql             # contains query analysis for employee database
|
|__ queries_bonus.ipynb     # contains bonus on SQLAlchemy
|
|__ sql_password.py         # contains password for connecting to PostgreSQL
|
|__ table_schema.sql        # contains schema for employee database tables
```

## Data Modelling:
- ERD and table schema constructed using http://www.quickdatabasediagrams.com
  
## Data Engineering:
- Create a table schema for each CSV file, specifying data types, foreign keys and other constraints
- Import each CSV into the relevant SQL table

## Dependencies and Setup
```
from sqlalchemy import create_engine
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from sql_password import password    ## ENSURE THAT PASSWORD IS SET IN sql_password.py ##
```


## Data Analysis
1. List the following details of each employee: employee number, last name, first name, sex and salary.
```
SELECT e.emp_no, e.last_name, e.first_name, e.sex, s.salary
FROM employees AS e
INNER JOIN salaries AS s ON e.emp_no = s.emp_no;
```
2. List first name, last name and hire date for employees who were hired in 1986.
```
SELECT first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1986-12-31';
```
3. List the manager of each department with the following information: department number, department name, the manager's employee number, last name and first name.
```
SELECT d.dept_no, d.dept_name, dm.emp_no, e.last_name, e.first_name
FROM departments AS d
INNER JOIN department_manager AS dm ON d.dept_no = dm.dept_no
INNER JOIN employees AS e ON dm.emp_no = e.emp_no;
```
4. List the department of each employee with the following information: employee number, last name, first name and department name.
```
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM employees AS e
INNER JOIN department_employees AS de ON e.emp_no = de.emp_no
INNER JOIN departments AS d ON d.dept_no = de.dept_no;
```
5. List first name, last name and sex for employees whose first name is "Hercules" and last names begin with "B."
```
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules'
AND last_name LIKE 'B%';
```
6. List all employees in the Sales department, including their employee number, last name, first name and department name.
```
SELECT de.emp_no, e.last_name, e.first_name, d.dept_name
FROM employees AS e
INNER JOIN department_employees AS de ON e.emp_no = de.emp_no
INNER JOIN departments AS d ON d.dept_no = de.dept_no
WHERE dept_name = 'Sales';
```
7. List all employees in the Sales and Development departments, including their employee number, last name, first name and department name.
```
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM employees AS e
INNER JOIN department_employees AS de ON e.emp_no = de.emp_no
INNER JOIN departments AS d ON d.dept_no = de.dept_no
WHERE dept_name = 'Sales' OR dept_name = 'Development';
```
8. In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
```
SELECT last_name, COUNT(last_name) AS Frequency
FROM employees
GROUP BY last_name
ORDER BY Frequency DESC;
```

## Bonus

1. Import SQL database into Pandas using SQLAlchemy
```
# Create Engine and connect
from sqlalchemy import create_engine
engine = create_engine(f'postgresql://postgres:{password}@localhost:5432/employees_db')
connection = engine.connect()
```  
2. Create a histogram to visualize the most common salary ranges for employees.  
![chart](images/salary_ranges_for_employees.png) 

3. Create a bar chart of average salary by title.  
![chart](images/average_salary_per_title.png)   
