---
title: SQL入门学习
cover: 'http://img.up-4ever.site/infinity-9198993.jpg'
top_img: 'http://img.up-4ever.site/infinity-9198993.jpg'
tags:
  - SQL
categories:
  - SQL
abbrlink: 6065ed5c
---
# SQL入门学习

## 前言

国庆节在家学习了一下SQL语言，主要是通过阮一峰老师在周刊推荐的网站[SQLBolt](https://sqlbolt.com/)。一共18节小课，每一节都有对应的练习题，相对来说比较简单，适合入门。

## SQL Lesson 1: SELECT queries

```sql
# Select query for a specific columns
SELECT <column>, <another_column>, …
FROM <mytable>;

# Select query for all columns
SELECT * 
FROM <mytable>;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

1. Find the `title` of each film

```sql
SELECT title FROM movies;
```

2. Find the `director` of each film

```sql
SELECT director FROM movies;
```

3. Find the `title` and `director` of each film

```sql
SELECT title, director FROM movies;
```

4. Find the `title` and `year` of each film

```sql
SELECT title, year FROM movies;
```

5. Find `all` the information about each film

```sql
SELECT * FROM movies;
```

## SQL Lesson 2: Queries with constraints (Pt. 1)

```sql
# Select query with constraints
SELECT <column>, <another_column>, …
FROM <mytable>
WHERE <condition>
    AND/OR <another_condition>
    AND/OR …;
```

| Operator            | Condition                                            | SQL Example                           |
| ------------------- | ---------------------------------------------------- | ------------------------------------- |
| =, !=, < <=, >, >=  | Standard numerical operators                         | col_name **!=** 4                     |
| BETWEEN … AND …     | Number is within range of two values (inclusive)     | col_name **BETWEEN** 1.5 **AND** 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) | col_name **NOT BETWEEN** 1 **AND** 10 |
| IN (…)              | Number exists in a list                              | col_name **IN** (2, 4, 6)             |
| NOT IN (…)          | Number does not exist in a list                      | col_name **NOT IN** (1, 3, 5)         |

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

1. Find the movie with a row `id` of 6

```sql
SELECT * FROM movies WHERE id=6;
```

2. Find the movies released in the `year`s between 2000 and 2010

```sql
SELECT * FROM movies WHERE year BETWEEN 2000 AND 2010;
```

3. Find the movies **not** released in the `year`s between 2000 and 2010

```sql
SELECT * FROM movies WHERE year NOT BETWEEN 2000 AND 2010;
```

4. Find the first 5 Pixar movies and their release `year`

```sql
SELECT title, year FROM movies WHERE year <= 2003;
```

## SQL Lesson 3: Queries with constraints (Pt. 2)

| Operator   | Condition                                                    | Example                                                      |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| =          | Case sensitive exact string comparison (*notice the single equals*) | col_name **=** "abc"                                         |
| != or <>   | Case sensitive exact string inequality comparison            | col_name **!=** "abcd"                                       |
| LIKE       | Case insensitive exact string comparison                     | col_name **LIKE** "ABC"                                      |
| NOT LIKE   | Case insensitive exact string inequality comparison          | col_name **NOT LIKE** "ABCD"                                 |
| %          | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name **LIKE** "%AT%" (matches "AT", "ATTIC", "CAT" or even "BATS") |
| _          | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | col_name **LIKE** "AN_" (matches "AND", but not "AN")        |
| IN (…)     | String exists in a list                                      | col_name **IN** ("A", "B", "C")                              |
| NOT IN (…) | String does not exist in a list                              | col_name **NOT IN** ("D", "E", "F")                          |

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |
| 87   | WALL-G              | Brenda Chapman | 2042 | 97             |

1. Find all the Toy Story movies

```sql
SELECT * FROM movies WHERE title LIKE 'Toy Story%';
```

2. Find all the movies directed by John Lasseter

```sql
SELECT * FROM movies WHERE director='John Lasseter';
```

3. Find all the movies (and director) not directed by John Lasseter

```sql
SELECT * FROM movies WHERE director!='John Lasseter';
```

4. Find all the WALL-* movies

```sql
SELECT * FROM movies WHERE title LIKE 'WALL-_';
```

## SQL Lesson 4: Filtering and sorting Query results

```sql
# Select query with unique results
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);

