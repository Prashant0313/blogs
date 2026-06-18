+++
date = '2026-06-18T22:33:03+05:30'
title = 'SQL'
+++
# What is SQL?

SQL, or Structured Query Language, is a language designed to allow both technical and non-technical users query, manipulate, and transform data from a relational database. And due to its simplicity, SQL databases provide safe and scalable storage for millions of websites and mobile applications.

>**Did you know?**
	There are many popular SQL databases including SQLite, MySQL, Postgres, Oracle and Microsoft SQL Server. All of them support the common SQL language standard, which is what this site will be teaching, but each implementation can differ in the additional features and storage types it supports.

# Relational databases

Before learning the SQL syntax, it's important to have a model for what a relational database actually is. A relational database represents a collection of related (two-dimensional) tables. Each of the tables are similar to an Excel spreadsheet, with a fixed number of named columns (the attributes or properties of the table) and any number of rows of data.

For example, if the Department of Motor Vehicles had a database, you might find a table containing all the known vehicles that people in the state are driving. This table might need to store the model name, type, number of wheels, and number of doors of each vehicle for example.


### Table: Vehicles

| Id  | Make/Model        | # Wheels | # Doors | Type       |
| --- | ----------------- | -------- | ------- | ---------- |
| 1   | Ford Focus        | 4        | 4       | Sedan      |
| 2   | Tesla Roadster    | 4        | 2       | Sports     |
| 3   | Kawakasi Ninja    | 2        | 0       | Motorcycle |
| 4   | McLaren Formula 1 | 4        | 0       | Race       |
| 5   | Tesla S           | 4        | 4       | Sedan      |

In such a database, you might find additional related tables containing information such as a list of all registered drivers in the state, the types of driving licenses that can be granted, or even driving violations for each driver.

By learning SQL, the goal is to learn how to answer specific questions about this data, like _"What types of vehicles are on the road have less than four wheels?"_, or _"How many models of cars does Tesla produce?"_, to help us make better decisions down the road.

### SELECT
To retrieve data from a SQL database, we need to write `SELECT` statements, which are often colloquially referred to as _queries_. A query in itself is just a statement which declares what data we are looking for, where to find it in the database, and optionally, how to transform it before it is returned.
```sql
SELECT column, another_column, … FROM mytable;
```
The result of this query will be a two-dimensional set of rows and columns, effectively a copy of the table, but only with the columns that we requested.

If we want to retrieve absolutely all the columns of data from a table, we can then use the asterisk (`*`) shorthand in place of listing all the column names individually.

Select query for all columns

```sql
SELECT * FROM mytable;
```

This query, in particular, is really useful because it's a simple way to inspect a table by dumping all the data at once.

### WHERE 
In order to filter certain results from being returned, we need to use a `WHERE` clause in the query. The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.

Select query with constraints

```sql
SELECT column, another_column, … 
FROM mytable 
WHERE condition 
	AND/OR another_condition 
	AND/OR …;
```

More complex clauses can be constructed by joining numerous `AND` or `OR` logical keywords (ie. num_wheels >= 4 AND doors <= 2). And below are some useful operators that you can use for numerical data (ie. integer or floating point):

| Operator            | Condition                                            | SQL Example                   |
| ------------------- | ---------------------------------------------------- | ----------------------------- |
| =, !=, < <=, >, >=  | Standard numerical operators                         | col_name != 4                 |
| BETWEEN … AND …     | Number is within range of two values (inclusive)     | col_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN … AND … | Number is not within range of two values (inclusive) | col_name NOT BETWEEN 1 AND 10 |
| IN (…)              | Number exists in a list                              | col_name IN (2, 4, 6)         |
| NOT IN (…)          | Number does not exist in a list                      | col_name NOT IN (1, 3, 5)     |

In addition to making the results more manageable to understand, writing clauses to constrain the set of rows returned also allows the query to run faster due to the reduction in unnecessary data being returned.

When writing `WHERE` clauses with columns containing text data, SQL supports a number of useful operators to do things like case-insensitive string comparison and wildcard pattern matching. We show a few common text-data specific operators below:

