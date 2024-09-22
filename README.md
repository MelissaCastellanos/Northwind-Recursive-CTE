# Northwind Database Employee Hierarchy – Recursive CTE 
## What is a Recursive CTE?  
Recursive CTEs are common table expressions that reference themselves one or more times until a given condition is met (kind of like a WHILE loop). They begin with the execution of the anchor member, which is used to initialize the first part of the query. It contains a simple SELECT statement to retrieve a subset of data which serves as the starting point for the recursive member. The recursive member is the repetitive member of the query. It contains a SELECT statement that references the CTE itself and loops until the termination condition is met. Once both queries run, the UNION ALL statement joins the query results and allows the return of the final result.  
  
![Recursive CTE](https://builtin.com/sites/www.builtin.com/files/styles/ckeditor_optimize/public/inline-images/8_recursive-sql.jpg)  
  
## The Northwind Employee Hierarchy  
The following code uses a recursive CTE to create a hierarchy of the employees listed in the Northwind database. The EmployeesHierarchy CTE created begins by defining the anchor/base query. This query retrieves the employees that have no supervisor (ReportsTo is NULL) and identifies them as Level 1. The recursive query then iteratively joins employees to their supervisors (ReportsTo field) and increases the Level by 1 with each loop. This process creates an employee hierarchy which is then ordered from top to bottom.  
```
--SQL 
with EmployeesHierarchy  AS 
(
-- Anchor Member/ Base Query
SELECT 
	[EmployeeID],
	[LastName],
	[FirstName],
	[ReportsTo],
	1 AS Level
FROM [Northwind].[dbo].[Employees]
WHERE [ReportsTo] is NULL
UNION ALL 
--Recursive Query
SELECT 
	Employee.[EmployeeID],
	Employee.[LastName],
	Employee.[FirstName],
	Employee.[ReportsTo],
	Level + 1
FROM [Northwind].[dbo].[Employees] AS Employee
JOIN EmployeesHierarchy 
ON Employee.ReportsTo = EmployeesHierarchy.EmployeeID
)
SELECT * FROM EmployeesHierarchy ORDER BY Level  
```
