# Hackerrank SQL Questions and Solutions

## Question 4
- https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true

You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too!
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of  from your result.


## Difficulty: Medium

## Knowledge Review: 
### HAVING clause syntax: 
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s); 
### CTE - Common Table Expression
CTEs act as virtual tables (with records and columns) that are created during query execution, used by the query, and deleted after the query executes.


## Solution: CTE and Subquery (MS SQL Server)
- Step 1: The total score of a hacker is the sum of their maximum scores for all of the challenges. Therefore, I use CTE as a virtual table to extract the maximum score of each hacker and challenge. 
- Step 2: Join two tables to get the name, id, total of maximum scores from the cte and hackers table.
- Step 3: Add the condition of total of maximum scores higher than 0 using HAVING clause
- Step 4: Sort the results

>WITH max_score(hacker_id, challenge_id, max_score) AS ( 

>>SELECT hacker_id, challenge_id, MAX(score)AS max_score\
    FROM submissions\
    GROUP BY hacker_id, challenge_id\
)\

>SELECT mc.hacker_id, h.name, SUM(mc.max_score) AS score\
FROM max_score AS mc\
INNER JOIN hackers AS h ON mc.hacker_id=h.hacker_id\
GROUP BY mc.hacker_id, h.name\
HAVING SUM(mc.max_score)>0\
ORDER BY score DESC, hacker_id