|                  |                                                                                                       |                                                                         |
| ---------------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Operator         | Condition                                                                                             | Example                                                                 |
| =                | Case sensitive exact string comparison (_notice the single equals_)                                   | col_name = "abc"                                                        |
| != or <>         | Case sensitive exact string inequality comparison                                                     | col_name != "abcd"                                                      |
| LIKE             | Case insensitive exact string comparison                                                              | col_name LIKE "ABC"                                                     |
| <br>NOT LIKE<br> | Case insensitive exact string inequality comparison                                                   | col_name NOT LIKE "ABCD"                                                |
| %                | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name LIKE "%AT%"  <br>(matches "AT", "ATTIC", "CAT" or even "BATS") |
| _                | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)                    | col_name LIKE "AN_"  <br>(matches "AND", but not "AN")                  |
| IN (…)           | String exists in a list                                                                               | col_name IN ("A", "B", "C")                                             |
| NOT IN (…)       | String does not exist in a list                                                                       | col_name NOT IN ("D", "E", "F")                                         |

> ***NOTE***
>All strings must be quoted so that the query parser can distinguish words in the string from SQL keywords.

Select query with ordered results

### DISTINCT
SQL provides a convenient way to discard rows that have a duplicate column value by using the `DISTINCT` keyword.

Select query with unique results

```sql
SELECT DISTINCT column, another_column, … 
FROM mytable 
WHERE condition(s);
```


### ORDER BY
```SQL 
SELECT column, another_column, … 
FROM mytable 
WHERE condition(s) 
ORDER BY column ASC/DESC;
```

When an `ORDER BY` clause is specified, each row is sorted alpha-numerically based on the specified column's value. In some databases, you can also specify a collation to better sort data containing international text.

# Limiting results to a subset

Another clause which is commonly used with the `ORDER BY` clause are the `LIMIT` and `OFFSET` clauses, which are a useful optimization to indicate to the database the subset of the results you care about.  
The `LIMIT` will reduce the number of rows to return, and the optional `OFFSET` will specify where to begin counting the number rows from.

Select query with limited rows

```sql
SELECT column, another_column, … 
FROM mytable 
WHERE condition(s) 
ORDER BY column ASC/DESC 
LIMIT num_limit OFFSET num_offset
```


If you think about websites like Reddit or Pinterest, the front page is a list of links sorted by popularity and time, and each subsequent page can be represented by sets of links at different offsets in the database. Using these clauses, the database can then execute queries faster and more efficiently by processing and returning only the requested content.

# Database normalization

Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car). As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables.

In order to answer questions about an entity that has data spanning multiple tables in a normalized database, we need to learn how to write a query that can combine all that data and pull out exactly the information we need.

# Multi-table queries with JOINs

Tables that share information about a single entity need to have a _primary key_ that identifies that entity _uniquely_ across the database. One common primary key type is an auto-incrementing integer (because they are space efficient), but it can also be a string, hashed value, so long as it is unique.

Using the `JOIN` clause in a query, we can combine row data across two separate tables using this unique key. The first of the joins that we will introduce is the `INNER JOIN`.

Select query with INNER JOIN on multiple tables

```sql
SELECT column, another_table_column, … 
FROM mytable 
INNER JOIN another_table 
	ON mytable.id = another_table.id 
WHERE condition(s) 
ORDER BY column, … ASC/DESC 
LIMIT num_limit OFFSET num_offset;
```

The `INNER JOIN` is a process that matches rows from the first table and the second table which have the same key (as defined by the `ON` constraint) to create a result row with the combined columns from both tables.

### OUTER JOINs

Depending on how you want to analyze the data, the `INNER JOIN` we used last lesson might not be sufficient because the resulting table only contains data that belongs in both of the tables.

If the two tables have asymmetric data, which can easily happen when data is entered in different stages, then we would have to use a `LEFT JOIN`, `RIGHT JOIN` or `FULL JOIN` instead to ensure that the data you need is not left out of the results.

Select query with `LEFT/RIGHT/FULL JOINs` on multiple tables

```sql
SELECT column, another_column, … 
FROM mytable 
INNER/LEFT/RIGHT/FULL JOIN another_table 
	ON mytable.id = another_table.matching_id
WHERE condition(s) 
ORDER BY column, … ASC/DESC LIMIT num_limit OFFSET num_offset;
```

Like the `INNER JOIN` these three new joins have to specify which column to join the data on.  
When joining table A to table B, a `LEFT JOIN` simply includes rows from A regardless of whether a matching row is found in B. The `RIGHT JOIN` is the same, but reversed, keeping rows in B regardless of whether a match is found in A. Finally, a `FULL JOIN` simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table.