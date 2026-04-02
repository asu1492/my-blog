---
---
<https://docs.google.com/spreadsheets/d/1Jr9mdVUlmL3BPa-vSl5IBZFCQ48zTxTkbvbLxMRDUQk/edit#gid=614167753>

|     |     |
| --- | --- |
| SELECT \* | prefer not to use, will increase traffic b/w db server and the application |
| SELECT DISTINCT(column\_name) | parentesis is optional, yet preferable for readability |
| SELECT COUNT(column\_name)<br><br>SELECT COUNT(DISTINCT column\_name) | Count is a function |
| SELECT col\_name FROM table\_name <br>WHERE col\_name condition | Where statement enable queries based on conditions such as use of comparison operators(=,<,>) <br><br>cannot use WHERE with aggregate function -- HAVING comes into picture |
| Order By<br><br>SELECT col\_name FROM table\_name <br>WHERE col\_name condition <br>Order By col\_name1, col\_name2 ASC/DESC <br><br>or <br><br>ORDER BY col\_nam1 ASC, col\_nam2 DESC | By default, order is ascending. |
| LIMIT | to get the general layout of the table |
| BETWEEN <br>  dates BETWEEN YYYY-MM-DD AND YYYY-MM-DD (ISO 8601 standard)<br><br>NOT BETWEEN |     |
| IN <br><br>SELECT colour FROM tableName <br>WHERE colour IN ('red', 'blue', 'green')<br><br>NOT IN | enable putting together bunch of ORs |
| LIKE(case sensitive) vs ILIKE(not case sensitive)<br><br>Underscore -- WHERE Value LIKE 'Version\_ \_'<br><br>WHERE name LIKE '\_her%' | ![[Computer Science/Databases/_resources/DML.resources/unknown_filename.4.png]] |
| REGEX | <https://www.postgresql.org/docs/current/functions-matching.html><https://www.postgresql.org/docs/9.5/functions-aggregate.html> |
| Aggregate Function <br><br>* only happen in SELECT or HAVING CLAUSE <br><br>Can use ROUND() with AVG() function to specify precision after the decimal <br><br>MIN(Column\_Name) <br><br>SELECT ROUND (AVG(col\_name), 2) FROM tableName ; | ![[Computer Science/Databases/_resources/DML.resources/unknown_filename.3.png]] |
| AS  | get executed at the very end of a query -- cannot uses alias inside a where operator |

**Comparison Operators** 
![[Computer Science/Databases/_resources/DML.resources/unknown_filename.png]]

Logical Operators 
![[Computer Science/Databases/_resources/DML.resources/unknown_filename.2.png]]![[Computer Science/Databases/_resources/DML.resources/unknown_filename.1.png]]

GROUP BY 

1. We need categorical columns which are non continuous 

|     |     |
| --- | --- |
| SELECT col\_name from tableName <br>GROUP BY col\_name ; | Result will be similar to distinct operations |

HAVING

1. it allows us to filter after an aggregation has already taken place 

DATE

|     |     |
| --- | --- |
| DATE(time stamp) | Changes time stamp to a date |
|     |     |

![[Screenshot from 2022-04-18 17-27-01.png]]

![[Screenshot from 2022-04-18 17-32-50.png]]

The `CASE` expression evaluates to a value, i.e. it is used to evaluate to one of a set of results, based on some condition.

Example:
```
SELECT CASE
    WHEN type = 1 THEN 'foo'
    WHEN type = 2 THEN 'bar'
    ELSE 'baz'
END AS name_for_numeric_type
FROM sometable`


```
The `CASE` statement executes one of a set of statements, based on some condition.
Example:
```
CASE
    WHEN action = 'update' THEN
        UPDATE sometable SET column = value WHERE condition;
    WHEN action = 'create' THEN
        INSERT INTO sometable (column) VALUES (value);
END CASE


```

You see how they are similar, but the statement _does not_ evaluate to a value and can be used on its own, while the expression needs to be a part of an expression, e.g. a query or an assignment. You cannot use the statement in a query, since a query cannot contain statements, only expressions that need to evaluate to something (the query itself is a statement, in a way), e.g. `SELECT CASE WHEN condition THEN UPDATE table SET something; END CASE` makes no sense.
