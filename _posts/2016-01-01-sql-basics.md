---
layout: post
title: SQL basics
date: 2016-01-01 13:50:00 +0800
author: Yuan Jiang
tags: sql
---

SQL basics.

## What SQL does
- Insert/Delete/Update/Select
- Create DB/Table/View
- Create Stored Procedures
- Set Permissions

## SQL knowledge
- Case insensitive
- Semicolon after statement (some optional)

## Most common statements
- SELECT
- UPDATE
- DELETE
- INSERT INTO
- CREATE DATABASE
- ALTER DATABASE
- CREATE TABLE
- ALTER TABLE
- DROP TABLE
- CREATE INDEX
- DROP INDEX

## SELECT statement
{% highlight sql %}
SELECT * FROM table_name;

SELECT column_name,column_name
FROM table_name;

SELECT DISTINCT column_name,column_name
FROM table_name;
{% endhighlight %}

## WHERE clause
{% highlight sql %}
SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;

operators:
=           => equal
<>          => not equal (!=)
>           => greater than
<           => less than
>=          => greater than or equal
<=          => less than or equal
BETWEEN     => between an inclusive range
LIKE        => search for a pattern
IN          => specify multiple possible values for a column

AND         => both condition true
OR          => either condition true
{% endhighlight %}

## ORDER BY keyword
{% highlight sql %}
SELECT column_name,column_name
FROM table_name
ORDER BY column_name ASC|DESC, column_name ASC|DESC;
{% endhighlight %}

## INSERT INTO statement
{% highlight sql %}
INSERT INTO table_name
VALUES (value1,value2,value3,...);

INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
{% endhighlight %}

## UPDATE statement
{% highlight sql %}
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
{% endhighlight %}

## DELETE statement
{% highlight sql %}
DELETE FROM table_name
WHERE some_column=some_value;

=> delete all:
DELETE FROM table_name;
or
DELETE * FROM table_name;
{% endhighlight %}

## SELECT TOP clause
{% highlight sql %}
=> ms sqlserver/access style:
SELECT TOP number/percent column_name(s)
FROM table_name

=> mysql style:
SELECT column_name(s)
FROM table_name
LIMIT number;

=> oracle style:
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
{% endhighlight %}

## Common operators
{% highlight sql %}
=> LIKE operator
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;

=> NOT LIKE operator
SELECT column_name(s)
FROM table_name
WHERE column_name NOT LIKE pattern;

=> WILDCARDS chars:
%            => zero or more chars
_            => single char
[charlist]   => sets and ranges of chars
[^charlist]  => not within sets or ranges of chars
[!charlist]  => same as [^charlist]

=> IN operator
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);

=> BETWEEN operator
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;

=> NOT BETWEEN operator
SELECT column_name(s)
FROM table_name
WHERE column_name NOT BETWEEN value1 AND value2;
{% endhighlight %}

## Aliases
{% highlight sql %}
SELECT column_name AS alias_name
FROM table_name;

SELECT column_name(s)
FROM table_name AS alias_name;
{% endhighlight %}

## JOINS
{% highlight sql %}
=> INNER JOIN
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;

SELECT column_name(s)
FROM table1
JOIN table2
ON table1.column_name=table2.column_name;

=> LEFT JOIN
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;

SELECT column_name(s)
FROM table1
LEFT OUTER JOIN table2
ON table1.column_name=table2.column_name;

=> RIGHT JOIN
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;

SELECT column_name(s)
FROM table1
RIGHT OUTER JOIN table2
ON table1.column_name=table2.column_name;

=> FULL OUTER JOIN
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

## UNION operator
{% highlight sql %}
=> only distinct results
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2

=> with duplicate results
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2
{% endhighlight %}

## SELECT INTO statement
{% highlight sql %}
SELECT *
INTO newtable [IN externaldb]
FROM table1;

SELECT column_name(s)
INTO newtable [IN externaldb]
FROM table1;
{% endhighlight %}

## INSERT INTO SELECT statement
{% highlight sql %}
INSERT INTO table2
SELECT * FROM table1;

INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
{% endhighlight %}

## CREATE statement
{% highlight sql %}
=> create db:
CREATE DATABASE dbname;

=> create table:
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
...
);

constraints can be:
- NOT NULL
- UNIQUE
- PRIMARY KEY (AUTO_INCREMENT)
- FOREIGN KEY (REFERENCES)
- CHECK
- DEFAULT
{% endhighlight %}

## CREATE INDEX statement
{% highlight sql %}
CREATE INDEX index_name
ON table_name (column_name)

CREATE UNIQUE INDEX index_name
ON table_name (column_name)
{% endhighlight %}

## DROP statement
{% highlight sql %}
=> drop index
DROP INDEX index_name ON table_name (access)
DROP INDEX table_name.index_name (sqlserver)
DROP INDEX index_name (db2/oracle)
ALTER TABLE table_name DROP INDEX index_name (mysql)

=> drop table
DROP TABLE table_name

=> drop db
DROP DATABASE db_name

=> delete table data only
TRUNCATE TABLE table_name
{% endhighlight %}

## CREATE VIEW statement
{% highlight sql %}
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition

DROP VIEW view_name
{% endhighlight %}

## Data types
{% highlight sql %}
=> null type:
IS NULL     => test if column is null
IS NOT NULL => test if column is not null
=> general types:
- CHARACTER
- VARCHAR
- BINARY
- BOOLEAN
- INTEGER
- BIGINT
- NUMERIC
- FLOAT
- DATE
- TIME
- TIMESTAMP
- etc.
{% endhighlight %}

## References
- [SQL tutorial](http://www.w3schools.com/sql/default.asp)