SELECT <column>, <another_column>, …
FROM <mytable>
WHERE condition(s)
ORDER BY <column> ASC/DESC
LIMIT <num_limit> OFFSET <num_offset>;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 2    | Up                  | Pete Docter    | 2009 | 101            |
| 3    | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 4    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 5    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 6    | Toy Story           | John Lasseter  | 1995 | 81             |
| 7    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 8    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 9    | Brave               | Brenda Chapman | 2012 | 102            |
| 10   | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 11   | Cars                | John Lasseter  | 2006 | 117            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Monsters University | Dan Scanlon    | 2013 | 110            |
| 14   | The Incredibles     | Brad Bird      | 2004 | 116            |

1. List all directors of Pixar movies (alphabetically), without duplicates

```sql
SELECT DISTINCT director FROM movies ORDER BY director ASC;
```

2. List the last four Pixar movies released (ordered from most recent to least)

```sql
SELECT * FROM movies ORDER BY year DESC LIMIT 4;
```

3. List the **first** five Pixar movies sorted alphabetically

```sql
SELECT * FROM movies ORDER BY title LIMIT 5;
```

4. List the **next** five Pixar movies sorted alphabetically

```sql
SELECT * FROM movies ORDER BY title LIMIT 5 OFFSET 5;
```

## SQL lesson 5:SQL Review: Simple SELECT Queries

```sql
SELECT <column>, <another_column>, …
FROM <mytable>
WHERE condition(s)
ORDER BY <column> ASC/DESC
LIMIT <num_limit> OFFSET <num_offset>;
```

### Exercise

| City                | Country       | Population | Latitude  | Longitude   |
| ------------------- | ------------- | ---------- | --------- | ----------- |
| Guadalajara         | Mexico        | 1500800    | 20.659699 | -103.349609 |
| Toronto             | Canada        | 2795060    | 43.653226 | -79.383184  |
| Houston             | United States | 2195914    | 29.760427 | -95.369803  |
| New York            | United States | 8405837    | 40.712784 | -74.005941  |
| Philadelphia        | United States | 1553165    | 39.952584 | -75.165222  |
| Havana              | Cuba          | 2106146    | 23.05407  | -82.345189  |
| Mexico City         | Mexico        | 8555500    | 19.432608 | -99.133208  |
| Phoenix             | United States | 1513367    | 33.448377 | -112.074037 |
| Los Angeles         | United States | 3884307    | 34.052234 | -118.243685 |
| Ecatepec de Morelos | Mexico        | 1742000    | 19.601841 | -99.050674  |
| Montreal            | Canada        | 1717767    | 45.501689 | -73.567256  |
| Chicago             | United States | 2718782    | 41.878114 | -87.629798  |

1. List all the Canadian cities and their populations

```sql
SELECT * FROM north_american_cities WHERE Country='Canada';
```

2. Order all the cities in the United States by their latitude from north to south

```sql
SELECT * FROM north_american_cities WHERE Country='United States' ORDER BY Latitude DESC;
```

3. List all the cities west of Chicago, ordered from west to east

```sql
SELECT * FROM north_american_cities WHERE Longitude < -87.629798 ORDER BY Longitude;
```

4. List the two largest cities in Mexico (by population)

```sql
SELECT * FROM north_american_cities WHERE Country='Mexico' ORDER BY Population DESC LIMIT 2;
```

5. List the third and fourth largest cities (by population) in the United States and their population

```sql
SELECT * FROM north_american_cities WHERE Country = 'United States' ORDER BY Population DESC LIMIT 2, 2;
```

或者

```sql
SELECT * FROM north_american_cities WHERE Country = 'United States' ORDER BY Population DESC LIMIT 2 OFFSET 2;
```

## SQL lesson 6: Multi-table queries with JOINs

