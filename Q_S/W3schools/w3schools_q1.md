# W3schools SQL Questions and Solutions

## Question 1

- How many orders were shipped by Speedy Express in total?
- https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all



## Difficulty: Easy

## Solution:

### Solution 1: Filter the first record
- Join table Orders and table Shippers
- Filter the orders shipped by Speed Express
- Count the orders

>SELECT COUNT(*)
FROM Orders AS o
INNER JOIN Shippers AS sh ON o.shipperid=sh.shipperid
WHERE sh.shippername="Speedy Express";

### Solution 2: Nested Query 

> SELECT COUNT(*)
FROM Orders
WHERE shipperid=(SELECT shipperid FROM shippers WHERE shippername="Speedy Express");