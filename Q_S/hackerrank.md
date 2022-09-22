# Hackerrank SQL Questions and Solutions

## Question 1 
### https://www.hackerrank.com/challenges/african-cities/problem?isFullScreen=true
Given the CITY and COUNTRY tables, query the names of all cities where the CONTINENT is 'Africa'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.  

## Difficulty: Easy

## Knowledge Review: 
- INNER JOIN: Returns records that have matching values in both tables
- LEFT JOIN: Returns all records from the left table, and the matched records from the right table
- RIGHT JOIN: Returns all records from the right table, and the matched records from the left table
- CROSS JOIN: Returns all records from both tables 

## Solution: 

### Solution 1: Inner Join
- City's names are from city table
- Join two tables with matching country code
- Filter the results of Africa


SELECT ci.name
FROM city AS ci
INNER JOIN country AS co ON ci.CountryCode = co.Code
WHERE co.continent ="Africa"

### Solution 2: Cross Join
- Cross join two tables then filer the city name where the country code matches and continent is Africa


SELECT city.name
FROM city, country -- equal to cross join
WHERE   city.countrycode = country.code AND
        country.continent ="Africa"


### Solution 3: Subquery/nested query
- Filter the country code where continent is Africa --> List 1
- Filter the city name where where country code in List 1

SELECT city.name
FROM city
WHERE city.countrycode IN (
    SELECT code 
    FROM country
    WHERE continent = 'Africa')

### Solution 4: CTE and INNER JOIN
- CTE to get the country code where continent is Africa, namely 'africa_country'
- Inner join city table and africa_country to get the city name.

WITH africa_country AS (
    SELECT code 
    FROM country
    WHERE continent = 'Africa') 
SELECT city.name
FROM city
INNER JOIN africa_country ON city.countrycode = africa_country.code

### Solution 5: CTE and CROSS JOIN
- CTE to get the country code where continent is Africa, namely 'africa_country'
- Cross join city table and africa_country where the country code matches to get the city name.

WITH africa_country AS (
    SELECT code 
    FROM country
    WHERE continent = 'Africa') 
SELECT city.name
FROM city, africa_country
WHERE city.countrycode = africa_country.code