```sql
SELECT <column>, <another_table_column>, …
FROM <mytable>
INNER JOIN <another_table> 
    ON <mytable.id = another_table.id>
WHERE condition(s)
ORDER BY <column>, … ASC/DESC
LIMIT <num_limit> OFFSET <num_offset>;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

Table: Boxoffice

| Movie_id | Rating | Domestic_sales | International_sales |
| -------- | ------ | -------------- | ------------------- |
| 5        | 8.2    | 380843261      | 555900000           |
| 14       | 7.4    | 268492764      | 475066843           |
| 8        | 8      | 206445654      | 417277164           |
| 12       | 6.4    | 191452396      | 368400000           |
| 3        | 7.9    | 245852179      | 239163000           |
| 6        | 8      | 261441092      | 370001000           |
| 9        | 8.5    | 223808164      | 297503696           |
| 11       | 8.4    | 415004880      | 648167031           |
| 1        | 8.3    | 191796233      | 170162503           |
| 7        | 7.2    | 244082982      | 217900167           |
| 10       | 8.3    | 293004164      | 438338580           |
| 4        | 8.1    | 289916256      | 272900000           |
| 2        | 7.2    | 162798565      | 200600000           |
| 13       | 7.2    | 237283207      | 301700000           |

1. Find the domestic and international sales for each movie

```sql
SELECT title, domestic_sales, international_sales FROM movies INNER JOIN boxoffice ON id = movie_id;
```

2. Show the sales numbers for each movie that did better internationally rather than domestically

```sql
SELECT title, domestic_sales, international_sales FROM movies INNER JOIN boxoffice ON id = movie_id WHERE international_sales > domestic_sales;
```

3. List all the movies by their ratings in descending order

```sql
SELECT title, rating, domestic_sales, international_sales FROM movies INNER JOIN boxoffice ON id = movie_id ORDER BY rating DESC;
```

## SQL Lesson 7: OUTER JOINs

```sql
# Select query with LEFT/RIGHT/FULL JOINs on multiple tables
SELECT <column>, <another_column>, …
FROM <mytable>
INNER/LEFT/RIGHT/FULL JOIN <another_table> 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY <column>, … ASC/DESC
LIMIT <num_limit> OFFSET <num_offset>;
```

### Exercise

Table: Buildings

| Building_name | Capacity |
| ------------- | -------- |
| 1e            | 24       |
| 1w            | 32       |
| 2e            | 16       |
| 2w            | 20       |

Table: Employees

| Role     | Name       | Building | Years_employed |
| -------- | ---------- | -------- | -------------- |
| Engineer | Becky A.   | 1e       | 4              |
| Engineer | Dan B.     | 1e       | 2              |
| Engineer | Sharon F.  | 1e       | 6              |
| Engineer | Dan M.     | 1e       | 4              |
| Engineer | Malcom S.  | 1e       | 1              |
| Artist   | Tylar S.   | 2w       | 2              |
| Artist   | Sherman D. | 2w       | 8              |
| Artist   | Jakob J.   | 2w       | 6              |
| Artist   | Lillia A.  | 2w       | 7              |
| Artist   | Brandon J. | 2w       | 7              |
| Manager  | Scott K.   | 1e       | 9              |
| Manager  | Shirlee M. | 1e       | 3              |
| Manager  | Daria O.   | 2w       | 6              |

1. Find the list of all buildings that have employees

```sql
SELECT DISTINCT building FROM employees;
```

2. Find the list of all buildings and their capacity

```sql
SELECT * FROM buildings;
```

2. List all buildings and the distinct employee roles in each building (including empty buildings)

```sql
SELECT DISTINCT building_name, role 
FROM buildings 
  LEFT JOIN employees
    ON building_name = building;
```

## SQL Lesson 8: A short note on NULLs

```sql
# Select query with constraints on NULL values
SELECT <column>, <another_column>, …
FROM <mytable>
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

### Exrecise

Table: Buildings

