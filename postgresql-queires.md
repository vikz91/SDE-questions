# PostgreSQL Queries

## SELECT and WHERE clauses:
```sql
-- Question: Write a query to retrieve all employees whose salary is greater than $50,000 from the 'employees' table.

SELECT * 
FROM employees 
WHERE salary > 50000;
```

## JOINs (INNER, LEFT, RIGHT, and FULL):
```sql
-- Question: Write a query to retrieve the employee names and their department names from the 'employees' and 'departments' tables.

SELECT e.name AS employee_name, d.name AS department_name 
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

## Aggregate Functions and GROUP BY:
```sql
-- Question: Write a query to find the total salary paid per department from the 'employees' table.

SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id;
```

## HAVING clause:
```sql

-- Question: Write a query to find departments with more than five employees in the 'employees' table.

SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING employee_count > 5;
```

## Subqueries:
```sql
-- Question: Write a query to retrieve employees with a salary higher than the average salary in their department.

SELECT * 
FROM employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e1.department_id = e2.department_id
);
```


## Window Functions:
```sql
-- Question: Write a query to rank employees within each department by their salary in descending order.

SELECT name, department_id, salary, RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
FROM employees;
```


## Common Table Expressions (CTEs):
```sql
-- Question: Write a query to calculate the total salary per department and return departments with a total salary greater than $100,000.

WITH department_salaries AS (
    SELECT department_id, SUM(salary) AS total_salary
    FROM employees
    GROUP BY department_id
)
SELECT department_id, total_salary
FROM department_salaries
WHERE total_salary > 100000;
```

## Data Manipulation Language (DML) - INSERT, UPDATE, DELETE:
```sql
-- Question: Write a query to update the salary of employee with ID 3 to $60,000 in the 'employees' table.

UPDATE employees
SET salary = 60000
WHERE id = 3;
```

## ALTER TABLE statement:
```sql
-- Question: Write a query to add a new column 'email' to the 'employees' table.

ALTER TABLE employees
ADD COLUMN email VARCHAR(255);
```

## CREATE INDEX:
```sql
-- Question: Write a query to create an index on the 'salary' column in the 'employees' table.

CREATE INDEX idx_salary
ON employees (salary);
```
