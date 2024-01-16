# Hackerrank SQL Questions and Solutions

## Question 3 
- https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=true

Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.


## Difficulty: Medium

## Knowledge Review: 
### The HAVING clause: is added to SQL because the WHERE keyword cannot be used with aggregate functions. 
### Subquery
- A subquery is a SQL query nested inside a larger query. A subquery may occur in :
    - A SELECT clause
    - A FROM clause
    - A WHERE clause
- A subquery is usually added within the WHERE Clause of another SQL SELECT statement.
- A subquert can be used in a SELECT, INSERT, DELETE, or UPDATE statement to perform the following tasks:
    - Compare an expression to the result of the query.
    - Determine if an expression is included in the results of the query.
    - Check whether the query selects any rows.


## Solution: CTE and Subquery 
- Step 1: query the id, count the number of challenges for each hacker
- Step 2: join two tables to get the name, with condition of maximum count.
	Keep the records if (1) 2 records has same challenges created AND challenges created equals to maximum challenges created\
	Keep the records if only that records have that number of challenges created (select from a list of unique number of challenges created).
- Step 3: sort the results


> WITH challenges_count(hacker_id, challenges_created) AS(
>>   SELECT hacker_id, COUNT(*)\
        FROM challenges
        GROUP BY hacker_id
        )

> SELECT h.hacker_id, h.name, cc.challenges_created
FROM hackers AS h\
LEFT JOIN challenges_count AS cc ON h.hacker_id=cc.hacker_id\
WHERE
>> challenges_created = (SELECT Max(challenges_created) FROM challenges_count) OR\
>>challenges_created IN (
>>> SELECT challenges_created
            FROM challenges_count\
            GROUP BY challenges_created\
            HAVING COUNT(*)=1\
            )\
>ORDER BY  cc.challenges_created DESC, h.hacker_id