| Building_name | Capacity |
| ------------- | -------- |
| 1e            | 24       |
| 1w            | 32       |
| 2e            | 16       |
| 2w            | 20       |

Table: Employees

| Role     | Name       | Building | Years_employed |
| -------- | ---------- | -------- | -------------- |
| Engineer | Becky A.   | 1e       | 4              |
| Engineer | Dan B.     | 1e       | 2              |
| Engineer | Sharon F.  | 1e       | 6              |
| Engineer | Dan M.     | 1e       | 4              |
| Engineer | Malcom S.  | 1e       | 1              |
| Artist   | Tylar S.   | 2w       | 2              |
| Artist   | Sherman D. | 2w       | 8              |
| Artist   | Jakob J.   | 2w       | 6              |
| Artist   | Lillia A.  | 2w       | 7              |
| Artist   | Brandon J. | 2w       | 7              |
| Manager  | Scott K.   | 1e       | 9              |
| Manager  | Shirlee M. | 1e       | 3              |
| Manager  | Daria O.   | 2w       | 6              |
| Engineer | Yancy I.   |          | 0              |
| Artist   | Oliver P.  |          | 0              |

1. Find the name and role of all employees who have not been assigned to a building

```sql
SELECT name, role FROM employees WHERE building is null;
```

2. Find the names of the buildings that hold no employees

```sql
SELECT DISTINCT building_name
FROM buildings
  LEFT JOIN employees
    ON building_name = building
WHERE role IS NULL;
```

## SQL Lesson 9: Queries with expressions

```sql
SELECT col_expression AS expr_description, …
FROM mytable;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

Table: Boxoffice

| Movie_id | Rating | Domestic_sales | International_sales |
| -------- | ------ | -------------- | ------------------- |
| 5        | 8.2    | 380843261      | 555900000           |
| 14       | 7.4    | 268492764      | 475066843           |
| 8        | 8      | 206445654      | 417277164           |
| 12       | 6.4    | 191452396      | 368400000           |
| 3        | 7.9    | 245852179      | 239163000           |
| 6        | 8      | 261441092      | 370001000           |
| 9        | 8.5    | 223808164      | 297503696           |
| 11       | 8.4    | 415004880      | 648167031           |
| 1        | 8.3    | 191796233      | 170162503           |
| 7        | 7.2    | 244082982      | 217900167           |
| 10       | 8.3    | 293004164      | 438338580           |
| 4        | 8.1    | 289916256      | 272900000           |
| 2        | 7.2    | 162798565      | 200600000           |
| 13       | 7.2    | 237283207      | 301700000           |

1. List all movies and their combined sales in **millions** of dollars

```sql
SELECT title, (domestic_sales + international_sales) / 1000000 AS gross_sales_millions
FROM movies
  JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```

2. List all movies and their ratings **in percent**

```sql
SELECT title, rating * 10 AS rate_percent
FROM movies
  INNER JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```

3. List all movies that were released on even number years

```sql
SELECT title, year FROM movies WHERE year % 2 = 0;
```

## SQL Lesson 10: Queries with aggregates (Pt. 1)

```sql
# Select query with aggregate functions over all rows
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM <mytable>
WHERE constraint_expression;

