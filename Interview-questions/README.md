# Interview questions

## Q1 Write a query to find the 2nd highest salary from an employee table.
```sql
-- Method 1 — using subquery:
SELECT 
	MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary)FROM employees);

--Method 2 — using DENSE_RANK (recommended):
SELECT
	salary
FROM (
	SELECT
		salary,
		DENSE_RANK() OVER(ORDER BY salary DESC) AS rnk
	FROM employees
)ranked
WHERE rnk = 2;
```
## Q2 Write a SQL query to find employees who have the same manager.
```sql
SELECT e1.name AS employee1,
       e2.name AS employee2,
       e1.manager_id
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.manager_id
AND e1.employee_id < e2.employee_id
ORDER BY e1.manager_id;
```
