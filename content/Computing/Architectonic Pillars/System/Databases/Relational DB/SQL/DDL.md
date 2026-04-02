---
---
Data types

1. Boolean 
2. Char
3. Numeric 
4. Temporal 
5. UUID 
6. Array 
7. JSON 
8. Hstore Key-value pair  

Phone No. 

* Makes more sense to store it as a VARCHAR 
* we do not perform airthemetic operations on phone no. 
* Also, in number datatype, 07 and 7 is same. 

Note:

* Google search best datatype for storage 
* Also refer to documentation of database

![[Screenshot from 2022-04-18 15-43-07.png]]

Constraints 

1. Two Main C
	1. Table 
	2. Column 

Table C 

* CHECK - check a condn when inserting or updating data 
* REFERENCES -  constrain the value stored in the column that must exist in a col in another table. 
* UNQIUE (col1, col2, col3....)
* PK (col1, col2, col3, col4.....) 

Column C

1. two imp C 
	1. NOT NULL 
	2. UNIQUE 
2. Other imp C 
	1. PK - uniquely identify each row 
	2. FK 
3. Check - all values in a col satisfy certain conditions 
4. Exclusion - 

* * *

CREATE 
![[Screenshot from 2022-04-18 17-08-30.png]]

![[Screenshot from 2022-04-18 17-10-30.png]]
![[Screenshot from 2022-04-18 17-12-08.png]]