# Select query with aggregate functions over groups
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY <column>;
```

### Common aggregate functions

| Function                                  | Description                                                  |
| ----------------------------------------- | ------------------------------------------------------------ |
| **COUNT(\*)**, **COUNT(*column*)** | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| **MIN(*column*)**                     | Finds the smallest numerical value in the specified column for all rows in the group. |
| **MAX(*column*)**                     | Finds the largest numerical value in the specified column for all rows in the group. |
| **AVG(*column*)**                     | Finds the average numerical value in the specified column for all rows in the group. |
| **SUM(*column*)**                     | Finds the sum of all numerical values in the specified column for the rows in the group. |

### Exercise

Table: Employees

| Role     | Name       | Building | Years_employed |
| -------- | ---------- | -------- | -------------- |
| Engineer | Becky A.   | 1e       | 4              |
| Engineer | Dan B.     | 1e       | 2              |
| Engineer | Sharon F.  | 1e       | 6              |
| Engineer | Dan M.     | 1e       | 4              |
| Engineer | Malcom S.  | 1e       | 1              |
| Artist   | Tylar S.   | 2w       | 2              |
| Artist   | Sherman D. | 2w       | 8              |
| Artist   | Jakob J.   | 2w       | 6              |
| Artist   | Lillia A.  | 2w       | 7              |
| Artist   | Brandon J. | 2w       | 7              |
| Manager  | Scott K.   | 1e       | 9              |
| Manager  | Shirlee M. | 1e       | 3              |
| Manager  | Daria O.   | 2w       | 6              |

1. Find the longest time that an employee has been at the studio

```sql
SELECT role, name, building, max(Years_employed) FROM employees;
```

2. For each role, find the average number of years employed by employees in that role

```sql
SELECT role, AVG(Years_employed) as avg_emply FROM employees group by role;
```

3. Find the total number of employee years worked in each building

```sql
SELECT role, name, building, SUM(Years_employed) as sum_employ FROM employees group by building;
```

## SQL Lesson 11: Queries with aggregates (Pt. 2)

```sql
# Select query with HAVING constraint
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

### Exercise

Table: Employees

| Role     | Name       | Building | Years_employed |
| -------- | ---------- | -------- | -------------- |
| Engineer | Becky A.   | 1e       | 4              |
| Engineer | Dan B.     | 1e       | 2              |
| Engineer | Sharon F.  | 1e       | 6              |
| Engineer | Dan M.     | 1e       | 4              |
| Engineer | Malcom S.  | 1e       | 1              |
| Artist   | Tylar S.   | 2w       | 2              |
| Artist   | Sherman D. | 2w       | 8              |
| Artist   | Jakob J.   | 2w       | 6              |
| Artist   | Lillia A.  | 2w       | 7              |
| Artist   | Brandon J. | 2w       | 7              |
| Manager  | Scott K.   | 1e       | 9              |
| Manager  | Shirlee M. | 1e       | 3              |
| Manager  | Daria O.   | 2w       | 6              |

1. Find the number of Artists in the studio (without a **HAVING** clause) 

```sql
SELECT role, COUNT(*) as Number_of_artists
FROM employees
WHERE role = "Artist";
```

或

```sql
SELECT role, COUNT(*) as total FROM employees group by role HAVING role = 'Artist';
```

2. Find the number of Employees of each role in the studio

```sql
SELECT role, COUNT(*) as Number_of_artists
FROM employees group by role;
```

3. Find the total number of years employed by all Engineers

```sql
SELECT Role, SUM(Years_employed) as sum_year_employed FROM employees where role='Engineer';
```

## SQL Lesson 12: Order of execution of a Query

完整SQL语句：

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

### Query order of execution(查询执行顺序)

1. `FROM` and `JOIN`s
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `DISTINCT`
7. `ORDER BY`
8. `LIMIT / OFFSET`

### Exercise

Table: Movies 

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

Table: Boxoffice

| Movie_id | Rating | Domestic_sales | International_sales |
| -------- | ------ | -------------- | ------------------- |
| 5        | 8.2    | 380843261      | 555900000           |
| 14       | 7.4    | 268492764      | 475066843           |
| 8        | 8      | 206445654      | 417277164           |
| 12       | 6.4    | 191452396      | 368400000           |
| 3        | 7.9    | 245852179      | 239163000           |
| 6        | 8      | 261441092      | 370001000           |
| 9        | 8.5    | 223808164      | 297503696           |
| 11       | 8.4    | 415004880      | 648167031           |
| 1        | 8.3    | 191796233      | 170162503           |
| 7        | 7.2    | 244082982      | 217900167           |
| 10       | 8.3    | 293004164      | 438338580           |
| 4        | 8.1    | 289916256      | 272900000           |
| 2        | 7.2    | 162798565      | 200600000           |
| 13       | 7.2    | 237283207      | 301700000           |

1. Find the number of movies each director has directed

