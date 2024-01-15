# Hackerrank SQL Questions and Solutions

## Question 2 
- https://www.hackerrank.com/challenges/earnings-of-employees/problem?isFullScreen=true

We define an employee's total earnings to be their monthly 'salary*months' worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as  space-separated integers.


## Difficulty: Easy

## Knowledge Review: 
- Create a new column in SELECT statement: \
SELECT salary*months AS earnings 
- GROUP BY Statement: The GROUP BY statement is often used with aggregate functions (COUNT(), MAX(), MIN(), SUM(), AVG()) to group the result-set by one or more columns.
- Window Function RANK(): (111,4, 5,6)
    - Syntax:  RANK() OVER (ORDER BY column (DESC))
- Window Function DENSERANK(): (111,2,3,4)
    - Syntax:  DENSERANK() OVER (ORDER BY column (DESC))


## Solution:

### Solution 1: Filter the first record
- Create a new column for earnings then count
- Group by earnings and reorder in desc order
- Retrieve the first result using LIMIT


>SELECT salary*months AS earnings, COUNT(*)\
FROM Employee\
GROUP BY earnings\
ORDER BY earnings DESC\
LIMIT 1

- Note: This solution has drawbacks as it does not work for other rankings such as 2nd, 3rd highest earnings.

### Solution 2: CTE and Subquery 
- Works on MySQL Server
- CTE to create total_earnings columns
- Query to extract maximum total_earnings and count the records 


> WITH earnings(total_earnings) AS(
>> SELECT salary * months AS total_earnings\
    FROM Employee
)

> SELECT total_earnings, COUNT(*)\
FROM earnings\
WHERE total_earnings IN (
>>SELECT MAX(total_earnings)\
FROM earnings\
)

> GROUP BY total_earnings

- Note: This solution only works with Maximum earnings or Minimum earnings

### Solution 3: Window Function
- Works on MySQL Server
- 1st CTE to create total_earnings columns
- 2nd CTE to rank earnings using RANK()
- Retrieve total_earnings and count where the rank is 1.

>WITH earnings(total_earnings) AS(\
 >>   SELECT salary * months AS total_earnings\
    FROM Employee\
),

> rank_earnings AS (
>> SELECT 
>>> total_earnings,\
>>> RANK() OVER (ORDER BY total_earnings DESC) AS earnings_rank

>> FROM earnings)
    
> SELECT total_earnings, COUNT(*)\
FROM rank_earnings\
WHERE earnings_rank = 1\
GROUP BY total_earnings

- This solution works for all rankings