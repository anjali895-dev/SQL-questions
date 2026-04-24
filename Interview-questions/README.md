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
## Q3 Write a query to find the first and last record for each employee based on hire_date.
```sql
--Method 1 — Simple MIN/MAX approach:
SELECT
	employee_id,
	name,
	MIN(hire_date) AS first_hire,
	MAX(hire_date) AS last_hire
FROM employees
GROUP BY employee_id,name;

--Method 2 — Using window functions for full row details:
SELECT *
FROM (
  SELECT *,
    FIRST_VALUE(hire_date) OVER (PARTITION BY dept_id
                                 ORDER BY hire_date) AS first_rec,
    LAST_VALUE(hire_date)  OVER (PARTITION BY dept_id
                                 ORDER BY hire_date
                                 ROWS BETWEEN UNBOUNDED PRECEDING
                                          AND UNBOUNDED FOLLOWING) AS last_rec
  FROM employees
) t;
```
## Q4 Write a query to display all employees who earn more than the average salary for their department.
```sql
--Method 1 — subquery with JOIN:
SELECT 
	e.employee_id,
	e.name,
	e.salary,
	e.dept_id
FROM employees e
JOIN(
	SELECT
	dept_id,
	AVG(salary)AS avg_sal
	FROM employees
	GROUP BY dept_id
)dept_avg ON e.dept_id = dept_avg.dept_id
WHERE e.salary > dept_avg.avg_sal;

--Method 2 — window function (cleaner):
SELECT
	employee_id,
	name,
	salary,
	dept_id
FROM (
	SELECT *,
	AVG(salary) OVER (PARTITION BY dept_id)AS dept_avg
	FROM employees
)t
WHERE salary > dept_avg;
```
## Q5 Write a query to list all employees who are also managers.
```sql
SELECT DISTINCT e.employee_id,
	e.name
FROM employees e
WHERE e.employee_id IN (
	SELECT DISTINCT manager_id
	FROM employees
	WHERE manager_id IS NOT NULL
);
```
## Q6 Write a query to find employees who joined in the same month and year.
```sql
SELECT
	e1.name AS emp1,
	e2.name AS emp2,
	MONTH(e1.hire_date) AS join_month,
	YEAR(e1.hire_date) AS join_year
FROM employees e1
JOIN employees e2 ON MONTH(e1.hire_date) = MONTH(e2.hire_date)
AND YEAR(e1.hire_date) = YEAR(e2.hire_date)
AND e1.employee_id < e2.employee_id;
```

## Q7 Write a SQL query to get employees older than the average age of their department.
```sql
SELECT
	employee_id,
	name,
	age,
	dept_id
FROM (
	SELECT *,
	AVG(age) OVER (PARTITION BY dept_id)AS dept_avg_age
	FROM employees
)t
WHERE age > dept_avg_age;
```

## Q8 Write a query to find the running total of orders for each customer sorted by order date.
```sql
WITH order_totals AS (
    SELECT
        o.customer_id,
        o.order_id,
        o.order_date,
        SUM(oi.total_amount) AS order_amount
    FROM dbo.orders o
    JOIN dbo.order_items oi ON o.order_id = oi.order_id 
    GROUP BY o.customer_id, o.order_id, o.order_date
)
SELECT
    customer_id,
    order_id,
    order_date,
    order_amount,
    SUM(order_amount) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS running_total
FROM order_totals
ORDER BY customer_id, order_date;
```
## Q9 Write a query to display the employee(s) with the longest tenure at the company.
```sql
--Method 1 — Simple approach — using MIN hire_date:
SELECT 
	employee_id,
	name,
	hire_date,
	CAST(DATEDIFF(YEAR, hire_date, GETDATE()) AS VARCHAR) + ' Years' AS tenure_date
FROM employees
WHERE hire_date = (SELECT MIN(hire_date) FROM employees);

--Method 2 — Using RANK to handle ties:
SELECT 
	employee_id,
	name,
	hire_date
FROM (
	SELECT *,
	RANK() OVER (ORDER BY hire_date ASC) AS rnk
	FROM employees
)t
WHERE rnk =1;
```
## Q10 Write a query to find the average order value by customer for each month.
```sql
SELECT 
	customer_id,
	YEAR(o.order_date) AS order_year,
	MONTH(o.order_date) AS order_month,
	CAST(AVG(oi.total_amount)AS DECIMAL(10,2)) AS avg_order_amount,
	COUNT(o.order_id) AS total_order
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY customer_id,
         YEAR(order_date),
         MONTH(order_date)
ORDER BY customer_id, order_year, order_month;
```
## Q11 Write a query to calculate the year-on-year growth of revenue.
```sql
WITH yearly_revenue AS (
  SELECT YEAR(o.order_date) AS yr,
         SUM(oi.total_amount) AS revenue
  FROM orders o
  JOIN order_items oi ON o.order_id = oi.order_id
  GROUP BY YEAR(o.order_date)
)
SELECT yr,
       revenue,
       LAG(revenue) OVER (ORDER BY yr) AS prev_year_revenue,
       ROUND(
         (revenue - LAG(revenue) OVER (ORDER BY yr)) * 100.0
         / LAG(revenue) OVER (ORDER BY yr), 2
       ) AS yoy_growth_pct
FROM yearly_revenue
ORDER BY yr;
```
## Q12 Write a query to find the Nth highest salary from the employee table.
```sql
--Method 1 — Replace N with any number (e.g. 3 for 3rd highest):
SELECT 
	salary
FROM (
	SELECT *,
		DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
	FROM employees
) ranked
WHERE rnk = 3;

--Method 2 — Subquery method (older SQL versions):
SELECT
	MIN(salary)
FROM (
	SELECT DISTINCT TOP 3 salary
	FROM employees
	ORDER BY salary DESC
) top_3;
```