```sql
SELECT Director, count(*) as numberofmovies FROM movies group by Director;
```

2. Find the total domestic and international sales that can be attributed to each director

```sql
SELECT Director, count(*) as NumOfMovie, sum(Domestic_sales + International_sales) FROM movies inner join Boxoffice on movies.id = Boxoffice.Movie_id group by Director;
```

## SQL Lesson 13: Inserting rows

```sql
# Insert statement with values for all columns
INSERT INTO mytable
VALUES (value_or_expr, another_value_or_expr, …),
       (value_or_expr_2, another_value_or_expr_2, …),
       …;
```

```sql
# Insert statement with specific columns
INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;
```

```sql
# Example Insert statement with expressions
INSERT INTO boxoffice
(movie_id, rating, sales_in_millions)
VALUES (1, 9.9, 283742034 / 1000000);
```

### Exercise

Table: Movies

| Id   | Title        | Director      | Year | Length_minutes |
| ---- | ------------ | ------------- | ---- | -------------- |
| 1    | Toy Story    | John Lasseter | 1995 | 81             |
| 2    | A Bug's Life | John Lasseter | 1998 | 95             |
| 3    | Toy Story 2  | John Lasseter | 1999 | 93             |

Table: Boxoffice

| Movie_id | Rating | Domestic_sales | International_sales |
| -------- | ------ | -------------- | ------------------- |
| 3        | 7.9    | 245852179      | 239163000           |
| 1        | 8.3    | 191796233      | 170162503           |
| 2        | 7.2    | 162798565      | 200600000           |

1. Add the studio's new production, **Toy Story 4** to the list of movies (you can use any director)

```sql
INSERT INTO movies VALUES (4, 'Toy Story 4', 'Jhon Lasseter', 1998, 89);
```

2. Toy Story 4 has been released to critical acclaim! It had a rating of **8.7**, and made **340 million domestically** and **270 million internationally**. Add the record to the `BoxOffice` table

```sql
INSERT INTO boxoffice VALUES (4, 8.7, 34000000, 27000000);
```

## SQL Lesson 14: Updating rows

```sql
UPDATE <mytable>
SET <column> = <value_or_expr>, 
    <other_column> = <another_value_or_expr>, 
    …
WHERE condition;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | El Directore   | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1899 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 8         | El Directore   | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

1. The director for A Bug's Life is incorrect, it was actually directed by **John Lasseter**

```sql
UPDATE movies SET director = 'John Lasseter' WHERE title="A Bug's Life"
```

2. The year that Toy Story 2 was released is incorrect, it was actually released in **1999**

```sql
UPDATE movies SET year = 1999 WHERE title="Toy Story 2";
```

3. Both the title and director for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by **Lee Unkrich**

```sql
UPDATE movies SET title = "Toy Story 3", director="Lee Unkrich" WHERE title="Toy Story 8";
```

## SQL Lesson 15: Deleting rows

```sql
DELETE FROM <mytable>
WHERE condition;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

1. The database needs to be cleaned up a little bit, so try and delete a few rows in the tasks below.

```sql
DELETE FROM movies WHERE year < 2005;
```

2. Andrew Stanton has also left the studio, so please remove all movies directed by him.

```sql
DELETE FROM movies WHERE director="Andrew Stanton";
```

## SQL Lesson 16: Creating tables

```sql
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

### Table data type

| Data type                                            | Description                                                  |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| `INTEGER`, `BOOLEAN`                                 | The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1. |
| `FLOAT`, `DOUBLE`, `REAL`                            | The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value. |
| `CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT` | The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables. |
| `DATE`, `DATETIME`                                   | SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones. |
| `BLOB`                                               | Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them. |

### Exercise

1. Create a new table named `Database` with the following columns:

   -  `Name` A string (text) describing the name of the database
   -  `Version` A number (floating point) of the latest version of this database
   -  `Download_count` An integer count of the number of times this database was downloaded

   This table has no constraints.

```sql
create table if not exists database (
    name TEXT,
    version FLOAT,
    download_count, INTEGER
)
```

## SQL Lesson 17: Altering tables

As your data changes over time, SQL provides a way for you to update your corresponding tables and database schemas by using the `ALTER TABLE` statement to add, remove, or modify **columns** and **table constraints**.

```sql
# Altering table to add new column(s)
ALTER TABLE <mytable>
ADD <column> <DataType OptionalTableConstraint>
    DEFAULT <default_value>;
