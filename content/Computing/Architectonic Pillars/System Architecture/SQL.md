
![[Pasted image 20260315215331.png]]


Table:  
Employees(id, name, dept_id, salary)
Department(id,name)

###### Highest Average Salary Department Wise 
```SQL
SELECT d.name, t.avg_salary
FROM (
    SELECT dept_id,
           AVG(salary) AS avg_salary,
           RANK() OVER (ORDER BY AVG(salary) DESC) AS rnk
    FROM Employees
    GROUP BY dept_id
) t
JOIN Department d
ON d.id = t.dept_id
WHERE rnk = 1;
```

Note: 
- The subquery inside parentheses produces a **derived table**.
- `t` is the alias assigned to that derived table.
- Columns available from `t`:
    - `dept_id`
    - `avg_salary`
    - `rnk`

The problem can be solved **without a subquery** using either a 
1. **window function with QUALIFY** (supported in some databases)
2. **HAVING + ALL pattern**. 
	```SQL
	SELECT d.name, AVG(e.salary) AS avg_salary
	FROM Employees e
	JOIN Department d
	ON e.dept_id = d.id
	GROUP BY d.id, d.name
	HAVING AVG(e.salary) >= ALL (
	    SELECT AVG(salary)
	    FROM Employees
	    GROUP BY dept_id);
	    
	Note:
	- `GROUP BY` computes average salary per department.
    - The inner query produces all department averages.
    - >= ALL ensures the departments average is greater than or equal to  
       every other department average..
    - If multiple departments tie for highest average, they all appear.
      
      
   Some databases support `QUALIFY` (Snowflake, BigQuery, Teradata).
   SELECT d.name,  
	AVG(e.salary) AS avg_salary,  
	RANK() OVER (ORDER BY AVG(e.salary) DESC) AS rnk  
	FROM Employees e  
	JOIN Department d  
	ON e.dept_id = d.id  
	GROUP BY d.id, d.name  
	QUALIFY rnk = 1;   
	
	
	Although technically a subquery, it is the **cleanest readable form**.
	
	A **Common Table Expression (CTE)** is a temporary named result set defined 
	within a SQL query using the `WITH` clause. It exists only for the duration  
	of that query and can be referenced like a table. It improves **readability,  
	modularity, and reusability** of complex queries.
	
	WITH dept_avg AS (
    SELECT dept_id, AVG(salary) AS avg_salary
	    FROM Employees
	    GROUP BY dept_id
	)
	SELECT d.name, avg_salary
	FROM dept_avg da
	JOIN Department d
	ON da.dept_id = d.id
	WHERE avg_salary = (
	    SELECT MAX(avg_salary)
	    FROM dept_avg
	);
	      
	```