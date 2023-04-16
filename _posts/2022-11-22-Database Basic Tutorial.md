---
layout: post
title: Database Basic Tutorial
date: 2022-11-20
author: Yancy
tags: [Database]
comments: true
---
## Basic

comment: `--comment.....` `/*.........*/`

create a database: `CREATE DATABASE database_name;`

create a table: ``

```
CREATE TABLE table_name (column1_name data_type constraints....);
```

data type:

| Type      | Description                       |
| --------- | --------------------------------- |
| INT       | -2^32 ~ 2^32-1                    |
| DECIMAL   |                                   |
| CHAR      | fixed length, max 225 byte string |
| VARCHAR   | elastic length string             |
| TEXT      | max 65535 byte string             |
| DATE      | YYYY-MM-DD                        |
| DATETIME  | YYYY-MM-DD HH:MM:SS               |
| TIMESTAMP | store the timestamp               |

### SELECT

select distinct: remove the duplicate rows

line alias: don't use in where! but ok in having, group by,order by

```
SELECT [column_1 | expression] AS descriptive_name / `descriptive name`
FROM table_name; 
```

table alias: `table_name (AS) table_alias `

### ORDER BY

ASC increase; DESC decrease

```
SELECT column1, column2,...
FROM tbl
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC],... 
```

customize order: 

```
ORDER BY FIELD(column_name, val1, val2, val3, ...); 
```

### FIELD()

return the index of the value

`FIELD(value, val1, val2, val3, ...)`

### WHERE

`WHERE search_condition; `

IN: `WHERE (expr|column_1) IN ('value1','value2',...); `

BETWEEN: `expr [NOT] BETWEEN begin_expr AND end_expr; `

LIKE: `expression LIKE pattern ESCAPE escape_character `

- 百分号（`%`）通配符匹配任何零个或多个字符的字符串。
- 下划线（`_`）通配符匹配任何单个字符。

ISNULL:`value IS NULL `

IFNULL: `IFNULL(expression, alt_value)`

### LIMIT

`LIMIT offset , count; `the first n items or nth item

### JOIN

CROSS JOIN: all combinations

(INNER) JOIN: intersection part

```
SELECT column_list
FROM t1
INNER JOIN t2 ON join_condition; / USING (column)---if same name
```

LEFT JOIN: left table complete

RIGHT JOIN: right table complete

### GROUP BY

after from and where

HAVING: filter for inner group, using with group by

roll up: generate the sum up for the group by items.  

group by a, b, c rollup === group by a, b,c 

group by 1: the first select item

### CONCAT

CONCAT (str1, str2,...)

group_concat( distinct 要连接的字段 order by 排序字段 asc/desc   separator '分隔符' )

### UNION

row union, don't use or, use union instead to enhance the efficiency

### DELETE, DROP, TRUNCATE

 `DELETE FROM table_name WHERE condition;`

### EXIST

```
SELECT 
    select_list
FROM
    a_table
WHERE
    [NOT] EXISTS(subquery); 
```

using exists to check the subquery. If null, return false.

the efficiency of in and exist: in MySQL 8.0, no significant difference

UPDATE

alter



### window function

rank: rank(), dense_rank(), row_number()

``` 
window_function_name(expression) 
    OVER (
        [partition_defintion]
        [order_definition]
        [frame_definition]
    ) 
```

### case

``` 
CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE; 
```

``` 
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE; 
```
