# Hackerrank SQL Questions and Solutions

## Question 3 
- https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=true

Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.


## Difficulty: Medium

## Knowledge Review: 
- Create a new column in SELECT statement: \
SELECT salary*months AS earnings 
- GROUP BY Statement: The GROUP BY statement is often used with aggregate functions (COUNT(), MAX(), MIN(), SUM(), AVG()) to group the result-set by one or more columns.
- Window Function RANK(): (111,4, 5,6)
    - Syntax:  RANK() OVER (ORDER BY column (DESC))
- Window Function DENSERANK(): (111,2,3,4)
    - Syntax:  DENSERANK() OVER (ORDER BY column (DESC))


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
>> challenges_created = (SELECT Max(challenges_created) FROM challenges_count) OR
>>challenges_created IN (
>>> SELECT challenges_created 
            FROM challenges_count\
            GROUP BY challenges_created\
            HAVING COUNT(*)=1\
            )
>ORDER BY  cc.challenges_created DESC, h.hacker_id
