# W3schools SQL Questions and Solutions

## Question 2

- What is the last name of the employee with the most orders?
- https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all


## Difficulty: Intermediate

## Solution:

- Create a CTE to count the orders done by each employee
- Filter the employee from the employees table with Conditions: having the id that in the list of ids having maximum orders.
```
WITH counter AS (
    SELECT EmployeeID AS id, COUNT(*) AS order_count
    FROM Orders
    GROUP BY EmployeeID
    )

SELECT LastName
FROM employees
WHERE employeeid IN (
    SELECT id 
    FROM counter
    WHERE order_count = (SELECT MAX(order_count) FROM counter));
```