## Q13 Write a query to find the most recent transaction for each customer.
```sql
SELECT *
FROM (
	SELECT *,
	ROW_NUMBER() OVER(
		PARTITION BY customer_id
		ORDER BY transaction_date DESC
		) AS rn
	FROM transactions
)t
WHERE rn = 1;
```
## Q14 Write a query to find all pairs of products that were ordered together at least once.
```sql
SELECT oi1.product_id AS product_1,
       oi2.product_id AS product_2,
       COUNT(DISTINCT oi1.order_id) AS times_together
FROM order_items oi1
JOIN order_items oi2
  ON oi1.order_id = oi2.order_id
 AND oi1.product_id < oi2.product_id
GROUP BY oi1.product_id, oi2.product_id
HAVING COUNT(DISTINCT oi1.order_id) >= 1
ORDER BY times_together DESC;
```
## Q15 Write a query to identify customers who made a purchase every month for the past year.
 ```sql
 SELECT 
	customer_id
FROM orders
WHERE order_date >= DATEADD (YEAR, -1, GETDATE())
GROUP BY customer_id
HAVING COUNT(DISTINCT
		CONCAT(YEAR(order_date),'-',MONTH(order_date))
) = 12;
```
## Q16 Write a query to find the total revenue for each region in a given quarter.
```sql
SELECT c.region,
       DATEPART(QUARTER, o.order_date) AS quarter,
       YEAR(o.order_date) AS yr,
       SUM(oi.total_amount) AS total_revenue
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_items oi	ON oi.order_id = o.order_id
WHERE YEAR(o.order_date) = 2026
  AND DATEPART(QUARTER, o.order_date) = 1
GROUP BY c.region,
         DATEPART(QUARTER, o.order_date),
         YEAR(o.order_date)
ORDER BY c.region;
```
## Q17 Write a query to find the first purchase date of each customer.
```SQL
--Method 1 — Simple approach:
SELECT 
	customer_id,
	MIN(order_date) AS first_purchase_date
FROM orders
GROUP BY customer_id
ORDER BY first_purchase_date;
	
--Method 2 — Full row details of first order:

SELECT *
FROM (
  SELECT *,
    ROW_NUMBER() OVER (
      PARTITION BY customer_id
      ORDER BY order_date ASC
    ) AS rn
  FROM orders
) t
WHERE rn = 1;
```
## Q18 Find the total salary of each department where total salary > 50,000.
```sql
SELECT
	dept_id,
	CAST(SUM(salary)AS DECIMAL(10,2)) AS total_salary
FROM employees
GROUP BY dept_id
HAVING SUM(salary) > 50000
ORDER BY total_salary DESC;
```
## Q19 Write a query to get the total number of employees hired per month and year.
```sql
SELECT 
	YEAR(hire_date)  AS hire_year,
    MONTH(hire_date) AS hire_month,
    COUNT(employee_id) AS employees_hired
FROM employees
GROUP BY YEAR(hire_date), MONTH(hire_date)
ORDER BY hire_year, hire_month;
```

## Q20 Write a query to delete all records from a table where the column value is NULL.
```sql
--Method 1 — Delete where a specific column is NULL:

DELETE FROM employees
WHERE salary IS NULL;

--Method 2 — Delete where ANY column is NULL:

DELETE FROM employees
WHERE salary IS NULL
	OR dept_id IS NULL
	OR name IS NULL;
```