```

```sql
# Altering table to remove column(s)
ALTER TABLE <mytable>
DROP <column_to_be_deleted>;
```

```sql
# Altering table name
ALTER TABLE <mytable>
RENAME TO <new_table_name>;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

1. Add a column named **Aspect_ratio** with a **FLOAT** data type to store the aspect-ratio each movie was released in.

```sql
ALTER TABLE movies ADD Aspect_ratio float DEFAULT 8.5;
```

2. Add another column named **Language** with a **TEXT** data type to store the language that the movie was released in. Ensure that the default for this language is **English**.

```sql
ALTER TABLE movies ADD Language TEXT DEFAULT 'English';
```

## SQL Lesson 18: Dropping tables

```sql
# Drop table statement
DROP TABLE IF EXISTS <mytable>;
```

### Exercise

Table: Movies

| Id   | Title               | Director       | Year | Length_minutes |
| ---- | ------------------- | -------------- | ---- | -------------- |
| 1    | Toy Story           | John Lasseter  | 1995 | 81             |
| 2    | A Bug's Life        | John Lasseter  | 1998 | 95             |
| 3    | Toy Story 2         | John Lasseter  | 1999 | 93             |
| 4    | Monsters, Inc.      | Pete Docter    | 2001 | 92             |
| 5    | Finding Nemo        | Andrew Stanton | 2003 | 107            |
| 6    | The Incredibles     | Brad Bird      | 2004 | 116            |
| 7    | Cars                | John Lasseter  | 2006 | 117            |
| 8    | Ratatouille         | Brad Bird      | 2007 | 115            |
| 9    | WALL-E              | Andrew Stanton | 2008 | 104            |
| 10   | Up                  | Pete Docter    | 2009 | 101            |
| 11   | Toy Story 3         | Lee Unkrich    | 2010 | 103            |
| 12   | Cars 2              | John Lasseter  | 2011 | 120            |
| 13   | Brave               | Brenda Chapman | 2012 | 102            |
| 14   | Monsters University | Dan Scanlon    | 2013 | 110            |

Table: Boxoffice (Read-Only)

| Movie_id | Rating | Domestic_sales | International_sales |
| -------- | ------ | -------------- | ------------------- |
| 5        | 8.2    | 380843261      | 555900000           |
| 14       | 7.4    | 268492764      | 475066843           |
| 8        | 8      | 206445654      | 417277164           |
| 12       | 6.4    | 191452396      | 368400000           |
| 3        | 7.9    | 245852179      | 239163000           |
| 6        | 8      | 261441092      | 370001000           |
| 9        | 8.5    | 223808164      | 297503696           |
| 11       | 8.4    | 415004880      | 648167031           |
| 1        | 8.3    | 191796233      | 170162503           |
| 7        | 7.2    | 244082982      | 217900167           |
| 10       | 8.3    | 293004164      | 438338580           |
| 4        | 8.1    | 289916256      | 272900000           |
| 2        | 7.2    | 162798565      | 200600000           |
| 13       | 7.2    | 237283207      | 301700000           |

1. We've sadly reached the end of our lessons, lets clean up by removing the **Movies** table

```sql
DROP TABLE IF EXISTS movies;
```

2. And drop the **BoxOffice** table as well

```sql
DROP TABLE IF EXISTS BoxOffice;
```

## 最后

到这SQL语言算简单入门，还需多加练习。

另外SQLBolt还有其他两篇文章：

- [**SQL Topic: Subqueries**](https://sqlbolt.com/topic/subqueries)
- [**SQL Topic: Unions, Intersections & Exceptions**](https://sqlbolt.com/topic/set_operations)