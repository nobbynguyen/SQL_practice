# Leetcode SQL Questions and Solutions

## Question 1 
- https://leetcode.com/problems/department-highest-salary/

Table: Employee


| Column Name  | Type    |
|--------------|---------|
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |

id is the primary key column for this table.
departmentId is a foreign key of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.

Table: Department

|-------------|---------|
| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
|-------------|---------|
id is the primary key column for this table.
Each row of this table indicates the ID of a department and its name.

Write an SQL query to find employees who have the highest salary in each of the departments.

Return the result table in any order.


## Difficulty: Medium

## Knowledge Review: 
-


## Solution:

### Solution 1: Filter the first record
- Create a new column for earnings then count
- Group by earnings and reorder in desc order
- Retrieve the first result using LIMIT

```
SELECT salary*months AS earnings, COUNT(*)
FROM Employee
GROUP BY earnings
ORDER BY earnings DESC
LIMIT 1
```
- Note: This solution has drawbacks as it does not work for other rankings such as 2nd, 3rd highest earnings.

### Solution 2: CTE and Subquery 
- Works on MySQL Server
- CTE to create total_earnings columns
- Query to extract maximum total_earnings and count the records 

```
WITH earnings(total_earnings) AS(
    SELECT salary * months AS total_earnings
    FROM Employee
    )

SELECT total_earnings, COUNT(*)
FROM earnings
WHERE total_earnings IN (
    SELECT MAX(total_earnings)
    FROM earnings
    )
GROUP BY total_earnings
```
- Note: This solution only works with Maximum earnings or Minimum earnings

### Solution 3: Window Function
- Works on MySQL Server
- 1st CTE to create total_earnings columns
- 2nd CTE to rank earnings using RANK()
- Retrieve total_earnings and count where the rank is 1.

```
WITH earnings(total_earnings) AS(
    SELECT salary * months AS total_earnings
    FROM Employee
    ),
rank_earnings AS (
    SELECT 
        total_earnings,
        RANK() OVER (ORDER BY total_earnings DESC) AS earnings_rank
    FROM earnings)

SELECT total_earnings, COUNT(*)
FROM rank_earnings
WHERE earnings_rank = 1
GROUP BY total_earnings
```
- This solution works for all rankings