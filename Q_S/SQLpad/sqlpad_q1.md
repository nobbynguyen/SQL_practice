# SQLpad SQL Questions and Solutions

## Question 1 
- https://sqlpad.io/questions/14/good-days-and-bad-days/

Good days and bad days
- Write a query to return the number of good days and bad days in May 2020 based on number of daily rentals.
- Return the results in one row with 2 columns from left to right: good_days, bad_days.
- good day: > 100 rentals.
- bad day: <= 100 rentals.
- Hint (For users already know OUTER JOIN), you can use dates table
- Hint: be super careful about datetime columns.
- Hint: this problem could be tricky, feel free to explore the rental table and take a look at some data.


## Difficulty: Hard

## Knowledge Review: 


## Solution:

- Filter time: May 2020
- Extract date from rental_ts
- Count the rental by day
- Classify good and bad day

```
WITH day_count AS (
    SELECT
        EXTRACT (YEAR FROM rental_ts) AS year_rent,
        EXTRACT (MONTH FROM rental_ts) AS month_rent,
        EXTRACT (DAY FROM rental_ts) AS day_rent,
        COUNT(*) AS count_rent
    FROM rental
    WHERE rental_ts BETWEEN '2020-05-01 00:00:00' AND '2020-06-01 00:00:00'
    GROUP BY day_rent, month_rent, year_rent
    )

SELECT COUNT(*) AS good_days, (31- COUNT(*)) AS bad_days
FROM day_count
WHERE count_rent > 100
```