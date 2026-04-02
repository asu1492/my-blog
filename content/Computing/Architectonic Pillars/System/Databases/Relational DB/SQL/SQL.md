---
source: https://app.slack.com/client/T022QFTP49L/C02V7HRCJE6
---
What is SQL 

* SQL is language used to query data from relational db. 

Basic SQL Commands

1. Create a table 
2. Insert
3. Select 
4. Update
5. Delete 

* * *

**SELECT statement** 

1. It is data manipulation language(DML) statement used to read and modify data 
2. All Columns 
	1. Select \* from tableName; 
3. Selected columns 
	1. Select column1, column2 from tableName;
4. Restricting the result set
	1. Where clause 
	2. always require a Predicate 
	3. Select col1, col2 from tableName WHERE predicate 
	4. ![[image (15).png]]

* * *

**Count** 

1. built in fun that retrieves no of rows matching query criteria 
2. SELECT COUNT(\*) from tableName; 
3. Select count(columnName) from tableName where coloumnName=' '

* * *

**Distinct** 

1. remove duplicate values from the result set and retrieve unique values in a column. 
2. Select DISTINCT columnName from tableName; 
3. Select DISTINCT country from Medals where medaltype ='Gold';

```
Retrieve the number of release years of the films distinctly, produced by Warner Bros. Pictures

SELECT Count(Distinct ReleaseYear) FROM FilmLocations
WHERE ProductionCompany = 'Warner Bros. Pictures';
```

* * *

**Limit** 

1. Used to restrict no of rows from the db. 
2. SELECT \* from tableName LIMIT 10; 

```
Retrieve the first 15 rows from the "FilmLocations" table starting from row 11.

SELECT * FROM FilmLocations LIMIT 15 OFFSET 10;
```

* * *

**Insert**

1. is one of the DML(data manipulation language) used to read and modify data
2. single insert statement can be used to insert one or multiple rows in a table.  
3. INSERT INTO TableName(C1, C2, C5)  Values('Abhi', 2, 87%) ('Shubi', 5, 97%)  

* * *

**Update**

1. DML 
2. UPDATE TableName SET Column1="Value"  Column2 = "vlaue2" WHERE columnValue="A2"
3. In update statement, if we do not specify Where clause, all the rows in the table are updated. 

* * *

**Delete**

1. DML 
2. DELETE FROM TableName Where ColumnName IN ('colval1', 'colval2'); 
3. If we do not specify where clause, all the rows n the table is removed.
