---
layout: post
title: Cheat Sheet - SQL
date: 2019-02-13 09:20:00
comments: true
author: Talita Shiguemoto
lang: en
ref: csSQL
---

* [SQL Basics](#basics)
	* [SELECT & FROM](#selectfrom)
	* [DISTINC](#distinc)
	* [LIMIT](#limit)
	* [ORDER BY](#orderby)
	* [WHERE](#where)
	* [Arithmetic Operators](#arithmetic)
	* [LIKE](#like)
	* [IN](#in)
	* [NOT](#not)
	* [AND & BETWEEN](#andbetween)
	* [OR](#or)
* [SQL Joins](#joins)
	* [JOIN two tables](#twotables)
	* [JOIN More than Two Table](moretwotables)
	* [Alias](#alias)
	* [INNER](#inner)
	* [LEFT](#left)
	* [RIGHT](#right)
	* [UNION](#union)
	* [Filtering](#filtering)
* [SQL Aggregations](#aggregations)
	* [NULL](#null)
	* [COUNT](#count)
	* [SUM](#sum)
	* [AVG](#avg)
	* [MIN](#min)
	* [MAX](#max)
	* [GROUP BY](#groupby)
	* [HAVING](#having)
	* [DATE_TRUNC](#datetrunc)
	* [DATE_PART](#datepart)
	* [CASE](#case)
* [SQL Subqueries](#subqueries)
	* [Basic](#basic)
	* [Subqueries with JOIN](#subjoin)
	* [WITH](#with)
* [SQL Data Cleaning](#dataclean)
	* [LOWER](#lower)
	* [UPPER](#upper)
	* [LEFT](#left)
	* [RIGHT](#right)
	* [LENGTH](#length)
	* [POSITION & STRPOS](#positionstrpos)
	* [CONCAT](#concat)
* [SQL Window Function](#windowfunction)
	* [OVER](#over)
	* [PARTITION BY](#partitionby)
	* [ROW_NUMBER() & RANK()](#rowandrank)
	* [WINDOWN](#window)
	* [LAG & LEAD](#lagandlead)
	* [NTILE](#ntile)

## **SQL Basics** {#basics}
<br/>
Here are some codes to interact with tables, the basic syntax of SQL.

### **SELECT & FROM** {#selectfrom}
<br/>

**SELECT** indicates which column(s) you want to get the data for.


**FROM** specifies from which table(s) you want to select the columns. 

```SQL
SELECT column1, column2, column3
FROM table_example;
```

If you want to select all columns in the table, you use \*

```SQL
SELECT * FROM table_example;
```

### **DISTINC** {#distinc}
<br/>

DISTINCT provides the unique rows for all columns written in the SELECT statement.

DISTINCT comes after SELECT statement.

Example: To return the unique rows across all three columns.

```SQL
SELECT DISTINCT column1, column2, column3
FROM table_example;
```

### **LIMIT** {#limit}
<br/>

LIMIT statement is useful when you want to see just the first few rows of a table. 

LIMIT command is always the very last part of a query.

Example: Show the first 15 rows of *table_example* from the columns: *column1, column2, column3*

```SQL
SELECT column1, column2, column3
FROM table_example
LIMIT 15;
```

### **ORDER BY** {#orderby}
<br/>

ORDER BY statement sorts our **output** using the data in any column. 

ORDER BY statement comes after the SELECT and FROM statements, but before the LIMIT statement.

```SQL
SELECT column1, column2, column3
FROM table_example
ORDER BY column1  
LIMIT 15;
```

By default ORDER BY sort in ascending order, but you can use DESC after the column in your ORDER BY statement to sort in descending order

Example: First in ASC and second in DESC

```SQL
SELECT column1, column2, column3
FROM table_example
ORDER BY column1, column2 DESC;
```

Example: First in DESC and second in ASC

```SQL
SELECT column1, column2, column3
FROM table_example
ORDER BY column2 DESC, column3;
```

### **WHERE** {#where}
<br/>

The WHERE statement display subsets of tables based on conditions,  it is like filtering the data.

Common symbols used in WHERE statements:
* \> (greater than)
* < (less than)
* \>= (greater than or equal to)
* <= (less than or equal to)
* = (equal to)
* != (not equal to)

```SQL
SELECT column1, column2, column3
FROM table_example
WHERE column1 >= 1000;
```

The WHERE BY statement comes after the SELECT and FROM statements, but before the ORDER BY statement.

```SQL
SELECT column1, column2, column3
FROM table_example
WHERE column1 <= 1000
ORDER BY column2 DESC
LIMIT 10;`
```

The WHERE statement can also be used with categorical data, using = or != operators. It is necessary to use single quotes with the text data.

```SQL
SELECT column1, name,
FROM table_example
WHERE name = 'Darth Bane';
```

### **Arithmetic Operators** {#arithmetic}
<br/>

It is possible to create a new temporary column resulting from an arithmetic operation, like adding two columns. To give a name to this column it is necessary to use `AS` keyword.

Arithmetic Operators:
* \* (Multiplication)
* + (Addition)
* - (Subtraction)
* / (Division)

Example: The query below shows *column1* and a new column *sum_columns*, this second is a result from `column2 + column3`

```SQL
SELECT column1, column2 + column3 AS sum_columns
FROM table_example;
```

Order of Operations

The order of operations is the same from math, sometimes you have to use parentheses () to choose the order

### **LIKE** {#like}
<br/>

When you are using WHERE and = and you don't have a exactly word or sentence to looking for, you can use `LIKE` to help you.

The LIKE operator is case sensitivy and you have to use some operators in the search.

Example: Finds all rows that contain values that start with `x` into the *name*

```SQL
SELECT name
FROM table_example
WHERE name LIKE 'x%';
```

LIKE Operators:	
* 'x%' : Finds any values that start with "x"
* '%x' : Finds any values that end with "x"
* '%xyz%' : Finds any values that have "xyz" in any position
* '\_x%' : Finds any values that have "x" in the second position
* 'x\_%\_%' : Finds any values that start with "x" and are at least 3 characters in length
* 'x%z' : Finds any values that start with "x" and ends with "z"
* '\_2%4' : Finds any values that have a 2 in the second position and end with a 4
* '2\_\_\_4' : Finds any values in a five-digit number that start with 2 and end with 4

### **IN** {#in}
<br/>

It is similar to WHERE and =, but for more than one condition.

Example: Finds all rows with *Darth Bane* or *Darth Maul* into the *name* 

```SQL
SELECT name
FROM table_example
WHERE name IN ('Darth Bane', 'Darth Maul');
```

### **NOT** {#not}
<br/>

It is used before `IN` or `LIKE` to select all inverse rows.

Example: Finds all rows without *Darth Bane* or *Darth Maul* into the *name* 

```SQL
SELECT name
FROM table_example
WHERE name NOT IN ('Darth Bane', 'Darth Maul');
```

Example: Finds all rows that don't contain values that start with `x` into the *name*

```SQL
SELECT name
FROM table_example
WHERE name NOT LIKE 'x%';
```

### **AND & BETWEEN** {#andbetween}
<br/>

Both operators are used within a WHERE statement to combine operations where all combined conditions must be true.

The AND operator to consider two or more logical clause at a time. You may link as many statements as you would like to consider at the same time.

```SQL
SELECT column1, column2, column3
FROM table_example
WHERE column1 > 200 AND column2 = 20 AND column3 = 10;
```

This operator works with all of the operations we have seen so far including arithmetic operators (`+`, `*`, `-`, `/`). LIKE, IN, and NOT logic can also be linked together using the AND operator.

```SQL
SELECT name
FROM table_example
WHERE name NOT LIKE 'X%' AND column4 LIKE '%z';
```

The BETWEEN operator works with AND operator, using the same column for different parts of our AND statement.

Instead of writing :
`WHERE column1 >= 6 AND column1 <= 10`

We can instead write:
`WHERE column1 BETWEEN 6 AND 10`

Examples:

```SQL
SELECT column1, column2, column3
FROM table_example
WHERE column1 BETWEEN 50 AND 90;
```

```SQL
SELECT name, date_time
FROM table_example
WHERE name NOT IN ('Darth Bane', 'Darth Maul') AND date_time BETWEEN '2018-01-01' AND '2019-01-01'
ORDER BY date_time DESC;
```

### **OR** {#or}
<br/>

The OR operator can combine multiple statements within a WHERE statement, very similiar as AND operator, but the difference is at least one of the combined conditions must be true. This operator works with all of the operations we have seen so far.

Examples:

```SQL
SELECT column1, column2
FROM table_example
WHERE column1 > 100 OR column2 < 200;
```

```SQL
SELECT name, date_time, column6
FROM table_example
WHERE (name LIKE 'Darth%' OR name LIKE 'Master%') 
AND (column6 LIKE %xxx% OR column6 LIKE %Xxx%)
AND (date_time BETWEEN '2018-01-01' AND '2019-01-01');
```

## **SQL Joins** {#joins}
<br/>


### **JOIN two tables** {#twotables}
<br/>

The JOIN statement allow us to pull data from more than one table at a time.
It is used with the ON statement that holds the two columns that get linked across the two tables.

For the next examples we will use the ERD below:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_erd.png" alt="" title="ERD example"/>
</div>
<div class="col three caption">
	ERD example
</div>
<br/>

Example: To JOIN the *movie* with other table called *director*, getting all columns, you have to used the following query. In ON are the columns with unique values in both tables, the key.

```SQL
SELECT *
FROM movie
JOIN director
ON movie.director_id = director.id;
```

If you want to get just few columns, you have to include in yout SELECT statement all columns needed, writing the table name before the period and the name column (`movie.director_id`).

Example: 

```SQL
SELECT movie.title, movie.date_time,
director.id, director.age
FROM movie
JOIN director
ON movie.director_id = director.id;
```

Example: getting all the information from only the *movie* table:

```SQL
SELECT movie.*
FROM movie
JOIN director
ON movie.director_id = director.id;
```

**Primary Key (PK)**

A primary key is a unique column in a table, it is common yo be the first column in our tables.

**Foreign Key (FK)**

A foreign key is a column in one table that is a primary key in a different table. 

**Primary - Foreign Key Link**

It is a link that conect two tables. The *director_id* is the foreign key and it is linked to *id*, this is the primary-foreign key link that connects these two tables.
So, after the FROM statement is one table and after the JOIN is another table, then after the ON is always PK = FK (or FK = PK). 

### **JOIN More than Two Tables** {#moretwotables}
<br/>

This same logic can actually assist in joining more than two tables together. Look at the three tables below.

If we wanted to join all three of these tables, we could use the same logic. The code below pulls all of the data from all of the joined tables.

```SQL
SELECT movie.title, movie.date_time,
character.name, actor.name
FROM movie
JOIN character
ON movie.id = character.movie_id
JOIN actor
ON character.actor_id = actor.id;
```

Again, our JOIN holds a table, and ON is a link for our PK to equal the FK.

### **Alias** {#alias}
<br/>

It is common to give an alias for each table, normally with the first letter of the table name.

Example:

```SQL
FROM movie AS m
JOIN character AS c
```

It is possible to give the alias without using the `AS`, just putting the name after a blank space, example:

```SQL
FROM movie m
JOIN character c
```

**Aliases for Columns in Resulting Table** 

It is also possible to give alias for the columns in resulting table

Example:

```SQL
Select c.name character_name, a.name actor_name
FROM character c
JOIN actor a
```

The result is going to be like this:

| character_name | actor_name | 
|--:|--:|
| Leia Organa | Carrie Fisher | 
| Luke Skywalker | Mark Hamill | 

### **INNER Joins** {#inner}
<br/>

The INNER JOIN is the same when we are using JOIN, the result is the matching values in both tables.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_inner.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	INNER JOIN
</div>
<br/>

The syntax for INNER is:

```SQL
SELECT movie.title, director.name
FROM movie
INNER JOIN director
ON movie.director_id = director.id;
```

## **OUTER Joins**
<br/>

The OUTER joins can return the inner join plus any unmatched rows from the first table or from the second table, or both, it is depends how you syntax you will use.

### **LEFT Joins** {#left}
<br/>

LEFT JOIN gets all the data that exists in both tables, as well as all of the rows from the table in the FROM.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_left.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	LEFT JOIN
</div>
<br/>

Example:

```SQL
SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id
```

### **RIGHT Joins** {#right}
<br/>

RIGHT JOIN gets all the data that exists in both tables, as well as all of the rows from the table in the JOIN.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_right.png" alt="" title="INNER JOIN"/>
</div>
<div class="col three caption">
	RIGHT JOIN
</div>
<br/>

Example:

```SQL
SELECT movie.title, director.name
FROM movie
RIGHT JOIN director
ON movie.director_id = director.id;
```

### **UNION** {#union}
<br/>

UNION appends two tables, only appending distinct values. Both tables must have the same number of columns and have the same data types in the same order as the first table. Column names don't need to be the same.

Example:
```SQL
SELECT * 
FROM movies1

UNION

SELECT * 
FROM movies2
```

UNION ALL is similar as UNION, but include repeated values.

Example:
```SQL
SELECT * 
FROM movies1

UNION ALL

SELECT * 
FROM movies2
```

It is possible to use conditional with UNION.

Example:
```SQL
SELECT * 
FROM movies1
WHERE budget < 250000000

UNION

SELECT * 
FROM movies2
```

### **Filtering** {#filtering}
<br/>

It is possible to filter the data using conditionals as `WHERE` once the two tables have already been joined. However, it is also possible to filter one or both tables before joining them using `AND` in `ON`statement. 

Example: Create matches between the tables just for director with more than 50 years old. The filter occurs before the JOIN.

```SQL
SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id
AND director.age > 50
ORDER BY director.age;
```

Example: Create matches between the tables just for director with more than 50 years old. The filter occurs later the JOIN.

```SQL
SELECT movie.title, director.name
FROM movie
LEFT JOIN director
ON movie.director_id = director.id
WHERE director.age > 50
ORDER BY director.age;
```

## **SQL Aggregations** {#aggregations}
<br/>

### **NULL** {#null}
<br/>

NULLs are missing data, a datatype that specifies where no data exists, it is different from 0 or whitespace.

To check how many NULLs are in your column you write to use `IS NULL` with `WHERE`

Example:

```SQL
SELECT movie.title, movie.budget
FROM movie
WHERE movie.date_time IS NULL
```

To check how many non NULLs are in your column you write to use `IS NOT NULL` with `WHERE`

Example:

```SQL
SELECT movie.title, movie.budget
FROM movie
WHERE movie.date_time IS NOT NULL
```

### **COUNT()** {#count}
<br/>

The COUNT() function, as the name says, it counts all values not NULL, in others words, in a column with all rows with data, the COUNT will be the number of rows.

Example:

```SQL
SELECT COUNT(*)
FROM movie;
```

```SQL
SELECT COUNT(date_time)
FROM movie;
```

### **SUM()** {#sum}
<br/>

The SUM() function returns the total sum of a numeric column. This function only works in numerical columns.

```SQL
SELECT SUM(budget) AS total_budget
FROM movie; 
```

Example: It is possible to use an aggregate and a mathematical operators.

```SQL
SELECT SUM(column1)/SUM(column2) 
FROM table_example; 
```

### Tip

Aggregations can cause your query to slow to run, if you use LIMIT in the same query, the aggregation will run before and then it will limit the number of rows. If you want to use LIMIT before aggregation, you need to use a subquery {#subqueries} with LIMIT and then do an OUTER query with aggregation.

Example:

```SQL
SELECT title, 
	SUM(budget) AS total_budget
FROM ( SELECT * FROM movie LIMIT 100) sub
GROUP BY 1; 
```

### **AVG()** {#avg}
<br/>

The AVG() function returns the average value of a numeric column. This function only works in numerical columns.

Example:

```SQL
SELECT AVG(budget) AS average_budget
FROM movie; 
```

### **MIN()** {#min}
<br/>

The MIN() function returns the minimum of column. This function works with different types of columns, with time will return the earliest time, with numerical will return the lowest value and with non-numeric column will return the first in the alphabet order

Example:

```SQL
SELECT MIN(budget) AS min_budget
FROM movie; 
```

### **MAX()** {#max}
<br/>

The MAX() function returns the maximum of column. This function works with different types of columns as same MIN() function.

Example:

```SQL
SELECT MAX(budget) AS min_budget
FROM movie;
```

### **GROUP BY** {#groupby}
<br/>

The GROUP BY aggregates subsets of the data.  If you want to SUM the *budget* by *director_id*, you have to group them.

Example:

```SQL
SELECT SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id;
```

All columns in the SELECT statement without an aggregator (as example: `SUM`, `AVG`, `COUNT`) must be in the GROUP BY clause.

Example:

```SQL
SELECT director_id, SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id;
```

The GROUP BY always goes between WHERE and ORDER BY.

It is possible to GROUP BY multiple columns at once, the order of columns names here doesn't matter.

If you want to sort the results, it is possible to use ORDER BY and the orders of columns names here does make difference, the order is from left to right

Example:

```SQL
SELECT SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id, title
ORDER BY title, director_id;
```

### **HAVING** {#having}
<br/>

HAVING is used as same as WHERE, but when the query has been aggregated.Any time you want to perform a WHERE on an element of your query that was created by an aggregate, you need to use HAVING instead.

HAVING always goes between GROUP BY and ORDER BY.

Example:

```SQL
SELECT director_id, SUM(budget) AS sum_budget
FROM movie
GROUP BY director_id
HAVING budget > 3000;
ORDER BY title, director_id;
```

### **DATE_TRUNC** {#datetrunc}
<br/>

DATE_TRUNC() allows you to truncate your date to a particular part of your date-time column. It is use with two parameters: DATE\_TRUNC('*time_unit*', *column_name*)

Some units of time, see more [here](https://mode.com/blog/date-trunc-sql-timestamp-function-count-on):
* hour
* day
* week
* month
* quarter
* year

It is necessary to put the function after SELECT and after GROUP BY too.

Example:

```SQL
SELECT DATE_TRUNC('year', date_time) year,  SUM(budget) total_budget
FROM movie
GROUP BY DATE_ TRUNC ('year', date_time)
ORDER BY total_budget DESC;
```

You can reference the columns in your select statement in GROUP BY and ORDER BY clauses with numbers that follow the order they appear in the select statement. 

Example:

```SQL
SELECT DATE_TRUNC('year', date_time) year,  SUM(budget) total_budget
FROM movie
GROUP BY 1
ORDER BY 2 DESC;`
```

GROUP BY 1: this 1 refers to year from date, since it is the first of the columns included in the SELECT statement

ORDER BY 2: this 2 refers to total_budget since it is the second of the columns included in the SELECT statement

### **DATE_PART** {#datepart}
<br/>

DATE_PART is used to get a specific portion of a date, but pulling month or day of the week (dow) means that you are no longer keeping the years in order. 

Example:

```SQL
SELECT DATE_PART('year', date_time) year, SUM(budget) total_budget
FROM movie
GROUP BY 1;
```

Some units of time (all units from DATE_TRUNC can be used too):
* dow (Day of the week as Sunday (0) to Saturday (6))
* isdow (Day of the week as Monday (1) to Sunday (7))
* doy (Day of Year)

### **CASE** {#case}
<br/>

The CASE statement is a list of conditionals that returns the possible results from the conditionals. 

As example, if you have the columns *house*, *apples* and *dogs* and you want to divide the number of *apple*s by *dogs* GROUP BY *house*, then you write the following query:

```SQL
SELECT house, SUM(apples)/SUM(dogs) AS apples_per_dogs
FROM table_dogs
GROUP BY house;
```

If any *house* doesn't have a dog (dog = 0), you won’t be able to divide and it will appear an error.

Instead, you can use CASE statement to resolve this problem.

```SQL
SELECT house, CASE WHEN SUM(dogs) = 0 OR SUM(dogs) IS NULL THEN 0
ELSE SUM(apples)/SUM(dogs) END AS apples_per_dogs
FROM table_dogs
GROUP BY house;
```

Now the first part of the statement (`CASE WHEN SUM(dogs) = 0 OR SUM(dogs) IS NULL THEN 0`) will write 0 for any of those division by zero values, that were causing the error, and the other part (`ELSE SUM(apples)/SUM(dogs) END AS apples_per_dogs`) will compute the division normally. 

The CASE statement always goes in the SELECT clause.

THE CASE must include: WHEN, THEN, and END. ELSE is an optional component to catch cases that didn’t meet any of the other previous CASE conditions.

You can make any conditional statement using any conditional operator (`WHERE`, `LIKE`, `AND`, `OR`) between WHEN and THEN. 

You can include multiple WHEN statements, as well as an ELSE statement again, to deal with any unaddressed conditions.

## **SQL Subqueries** {#subqueries}
<br/>

### **Basic** {#basic}
<br/>

**Subquery** is query inside another query, it means, when you need to use resulting tables to query again.

Example: To get all movies if budget below the average budget of all movies, you have to create a query to calculate the average, then create another query to get all results below this average:

```SQL
SELECT * 
FROM movie
WHERE  budget <
(SELECT AVG(budget)
FROM movie);
```

It is possible to do more than one subquery together.

Example: To calculate a median of a table with even number of rows, you have to get the 2 rows in the middle, sum and divide them by two. 

Let’s imagine that table *table_dogs* have 10 rows and you want to know what is the median for apples, you need to sort the column *apples* and get the first 6 rows

```SQL
FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) 
```

Then you have to get the last two lines, sorting again, but in DESC and LIMIT and 2. Notice you already have a subquery.

```SQL
(SELECT *
FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) AS sub
ORDER BY apples DESC
LIMIT 2)
```
Finally, you have to calculate the average of these two lines, resulting in this query:

```SQL
SELECT AVG(apples) FROM 
(SELECT *
FROM (SELECT apples
      FROM table_dogs
      ORDER BY apples
      LIMIT 6) AS sub
ORDER BY apples DESC
LIMIT 2) AS Table2;
```

### **Subqueries with JOIN** {#subjoin}
<br/>

It is possible to use JOIN with different subqueries, for example

Let’s imagine database about sales with 5 tables, this example is from Udacity, and you can see the ERD below:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post005_udacity.png" alt="" title="Udacity - SQL for Data Analysis"/>
</div>
<div class="col three caption">
	Font: Udacity - SQL for Data Analysis
</div>
<br/>

The following query provides the name of the sales_rep in each region with the largest amount of total_amt_usd sales.

```SQL
SELECT  t3.region_name, t3.rep_name, t3.total_amt
FROM(SELECT t1.region_name, MAX(total_amt) total_amt
	FROM (SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
		FROM sales_reps s
		JOIN accounts a
		ON a.sales_rep_id = s.id
		JOIN orders o
		ON o.account_id = a.id
		JOIN region r
		ON r.id = s.region_id
		GROUP BY 1,2) t1
	GROUP BY 1) t2
JOIN
	(SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2
	ORDER BY 3 DESC) t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt;
```

### **WITH** {#with}
<br/>

The WITH statement is also called Common Table Expression (CTE).  It is a good practice using WITH to the same purpose as subqueries, making cleaner for future readers.

The WITH statement comes before the query, SELECT

Example: Look the query below

```SQL
SELECT * 
FROM movie
WHERE  budget <
(SELECT AVG(budget)
FROM movie);
```

To make easier to read, let’s use WITH, putting the subquery as:

```SQL
WITH avg_budget AS (
SELECT AVG(budget)
FROM movie) 
```

Then you can write the final query:

```SQL
WITH avg_budget AS (
SELECT AVG(budget)
FROM movies

SELECT * 
FROM movie
WHERE budget < avg_budget
```

The query from earliest example can be written like this:

```SQL
WITH t1 AS (SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2),

    t3 AS(SELECT s.name rep_name, r.name region_name, SUM(o.total_amt_usd) total_amt
	FROM sales_reps s
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON o.account_id = a.id
	JOIN region r
	ON r.id = s.region_id
	GROUP BY 1,2
	ORDER BY 3 DESC),

	t2 AS (SELECT t1.region_name, MAX(total_amt) total_amt
	FROM  t1
	GROUP BY 1)

SELECT  t3.region_name, t3.rep_name, t3.total_amt
FROM t2
JOIN t3
ON t3.region_name = t2.region_name AND t3.total_amt = t2.total_amt;
```

## **SQL Data Cleaning** {#dataclean}
<br/>


### **LOWER** {#lower}
<br/>

LOWER turns all the letters in lowercase

```SQL
SELECT LOWER(title)
FROM movie;
```

### **UPPER** {#upper}
<br/>

UPPER turns all the letters in uppercase

```SQL
SELECT UPPER(title)
FROM movie;
```

### **LEFT** {#left}
<br/>

LEFT gets a specified number of characters for each row in a column starting from left

Example: Getting the first letter for all titles
```SQL
SELECT LEFT(title, 1) as first_letter
FROM movie;
```

To COUNT how many movies have the same first letter:
```SQL
SELECT LEFT(UPPER(title), 1) as first_letter, COUNT(*) count_movies
FROM movie
GROUP BY 1;
```

### **RIGHT** {#right}
<br/>

RIGHT gets a specified number of characters for each row in a column starting from right

Example: Getting the last letter for all ids
```SQL
SELECT RIGHT(id, 1) as last_letter
FROM movie;
```

### **LENGTH** {#lenght}
<br/>

LENGTH gets the number of characters for each row of a column. Remember: white space is also a character

Example: Counting the number of characters for all titles. 
```SQL
SELECT title, LENGTH(title) as num_characters_title
FROM movie;
```

### **POSITION & STRPOS** {#positionstrpos}
<br/>

POSITION provides the index where that character is for each row, being 1 for the index of the first position. The synstax is POSITION('*character*' IN *table_name*).

Example: Looking the index for the first white space
```SQL
SELECT POSITION(' ' IN title)
FROM movie;
```

STRPOS provides the same result as POSITION, but the syntax is different: STRPOS(*table_name*, '*character*').
```SQL
SELECT STRPOS(title, ' ')
FROM movie;
```

POSITION and STRPOS are case sensitive.

```SQL
SELECT LEFT(name, STRPOS(name, ' ') -1 ) first_name, 
RIGHT(name, LENGTH(name) - STRPOS(name, ' ')) last_name
FROM director;
```

### **CONCAT** {#concat}
<br/>

CONCAT combines columns together. The syntax is CONCAT(*first_column*, *second_column*) or you can use piping `||`: *first_column* \|\| *second_column*.

It is possible concatenate character with the columns, as example: CONCAT(*first_column*, ' ', *second_column*)

Example: Concatenate the title with date_time
```SQL
SELECT CONCAT(title, '---', date_time)
FROM movie;
```
## **Window Function** {#windowfunction}
<br/>

WINDOW function performs a cumulative calculation across the rows.

### **OVER** {#over}
<br/>

OVER creates a window function.  It is necessary put ORDER BY to get the cumulative calculation. 

Example: Add up all *budget* accumulating them by *date_time*
```SQL
SELECT budget, 
	SUM(budget) OVER(ORDER BY date_time) AS cumulative_sum
FROM movie;
```

### **PARTITION BY** {#partitionby}
<br/>

PARTITION BY breaks the cumulative calculation by the column indicated.

Example: Add up all *budget* accumulating by *date_time* for each *director_id*
```SQL
SELECT budget, director_id,
	SUM(budget) OVER(PARTITION BY director_id ORDER BY date_time) AS cumulative_sum
FROM movie;
```

It is possible to write several window functions in the same query.
```SQL
SELECT budget, director_id,
	DATE_TRUNC('month', date_time) AS month,
	SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget,
	AVG(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS avg_budget,
	MIN(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS min_budget,
	MAX(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS max_budget,
FROM movie;
```

### **ROW_NUMBER() & RANK()** {#rowandrank}
<br/>

ROW_NUMBER() enumerates rows in sequence. You can use to count the number of rows and using PARTITION BY the query will return a new sequence for each variable in PARTITION BY.

Example: Enumerate the *budget* from highest to lowest for each *director id*
```SQL
SELECT director_id, title, budget, 
	ROW_NUMBER() OVER(PARTITION BY director_id ORDER BY budget DESC) AS sequence_budget
FROM movie;
```

RANK() enumerates rows by sequence too, but if the table has same values in the column in ORDER BY, the query repeats the number in the sequence, jumping values.

Example: 
```SQL
SELECT director_id, budget, 
	RANK() OVER(PARTITION BY director_id ORDER BY budget DESC) AS rank_budget
FROM movie;
```
Output:

| director_id | budget | rank_budget |
|----:|----:|----:|
| 1208 | 275,000,000 | 1 |
| 1208 | 260,000,000 | 2 |
| 1208 | 260,000,000 | 2 |
| 1208 | 190,000,000 | 4 |
| 1208 | 180,000,000 | 5 |
| 1208 | 180,000,000 | 5 |
| 1208 | 175,000,000 | 7 |

DENSE_RANK() similar as RANK(), but without jumping values.

Example: 
```SQL
SELECT director_id, budget, 
	DENSE_RANK() OVER(PARTITION BY director_id ORDER BY budget DESC) AS rank_budget
FROM movie;
```
Output:

| director_id | budget | rank_budget |
|----:|----:|----:|
| 1208 | 275,000,000 | 1 |
| 1208 | 260,000,000 | 2 |
| 1208 | 260,000,000 | 2 |
| 1208 | 190,000,000 | 3 |
| 1208 | 180,000,000 | 4 |
| 1208 | 180,000,000 | 4 |
| 1208 | 175,000,000 | 5 |

### **WINDOW** {#window}
<br/>

WINDOW allows us to group all window functions with same query, using the same alias. WINDOW comes after the FROM statement.

```SQL
SELECT budget, director_id,
	DATE_TRUNC('month', date_time) AS month,
	SUM(budget) OVER main_window AS sum_budget,
	AVG(budget) OVER main_window AS avg_budget,
	MIN(budget) OVER main_window AS min_budget,
	MAX(budget) OVER main_window AS max_budget,
FROM movie;

WINDOW main_window AS (PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time))
```

### **LAG & LEAD** {#lagandlead}
<br/>

LAG function returns the value from a previous row to the current row in the table.

Example: To compare the total budget from one month to the other, first create an inner query getting the SUM for the *budget* by each month
```SQL
SELECT DATE_TRUNC('month', date_time) AS month,	
	SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
FROM movie
GROUP BY   1
```
To see the *sum_budget* from the previous month just use the LAG function as:

`LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag`

Then to see the difference from the current budget to the previous, write a line with  *sum_budge* - LAG function, as: 

`sum_budget - LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag_difference`

The entire query:
```SQL
SELECT month, sum_budget,
	LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag,
	sum_budget - LAG(sum_budget) OVER (ORDER BY sum_budget) AS lag_difference
FROM(
		SELECT DATE_TRUNC('month', date_time) AS month,	
			SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
		FROM movie
		GROUP BY   1
    ) sub
ORDER BY 1;
```
The ORDER BY comes in the end because the difference has to be by month.

LEAD returns the value from the row following the current row in the table.

Example: similar to previous example, to calculate the difference between the following budget and the current one, just use LEAD function - *sum_budget* , as:

`LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference - sum_budget`

Being LEAD function just:  

`LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference`

```SQL
SELECT month, sum_budget,
	LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference,
	LEAD(sum_budget) OVER (ORDER BY sum_budget) AS lead_difference - sum_budget
FROM(
		SELECT DATE_TRUNC('month', date_time) AS month,	
			SUM(budget) OVER(PARTITION BY director_id ORDER BY DATE_TRUNC('month', date_time)) AS sum_budget
		FROM movie
		GROUP BY   1
    ) sub
ORDER BY 1;
```

### **NTILE** {#ntile}
<br/>

NTILE function identifies what quartile (or percentile or any other subdivision) a given row falls into. The subdivision comes inside the parentheses: NTILE(*number*), while ORDER BY determines which column to use to calculate the quartiles

```SQL
SELECT director_id, title, budget, 
	NTILE(100) OVER(PARTITION BY director_id ORDER BY budget) AS budget_percentile
FROM movie;
```